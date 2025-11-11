Background tasks in React Native represent one of the most nuanced and powerful aspects of mobile development, yet they're also one of the most misunderstood. Let me guide you through this complex landscape by starting with the fundamental concepts and building toward sophisticated implementations.

## Understanding the Mobile Background Execution Model

Before diving into code, we need to understand how mobile operating systems think about background execution. Imagine your phone as a careful resource manager that's constantly balancing user experience against battery life and performance. When an app moves to the background, the operating system becomes increasingly restrictive about what that app can do.

This isn't arbitrary limitation - it's intelligent resource management. Consider what would happen if every app on your phone continued running at full capacity when backgrounded. Your battery would drain rapidly, your phone would heat up, and foreground apps would become sluggish. The operating system prevents this by implementing a tiered system of background execution privileges.

Think of it like a library with different reading rooms. The main reading room (foreground) has full lighting and resources. The quiet study area (background with limited execution time) has dimmed lights and basic amenities. The storage area (suspended apps) has minimal lighting but preserves your place. Understanding this hierarchy helps you choose the right approach for your background tasks.

## Categories of Background Tasks

Background tasks in React Native fall into several distinct categories, each with different capabilities and limitations. Let's explore these systematically, starting with the most straightforward approaches.

**App State Change Handling** represents the foundation of background task management. Every background task strategy begins with detecting when your app transitions between foreground and background states. This is like having a doorbell that alerts you when someone enters or leaves your house.

```javascript
import { AppState } from 'react-native';
import { useEffect, useRef } from 'react';

function useAppStateAware() {
  const appState = useRef(AppState.currentState);
  
  useEffect(() => {
    // This function runs whenever the app state changes
    const handleAppStateChange = (nextAppState) => {
      console.log('App state changing from', appState.current, 'to', nextAppState);
      
      if (appState.current === 'active' && nextAppState.match(/inactive|background/)) {
        // App is going to background - start your background tasks here
        console.log('App is going to background');
        handleAppGoingToBackground();
      }
      
      if (appState.current.match(/inactive|background/) && nextAppState === 'active') {
        // App is coming to foreground - clean up background tasks
        console.log('App is coming to foreground');
        handleAppComingToForeground();
      }
      
      appState.current = nextAppState;
    };
    
    // Subscribe to app state changes
    const subscription = AppState.addEventListener('change', handleAppStateChange);
    
    // Clean up the subscription when component unmounts
    return () => subscription?.remove();
  }, []);
  
  return appState.current;
}
```

This foundation gives you awareness of app transitions, but awareness alone isn't enough. You need mechanisms to actually perform work during these transitions.

**Limited-Time Background Execution** provides a brief window where your app can continue running after backgrounding. Think of this like having a few minutes to finish tidying up after guests leave your party. The operating system grants you a small amount of time to complete essential tasks before fully suspending your app.

```javascript
import BackgroundTimer from 'react-native-background-timer';

function handleAppGoingToBackground() {
  // Request additional execution time from the operating system
  // This gives you roughly 30 seconds on iOS, variable time on Android
  const backgroundTaskId = BackgroundTimer.start(() => {
    // This code runs even when the app is backgrounded
    console.log('Background task is running');
    
    // Perform your essential background work here
    // Examples: sync critical data, clean up temporary files, save user progress
    syncCriticalUserData()
      .then(() => {
        console.log('Critical data sync completed');
      })
      .catch((error) => {
        console.log('Background sync failed:', error);
      })
      .finally(() => {
        // Always clean up your background task
        BackgroundTimer.stop(backgroundTaskId);
      });
  });
  
  // Set a safety timeout to ensure we don't exceed allowed time
  setTimeout(() => {
    BackgroundTimer.stop(backgroundTaskId);
    console.log('Background task stopped due to timeout');
  }, 25000); // Stop before the system force-kills us
}
```

This approach works well for quick tasks like saving user progress, sending analytics events, or uploading critical data. However, it's not suitable for longer-running operations like file downloads or continuous data synchronization.

## True Background Processing with Background Jobs

For tasks that need to run for extended periods or on a schedule, you need dedicated background job libraries. These libraries negotiate with the operating system to register your app for specific types of background execution privileges.

**React Native Background Job** provides a robust foundation for longer-running background tasks. Think of this like hiring a dedicated assistant who can work even when you're not in the office, but who needs specific instructions about what they're allowed to do.

```javascript
import BackgroundJob from 'react-native-background-job';

function setupLongRunningBackgroundTask() {
  // Configure the background job with specific parameters
  const backgroundOptions = {
    taskName: 'DataSync',
    taskKey: 'dataSync',
    // Define what work should happen in the background
    taskFn: () => {
      console.log('Long-running background task started');
      
      // This function runs in a separate context, so it needs to be self-contained
      // You can't access React state or props from here
      performDataSynchronization()
        .then(() => {
          console.log('Background data sync completed successfully');
          // You might want to store results in AsyncStorage for the main app to read
          return AsyncStorage.setItem('lastSyncTime', new Date().toISOString());
        })
        .catch((error) => {
          console.log('Background sync encountered error:', error);
          // Log errors for debugging, but don't crash the background task
          return AsyncStorage.setItem('lastSyncError', error.message);
        });
    }
  };
  
  // Start the background job
  BackgroundJob.start(backgroundOptions);
  
  // You can stop the background job when it's no longer needed
  return () => {
    BackgroundJob.stop({taskKey: 'dataSync'});
  };
}

// Example of a self-contained background function
async function performDataSynchronization() {
  // Background tasks often need to work with stored data since they can't access app state
  const userToken = await AsyncStorage.getItem('userToken');
  const pendingUploads = await AsyncStorage.getItem('pendingUploads');
  
  if (!userToken || !pendingUploads) {
    console.log('No sync needed - missing token or pending data');
    return;
  }
  
  // Perform your actual synchronization work
  const response = await fetch('https://api.yourservice.com/sync', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${userToken}`,
      'Content-Type': 'application/json'
    },
    body: pendingUploads
  });
  
  if (response.ok) {
    // Clear the pending uploads since they're now synced
    await AsyncStorage.removeItem('pendingUploads');
  } else {
    throw new Error(`Sync failed with status: ${response.status}`);
  }
}
```

Notice how background tasks operate in isolation from your main app. They can't access React state, props, or context. This isolation is intentional - it ensures that background tasks remain lightweight and don't interfere with your app's main functionality.

## Scheduled Background Tasks

Some background work needs to happen on a schedule rather than in response to app state changes. This is like setting up automatic maintenance routines that run whether you're actively using the app or not.

```javascript
import BackgroundTimer from 'react-native-background-timer';
import AsyncStorage from '@react-native-async-storage/async-storage';

class ScheduledTaskManager {
  constructor() {
    this.tasks = new Map();
    this.isRunning = false;
  }
  
  // Register a task to run on a specific interval
  registerTask(taskId, taskFunction, intervalMinutes) {
    const task = {
      id: taskId,
      function: taskFunction,
      interval: intervalMinutes * 60 * 1000, // Convert to milliseconds
      lastRun: 0
    };
    
    this.tasks.set(taskId, task);
    console.log(`Registered background task: ${taskId}`);
  }
  
  // Start the task scheduler
  start() {
    if (this.isRunning) {
      console.log('Task scheduler already running');
      return;
    }
    
    this.isRunning = true;
    
    // Check for due tasks every minute
    this.schedulerInterval = BackgroundTimer.setInterval(() => {
      this.checkAndRunDueTasks();
    }, 60000); // Check every minute
    
    console.log('Background task scheduler started');
  }
  
  // Stop the task scheduler
  stop() {
    if (!this.isRunning) return;
    
    BackgroundTimer.clearInterval(this.schedulerInterval);
    this.isRunning = false;
    console.log('Background task scheduler stopped');
  }
  
  // Check which tasks are due and run them
  async checkAndRunDueTasks() {
    const currentTime = Date.now();
    
    for (const [taskId, task] of this.tasks) {
      const timeSinceLastRun = currentTime - task.lastRun;
      
      if (timeSinceLastRun >= task.interval) {
        console.log(`Running scheduled task: ${taskId}`);
        
        try {
          await task.function();
          task.lastRun = currentTime;
          
          // Persist the last run time so it survives app restarts
          await AsyncStorage.setItem(
            `lastRun_${taskId}`, 
            currentTime.toString()
          );
        } catch (error) {
          console.log(`Task ${taskId} failed:`, error);
          // Don't update lastRun time if the task failed
          // This ensures it will retry on the next check
        }
      }
    }
  }
  
  // Restore last run times from storage (call this when app starts)
  async restoreTaskHistory() {
    for (const [taskId, task] of this.tasks) {
      try {
        const lastRunString = await AsyncStorage.getItem(`lastRun_${taskId}`);
        if (lastRunString) {
          task.lastRun = parseInt(lastRunString, 10);
        }
      } catch (error) {
        console.log(`Could not restore history for task ${taskId}:`, error);
      }
    }
  }
}

// Usage example
const taskManager = new ScheduledTaskManager();

// Register various background tasks
taskManager.registerTask('dataCleanup', async () => {
  // Clean up old cached data
  await cleanupOldCacheFiles();
}, 60); // Run every hour

taskManager.registerTask('healthCheck', async () => {
  // Ping your server to maintain connection
  await performHealthCheck();
}, 15); // Run every 15 minutes

taskManager.registerTask('dataBackup', async () => {
  // Backup important user data
  await backupUserData();
}, 1440); // Run daily (24 hours * 60 minutes)

// Start the scheduler when app becomes active
function startBackgroundTasks() {
  taskManager.restoreTaskHistory()
    .then(() => {
      taskManager.start();
    });
}
```

This scheduler provides a foundation for complex background task management. You can extend it with features like task priorities, retry logic, or conditional execution based on device state (charging, WiFi availability, etc.).

## Push Notifications as Background Triggers

Push notifications offer a unique way to trigger background work. When a push notification arrives, your app gets a brief window to process the notification and potentially update its state, even if the app isn't running.

```javascript
import messaging from '@react-native-firebase/messaging';
import AsyncStorage from '@react-native-async-storage/async-storage';

function setupPushNotificationBackgroundHandler() {
  // This handler runs when a push notification is received while app is in background
  messaging().setBackgroundMessageHandler(async (remoteMessage) => {
    console.log('Message handled in the background!', remoteMessage);
    
    // You have limited time here (about 30 seconds) to process the notification
    // Common use cases: update local data, download content, prepare UI updates
    
    if (remoteMessage.data.type === 'new_message') {
      // Download and cache the new message so it's ready when user opens the app
      await downloadAndCacheMessage(remoteMessage.data.messageId);
    } else if (remoteMessage.data.type === 'sync_trigger') {
      // Perform a quick data sync
      await performQuickDataSync();
    } else if (remoteMessage.data.type === 'content_update') {
      // Pre-download updated content
      await downloadContentUpdate(remoteMessage.data.contentUrl);
    }
    
    // Update a badge count or other UI elements
    await updateAppBadgeCount(remoteMessage.data.badgeCount);
  });
}

async function downloadAndCacheMessage(messageId) {
  try {
    // Fetch the message details from your server
    const userToken = await AsyncStorage.getItem('userToken');
    const response = await fetch(`https://api.yourapp.com/messages/${messageId}`, {
      headers: {
        'Authorization': `Bearer ${userToken}`
      }
    });
    
    if (response.ok) {
      const messageData = await response.json();
      
      // Store the message locally so it's immediately available when user opens the app
      const existingMessages = await AsyncStorage.getItem('cachedMessages');
      const messages = existingMessages ? JSON.parse(existingMessages) : [];
      messages.unshift(messageData); // Add to beginning of array
      
      // Keep only the most recent 50 messages to avoid storage bloat
      const trimmedMessages = messages.slice(0, 50);
      await AsyncStorage.setItem('cachedMessages', JSON.stringify(trimmedMessages));
      
      console.log('Successfully cached new message');
    }
  } catch (error) {
    console.log('Failed to download and cache message:', error);
    // Don't throw - we don't want to crash the background handler
  }
}
```

This approach creates a responsive user experience where content is often ready before users even know they need it. When they open your app after receiving a notification, the relevant data is already cached and immediately available.

## Handling Background Task Limitations and Edge Cases

Background tasks come with numerous platform-specific limitations that you need to navigate carefully. Understanding these constraints helps you design robust solutions that work reliably across different devices and operating system versions.

**iOS Background App Refresh** represents one of the most significant constraints. Users can disable background app refresh globally or for individual apps. Your background tasks need to gracefully handle situations where they're not allowed to run.

```javascript
import { Alert, Linking, Platform } from 'react-native';
import BackgroundTimer from 'react-native-background-timer';

class BackgroundTaskManager {
  constructor() {
    this.isBackgroundRefreshEnabled = true;
    this.fallbackStrategies = [];
  }
  
  // Check if background execution is available
  async checkBackgroundPermissions() {
    if (Platform.OS === 'ios') {
      // On iOS, you can't directly check background app refresh status
      // But you can test if background tasks actually run
      return new Promise((resolve) => {
        let testCompleted = false;
        
        // Start a test background task
        const testTaskId = BackgroundTimer.start(() => {
          if (!testCompleted) {
            testCompleted = true;
            this.isBackgroundRefreshEnabled = true;
            BackgroundTimer.stop(testTaskId);
            resolve(true);
          }
        });
        
        // If the test doesn't complete within 2 seconds, assume background refresh is disabled
        setTimeout(() => {
          if (!testCompleted) {
            testCompleted = true;
            this.isBackgroundRefreshEnabled = false;
            BackgroundTimer.stop(testTaskId);
            resolve(false);
          }
        }, 2000);
      });
    }
    
    // Android has different permission models
    return true; // Simplified for this example
  }
  
  // Gracefully handle situations where background tasks can't run
  async executeTaskWithFallback(primaryTask, fallbackStrategy) {
    const canRunInBackground = await this.checkBackgroundPermissions();
    
    if (canRunInBackground) {
      try {
        await primaryTask();
        console.log('Background task completed successfully');
      } catch (error) {
        console.log('Background task failed, using fallback:', error);
        await fallbackStrategy();
      }
    } else {
      console.log('Background execution not available, using fallback strategy');
      await this.promptUserForBackgroundPermission();
      await fallbackStrategy();
    }
  }
  
  // Guide users to enable background app refresh
  promptUserForBackgroundPermission() {
    Alert.alert(
      'Background Updates Disabled',
      'To keep your data synchronized, please enable Background App Refresh in Settings.',
      [
        { text: 'Cancel', style: 'cancel' },
        {
          text: 'Open Settings',
          onPress: () => {
            if (Platform.OS === 'ios') {
              Linking.openURL('app-settings:');
            } else {
              // Android users need to navigate to battery optimization settings
              Linking.openSettings();
            }
          }
        }
      ]
    );
  }
}

// Example usage with fallback strategies
const backgroundManager = new BackgroundTaskManager();

async function syncDataWithFallback() {
  const primaryTask = async () => {
    // Attempt to sync data in the background
    await performFullDataSync();
  };
  
  const fallbackStrategy = async () => {
    // Store sync request for next time app becomes active
    await AsyncStorage.setItem('pendingSyncRequest', JSON.stringify({
      timestamp: Date.now(),
      type: 'full_sync'
    }));
    
    // Or schedule a local notification to remind user to open the app
    scheduleDataSyncReminder();
  };
  
  await backgroundManager.executeTaskWithFallback(primaryTask, fallbackStrategy);
}
```

This defensive approach ensures your app continues to function even when background execution isn't available, while gently encouraging users to enable the features that improve their experience.

## Building a Complete Background Task System

Let's bring together all these concepts into a comprehensive background task system that handles various scenarios and provides a clean API for your app to use.This comprehensive background task system brings together all the concepts we've explored into a production-ready solution. Think of it as a sophisticated orchestra conductor that coordinates different types of background work, ensuring each task plays at the right time and in harmony with your app's lifecycle.

## Understanding the System Architecture

The architecture follows a modular design where the `BackgroundTaskManager` acts as the central coordinator. Like a well-organized office manager, it handles task registration, scheduling, execution, error recovery, and resource cleanup. Each task type gets specialized handling based on its unique requirements and constraints.

The system recognizes four distinct task types, each optimized for different scenarios. Immediate tasks respond to app state changes, scheduled tasks run on regular intervals, push-triggered tasks activate when specific notifications arrive, and one-time tasks handle delayed operations. This categorization helps you choose the right approach for each piece of background work.

Notice how the system includes sophisticated error handling with exponential backoff retry logic. When tasks fail, the system doesn't give up immediately. Instead, it waits progressively longer between retry attempts, preventing your app from overwhelming network resources or battery life during temporary failures.

## Implementing the System in Your App

To integrate this background task system into your React Native application, start by initializing it during your app's startup sequence. Think of this initialization as setting up the foundation before building the rest of your house. Here's how you might structure this in your main App component:

```javascript
import React, { useEffect } from 'react';
import backgroundTaskManager, { TASK_TYPES } from './BackgroundTaskManager';

function App() {
  useEffect(() => {
    const initializeBackgroundTasks = async () => {
      // Initialize the background task system
      await backgroundTaskManager.initialize();
      
      // Register your app's specific background tasks
      await registerAppBackgroundTasks();
      
      console.log('Background task system ready');
    };
    
    initializeBackgroundTasks();
    
    // Cleanup when app unmounts
    return () => {
      backgroundTaskManager.cleanup();
    };
  }, []);
  
  // Rest of your app component
  return (
    // Your app's UI components
  );
}

async function registerAppBackgroundTasks() {
  // Register immediate tasks for critical data sync
  backgroundTaskManager.registerTask(
    'criticalDataSync',
    TASK_TYPES.IMMEDIATE,
    async (context) => {
      await syncCriticalUserData();
      await uploadPendingAnalytics();
    },
    {
      maxRetries: 3
    }
  );
  
  // Register scheduled maintenance tasks
  backgroundTaskManager.registerTask(
    'maintenanceCleanup',
    TASK_TYPES.SCHEDULED,
    async (context) => {
      await cleanupOldCacheFiles();
      await optimizeLocalDatabase();
    },
    {
      interval: 2 * 60 * 60 * 1000, // Every 2 hours
      enabled: true
    }
  );
  
  // Register push notification handlers
  backgroundTaskManager.registerTask(
    'messageHandler',
    TASK_TYPES.PUSH_TRIGGERED,
    async (context) => {
      await refreshMessageList();
      await updateNotificationBadge();
    },
    {
      pushTriggers: [
        'new_message',
        'friend_request',
        // You can also use custom trigger functions
        (notification) => notification.data?.priority === 'high'
      ]
    }
  );
}
```

Notice how we separate the task registration into its own function. This organization makes it easier to manage different types of background work and modify them as your app evolves.

## Best Practices for Background Task Implementation

When writing the actual task functions, remember that they operate in a constrained environment. Background tasks can't access React state or context, so they must be self-contained and rely on persistent storage for any data they need.

Structure your task functions to be resilient and efficient. Start with the most critical operations first, since background execution time is limited. For example, prioritize uploading crash reports or critical user data before performing optional maintenance tasks.

```javascript
// Example of a well-structured background task function
async function syncCriticalUserData() {
  try {
    // Step 1: Get authentication token from persistent storage
    const userToken = await AsyncStorage.getItem('userToken');
    if (!userToken) {
      console.log('No user token available, skipping sync');
      return;
    }
    
    // Step 2: Get pending data that needs to be uploaded
    const pendingData = await AsyncStorage.getItem('pendingUserData');
    if (!pendingData) {
      console.log('No pending data to sync');
      return;
    }
    
    // Step 3: Attempt the sync with timeout protection
    const syncPromise = fetch('https://api.yourapp.com/sync', {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${userToken}`,
        'Content-Type': 'application/json'
      },
      body: pendingData
    });
    
    // Set a reasonable timeout for the network request
    const timeoutPromise = new Promise((_, reject) => 
      setTimeout(() => reject(new Error('Sync timeout')), 15000)
    );
    
    const response = await Promise.race([syncPromise, timeoutPromise]);
    
    if (response.ok) {
      // Step 4: Clear the pending data since sync succeeded
      await AsyncStorage.removeItem('pendingUserData');
      await AsyncStorage.setItem('lastSyncTime', new Date().toISOString());
      console.log('Critical data sync completed successfully');
    } else {
      throw new Error(`Sync failed with status: ${response.status}`);
    }
    
  } catch (error) {
    console.log('Critical data sync failed:', error.message);
    // Don't clear pending data on failure - let the retry mechanism handle it
    throw error; // Re-throw to trigger retry logic
  }
}
```

## Advanced Customization and Extension

The background task system provides several extension points for advanced use cases. You might want to add custom task prioritization, conditional execution based on device state, or integration with external monitoring systems.

For example, you could extend the system to consider device battery level, network type, or charging status when deciding whether to execute certain tasks. This creates a more intelligent system that adapts to user conditions:

```javascript
// Example extension for battery-aware task execution
async function shouldExecuteTask(task, context) {
  // Always execute critical immediate tasks
  if (task.type === TASK_TYPES.IMMEDIATE && task.options.critical) {
    return true;
  }
  
  // Check battery level for non-critical tasks
  try {
    const batteryLevel = await getBatteryLevel(); // You'd need to implement this
    
    if (batteryLevel < 0.2 && !task.options.lowBatteryAllowed) {
      console.log(`Skipping task ${task.id} due to low battery (${batteryLevel * 100}%)`);
      return false;
    }
    
    // Check network type for data-intensive tasks
    if (task.options.requiresWiFi) {
      const networkInfo = await getNetworkInfo(); // You'd need to implement this
      if (networkInfo.type !== 'wifi') {
        console.log(`Skipping task ${task.id} - requires WiFi but on ${networkInfo.type}`);
        return false;
      }
    }
    
    return true;
  } catch (error) {
    console.log('Could not check device conditions, allowing task execution');
    return true;
  }
}
```

## Monitoring and Debugging Background Tasks

Effective monitoring helps you understand how your background tasks perform in real-world conditions. The system includes built-in logging and status reporting, but you might want to enhance this with custom analytics or crash reporting integration.

Consider implementing a debugging dashboard that shows task execution history, failure rates, and performance metrics. This information becomes invaluable when optimizing your background task strategy or troubleshooting user-reported issues.

```javascript
// Example monitoring enhancement
function createTaskMonitor() {
  return {
    async logTaskExecution(taskId, duration, success, context) {
      const logEntry = {
        taskId,
        duration,
        success,
        context,
        timestamp: Date.now(),
        batteryLevel: await getBatteryLevel(),
        networkType: await getNetworkType()
      };
      
      // Store locally for debugging
      const existingLogs = await AsyncStorage.getItem('taskExecutionLogs');
      const logs = existingLogs ? JSON.parse(existingLogs) : [];
      logs.push(logEntry);
      
      // Keep only the most recent 100 log entries
      const trimmedLogs = logs.slice(-100);
      await AsyncStorage.setItem('taskExecutionLogs', JSON.stringify(trimmedLogs));
      
      // Optionally send to analytics service
      if (success) {
        analytics.track('BackgroundTaskSuccess', logEntry);
      } else {
        analytics.track('BackgroundTaskFailure', logEntry);
      }
    }
  };
}
```

## Handling Platform-Specific Considerations

Different mobile platforms have unique characteristics that affect background task execution. iOS tends to be more restrictive about background processing, while Android provides more flexibility but requires careful battery optimization handling.

The system includes basic platform detection and adaptation, but you might need to implement additional platform-specific strategies based on your app's requirements. For instance, iOS apps benefit from using Background App Refresh efficiently, while Android apps might need to handle doze mode and app standby optimizations.

Understanding these platform differences helps you set appropriate user expectations and design fallback strategies. When background execution isn't available, your app should gracefully degrade functionality while still providing value to users.

Background task implementation in React Native requires balancing user experience against system constraints. The comprehensive system we've built provides a solid foundation that handles the complexity while exposing a clean API for your app's specific needs. Start with simple immediate and scheduled tasks, then gradually add more sophisticated functionality as you understand your app's background processing requirements.

The key insight is that background tasks should enhance your app's experience without becoming a burden on users' devices. When implemented thoughtfully, they create the seamless, always-ready experience that modern mobile users expect. Would you like to explore any specific aspect of this system in more detail, such as testing strategies, performance optimization, or integration with particular types of data synchronization

```
/**
 * Comprehensive Background Task System for React Native
 * 
 * This system provides a unified way to handle different types of background tasks:
 * - Immediate tasks (run when app goes to background)
 * - Scheduled tasks (run on intervals)
 * - Push-triggered tasks (run when push notifications arrive)
 * - One-time delayed tasks
 * 
 * Features:
 * - Automatic retry logic with exponential backoff
 * - Platform-specific optimizations
 * - Graceful degradation when background execution isn't available
 * - Persistent task state across app restarts
 * - Clean API for registering and managing tasks
 */

import { AppState, Platform, Alert, Linking } from 'react-native';
import BackgroundTimer from 'react-native-background-timer';
import AsyncStorage from '@react-native-async-storage/async-storage';
import messaging from '@react-native-firebase/messaging';

// Task types that the system supports
const TASK_TYPES = {
  IMMEDIATE: 'immediate',    // Run when app goes to background
  SCHEDULED: 'scheduled',    // Run on regular intervals
  PUSH_TRIGGERED: 'push',    // Run when specific push notifications arrive
  ONE_TIME: 'one_time'       // Run once after a delay
};

// Task execution contexts
const EXECUTION_CONTEXTS = {
  BACKGROUND: 'background',
  FOREGROUND: 'foreground',
  PUSH_HANDLER: 'push_handler'
};

class BackgroundTaskManager {
  constructor() {
    this.tasks = new Map();
    this.isInitialized = false;
    this.isBackgroundRefreshEnabled = true;
    this.currentAppState = AppState.currentState;
    this.activeTimers = new Set();
    this.retryAttempts = new Map();
    
    // Configuration options
    this.config = {
      maxRetryAttempts: 3,
      baseRetryDelay: 1000, // 1 second
      maxBackgroundTime: 25000, // 25 seconds (safe buffer under iOS 30s limit)
      persistenceKey: 'backgroundTasks',
      maxConcurrentTasks: 3
    };
  }

  /**
   * Initialize the background task system
   * Call this once when your app starts up
   */
  async initialize() {
    if (this.isInitialized) {
      console.log('BackgroundTaskManager already initialized');
      return;
    }

    console.log('Initializing BackgroundTaskManager...');
    
    // Set up app state monitoring
    this.setupAppStateMonitoring();
    
    // Set up push notification background handler
    this.setupPushNotificationHandler();
    
    // Restore persisted tasks
    await this.restorePersistedTasks();
    
    // Check background execution permissions
    await this.checkBackgroundPermissions();
    
    this.isInitialized = true;
    console.log('BackgroundTaskManager initialized successfully');
  }

  /**
   * Register a new background task
   * 
   * @param {string} taskId - Unique identifier for the task
   * @param {string} type - Task type (IMMEDIATE, SCHEDULED, PUSH_TRIGGERED, ONE_TIME)
   * @param {Function} taskFunction - The function to execute
   * @param {Object} options - Additional configuration options
   */
  registerTask(taskId, type, taskFunction, options = {}) {
    if (!Object.values(TASK_TYPES).includes(type)) {
      throw new Error(`Invalid task type: ${type}`);
    }

    const task = {
      id: taskId,
      type,
      function: taskFunction,
      options: {
        interval: options.interval || 60000, // Default 1 minute for scheduled tasks
        maxRetries: options.maxRetries || this.config.maxRetryAttempts,
        pushTriggers: options.pushTriggers || [], // Array of push notification types that trigger this task
        delay: options.delay || 0, // Delay for one-time tasks
        enabled: options.enabled !== false, // Default to enabled
        ...options
      },
      lastRun: 0,
      nextRun: 0,
      isRunning: false,
      createdAt: Date.now()
    };

    this.tasks.set(taskId, task);
    
    // Schedule the task if it's a scheduled or one-time task
    if (type === TASK_TYPES.SCHEDULED) {
      this.scheduleTask(task);
    } else if (type === TASK_TYPES.ONE_TIME) {
      this.scheduleOneTimeTask(task);
    }

    console.log(`Registered ${type} task: ${taskId}`);
    
    // Persist the updated task list
    this.persistTasks();
  }

  /**
   * Remove a registered task
   */
  unregisterTask(taskId) {
    const task = this.tasks.get(taskId);
    if (!task) {
      console.log(`Task ${taskId} not found`);
      return;
    }

    // Clear any active timers for this task
    this.clearTaskTimers(taskId);
    
    // Remove from task registry
    this.tasks.delete(taskId);
    
    console.log(`Unregistered task: ${taskId}`);
    this.persistTasks();
  }

  /**
   * Execute a task with comprehensive error handling and retry logic
   */
  async executeTask(task, context = EXECUTION_CONTEXTS.BACKGROUND) {
    if (task.isRunning) {
      console.log(`Task ${task.id} is already running, skipping`);
      return;
    }

    console.log(`Executing task ${task.id} in ${context} context`);
    task.isRunning = true;

    try {
      // Create a timeout promise to prevent tasks from running too long
      const timeoutPromise = new Promise((_, reject) => {
        setTimeout(() => reject(new Error('Task timeout')), 
          context === EXECUTION_CONTEXTS.BACKGROUND ? this.config.maxBackgroundTime : 60000);
      });

      // Race the task against the timeout
      await Promise.race([
        task.function(context),
        timeoutPromise
      ]);

      // Task completed successfully
      task.lastRun = Date.now();
      this.retryAttempts.delete(task.id);
      console.log(`Task ${task.id} completed successfully`);

    } catch (error) {
      console.log(`Task ${task.id} failed:`, error.message);
      
      // Handle retry logic
      await this.handleTaskFailure(task, error, context);
    } finally {
      task.isRunning = false;
      this.persistTasks();
    }
  }

  /**
   * Handle task failures with exponential backoff retry
   */
  async handleTaskFailure(task, error, context) {
    const currentAttempts = this.retryAttempts.get(task.id) || 0;
    
    if (currentAttempts < task.options.maxRetries) {
      const nextAttempts = currentAttempts + 1;
      this.retryAttempts.set(task.id, nextAttempts);
      
      // Calculate exponential backoff delay
      const delay = this.config.baseRetryDelay * Math.pow(2, currentAttempts);
      
      console.log(`Scheduling retry ${nextAttempts}/${task.options.maxRetries} for task ${task.id} in ${delay}ms`);
      
      // Schedule retry based on context
      if (context === EXECUTION_CONTEXTS.BACKGROUND) {
        // In background, retry immediately if we have time
        setTimeout(() => {
          this.executeTask(task, context);
        }, Math.min(delay, 5000)); // Don't wait too long in background
      } else {
        // In foreground, we can wait longer
        setTimeout(() => {
          this.executeTask(task, context);
        }, delay);
      }
    } else {
      console.log(`Task ${task.id} failed after ${task.options.maxRetries} attempts`);
      this.retryAttempts.delete(task.id);
      
      // Store failure information for debugging
      await AsyncStorage.setItem(`taskFailure_${task.id}`, JSON.stringify({
        error: error.message,
        timestamp: Date.now(),
        attempts: currentAttempts + 1
      }));
    }
  }

  /**
   * Set up monitoring of app state changes
   */
  setupAppStateMonitoring() {
    AppState.addEventListener('change', (nextAppState) => {
      console.log(`App state changed: ${this.currentAppState} -> ${nextAppState}`);
      
      if (this.currentAppState === 'active' && nextAppState.match(/inactive|background/)) {
        // App is going to background
        this.handleAppGoingToBackground();
      } else if (this.currentAppState.match(/inactive|background/) && nextAppState === 'active') {
        // App is coming to foreground
        this.handleAppComingToForeground();
      }
      
      this.currentAppState = nextAppState;
    });
  }

  /**
   * Handle app going to background - execute immediate tasks
   */
  handleAppGoingToBackground() {
    console.log('App going to background, executing immediate tasks...');
    
    // Get all immediate tasks
    const immediateTasks = Array.from(this.tasks.values())
      .filter(task => task.type === TASK_TYPES.IMMEDIATE && task.options.enabled);
    
    if (immediateTasks.length === 0) {
      console.log('No immediate tasks to execute');
      return;
    }

    // Request extended background execution time
    const backgroundTaskId = BackgroundTimer.start(async () => {
      try {
        // Execute immediate tasks concurrently but with limit
        await this.executeConcurrentTasks(immediateTasks, EXECUTION_CONTEXTS.BACKGROUND);
      } catch (error) {
        console.log('Error executing immediate tasks:', error);
      } finally {
        // Always clean up the background task
        BackgroundTimer.stop(backgroundTaskId);
      }
    });

    // Safety timeout to ensure we don't exceed allowed background time
    setTimeout(() => {
      BackgroundTimer.stop(backgroundTaskId);
      console.log('Background execution stopped due to safety timeout');
    }, this.config.maxBackgroundTime);
  }

  /**
   * Handle app coming to foreground - check for pending tasks
   */
  async handleAppComingToForeground() {
    console.log('App coming to foreground, checking for pending tasks...');
    
    // Check for failed tasks that might need user attention
    await this.checkForFailedTasks();
    
    // Update next run times for scheduled tasks
    this.updateScheduledTasks();
    
    // Execute any overdue scheduled tasks
    await this.executeOverdueTasks();
  }

  /**
   * Execute multiple tasks concurrently with concurrency limit
   */
  async executeConcurrentTasks(tasks, context) {
    const concurrentTasks = [];
    const maxConcurrent = this.config.maxConcurrentTasks;
    
    for (let i = 0; i < tasks.length; i += maxConcurrent) {
      const batch = tasks.slice(i, i + maxConcurrent);
      const batchPromises = batch.map(task => this.executeTask(task, context));
      
      try {
        await Promise.allSettled(batchPromises);
      } catch (error) {
        console.log('Error in task batch execution:', error);
      }
    }
  }

  /**
   * Set up push notification background handler
   */
  setupPushNotificationHandler() {
    messaging().setBackgroundMessageHandler(async (remoteMessage) => {
      console.log('Push notification received in background:', remoteMessage);
      
      // Find tasks that should be triggered by this push notification
      const triggeredTasks = Array.from(this.tasks.values())
        .filter(task => 
          task.type === TASK_TYPES.PUSH_TRIGGERED && 
          task.options.enabled &&
          this.shouldTriggerTask(task, remoteMessage)
        );
      
      if (triggeredTasks.length > 0) {
        console.log(`Executing ${triggeredTasks.length} push-triggered tasks`);
        await this.executeConcurrentTasks(triggeredTasks, EXECUTION_CONTEXTS.PUSH_HANDLER);
      }
    });
  }

  /**
   * Check if a push notification should trigger a specific task
   */
  shouldTriggerTask(task, remoteMessage) {
    const triggers = task.options.pushTriggers;
    if (!triggers || triggers.length === 0) return false;
    
    // Check if the push notification type matches any of the task's triggers
    return triggers.some(trigger => {
      if (typeof trigger === 'string') {
        return remoteMessage.data?.type === trigger;
      } else if (typeof trigger === 'function') {
        return trigger(remoteMessage);
      }
      return false;
    });
  }

  /**
   * Schedule a recurring task
   */
  scheduleTask(task) {
    if (task.type !== TASK_TYPES.SCHEDULED) return;
    
    const interval = task.options.interval;
    
    const timerId = BackgroundTimer.setInterval(async () => {
      if (task.options.enabled) {
        await this.executeTask(task, EXECUTION_CONTEXTS.FOREGROUND);
      }
    }, interval);
    
    this.activeTimers.add(timerId);
    task.timerId = timerId;
    
    console.log(`Scheduled task ${task.id} to run every ${interval}ms`);
  }

  /**
   * Schedule a one-time task
   */
  scheduleOneTimeTask(task) {
    if (task.type !== TASK_TYPES.ONE_TIME) return;
    
    const delay = task.options.delay;
    
    const timerId = BackgroundTimer.setTimeout(async () => {
      await this.executeTask(task, EXECUTION_CONTEXTS.FOREGROUND);
      // Remove the task after execution since it's one-time
      this.unregisterTask(task.id);
    }, delay);
    
    this.activeTimers.add(timerId);
    task.timerId = timerId;
    
    console.log(`Scheduled one-time task ${task.id} to run in ${delay}ms`);
  }

  /**
   * Clear all timers associated with a task
   */
  clearTaskTimers(taskId) {
    const task = this.tasks.get(taskId);
    if (task && task.timerId) {
      if (task.type === TASK_TYPES.SCHEDULED) {
        BackgroundTimer.clearInterval(task.timerId);
      } else if (task.type === TASK_TYPES.ONE_TIME) {
        BackgroundTimer.clearTimeout(task.timerId);
      }
      
      this.activeTimers.delete(task.timerId);
      delete task.timerId;
    }
  }

  /**
   * Check background execution permissions
   */
  async checkBackgroundPermissions() {
    if (Platform.OS === 'ios') {
      // Test if background execution actually works
      return new Promise((resolve) => {
        let testCompleted = false;
        
        const testTaskId = BackgroundTimer.start(() => {
          if (!testCompleted) {
            testCompleted = true;
            this.isBackgroundRefreshEnabled = true;
            BackgroundTimer.stop(testTaskId);
            resolve(true);
          }
        });
        
        setTimeout(() => {
          if (!testCompleted) {
            testCompleted = true;
            this.isBackgroundRefreshEnabled = false;
            BackgroundTimer.stop(testTaskId);
            console.log('Background app refresh appears to be disabled');
            resolve(false);
          }
        }, 2000);
      });
    }
    
    // Android permissions are handled differently
    this.isBackgroundRefreshEnabled = true;
    return true;
  }

  /**
   * Persist task configuration to storage
   */
  async persistTasks() {
    try {
      const taskData = Array.from(this.tasks.entries()).map(([id, task]) => ({
        id,
        type: task.type,
        options: task.options,
        lastRun: task.lastRun,
        nextRun: task.nextRun,
        createdAt: task.createdAt
      }));
      
      await AsyncStorage.setItem(this.config.persistenceKey, JSON.stringify(taskData));
    } catch (error) {
      console.log('Failed to persist tasks:', error);
    }
  }

  /**
   * Restore persisted tasks from storage
   */
  async restorePersistedTasks() {
    try {
      const taskDataString = await AsyncStorage.getItem(this.config.persistenceKey);
      if (!taskDataString) return;
      
      const taskData = JSON.parse(taskDataString);
      
      taskData.forEach(savedTask => {
        // Note: We can't restore the actual task functions, only metadata
        // The app must re-register task functions on startup
        console.log(`Found persisted task: ${savedTask.id} (${savedTask.type})`);
        
        // Store the metadata for when the task gets re-registered
        AsyncStorage.setItem(`taskMeta_${savedTask.id}`, JSON.stringify(savedTask));
      });
    } catch (error) {
      console.log('Failed to restore persisted tasks:', error);
    }
  }

  /**
   * Check for tasks that failed and might need user attention
   */
  async checkForFailedTasks() {
    const allKeys = await AsyncStorage.getAllKeys();
    const failureKeys = allKeys.filter(key => key.startsWith('taskFailure_'));
    
    if (failureKeys.length > 0) {
      console.log(`Found ${failureKeys.length} failed tasks`);
      // You might want to show a notification or log this information
      // For this example, we'll just log it
    }
  }

  /**
   * Execute any scheduled tasks that are overdue
   */
  async executeOverdueTasks() {
    const now = Date.now();
    const overdueTasks = Array.from(this.tasks.values())
      .filter(task => 
        task.type === TASK_TYPES.SCHEDULED && 
        task.options.enabled &&
        (now - task.lastRun) >= task.options.interval
      );
    
    if (overdueTasks.length > 0) {
      console.log(`Executing ${overdueTasks.length} overdue tasks`);
      await this.executeConcurrentTasks(overdueTasks, EXECUTION_CONTEXTS.FOREGROUND);
    }
  }

  /**
   * Update scheduled task timing
   */
  updateScheduledTasks() {
    const now = Date.now();
    
    this.tasks.forEach(task => {
      if (task.type === TASK_TYPES.SCHEDULED) {
        task.nextRun = task.lastRun + task.options.interval;
      }
    });
  }

  /**
   * Get status information about all tasks
   */
  getTaskStatus() {
    const status = {
      totalTasks: this.tasks.size,
      tasksByType: {},
      backgroundRefreshEnabled: this.isBackgroundRefreshEnabled,
      activeTimers: this.activeTimers.size
    };
    
    Object.values(TASK_TYPES).forEach(type => {
      status.tasksByType[type] = Array.from(this.tasks.values())
        .filter(task => task.type === type).length;
    });
    
    return status;
  }

  /**
   * Clean up resources when shutting down
   */
  cleanup() {
    // Clear all active timers
    this.activeTimers.forEach(timerId => {
      BackgroundTimer.clearInterval(timerId);
      BackgroundTimer.clearTimeout(timerId);
    });
    
    this.activeTimers.clear();
    console.log('BackgroundTaskManager cleaned up');
  }
}

// Export a singleton instance
const backgroundTaskManager = new BackgroundTaskManager();

// Export task types for convenience
export { TASK_TYPES, EXECUTION_CONTEXTS };
export default backgroundTaskManager;

/**
 * USAGE EXAMPLES:
 * 
 * // Initialize the system when your app starts
 * import backgroundTaskManager, { TASK_TYPES } from './BackgroundTaskManager';
 * 
 * async function initializeApp() {
 *   await backgroundTaskManager.initialize();
 * }
 * 
 * // Register an immediate task (runs when app goes to background)
 * backgroundTaskManager.registerTask(
 *   'syncUserData',
 *   TASK_TYPES.IMMEDIATE,
 *   async (context) => {
 *     console.log('Syncing user data in', context);
 *     await syncCriticalUserData();
 *   },
 *   {
 *     maxRetries: 2
 *   }
 * );
 * 
 * // Register a scheduled task (runs every 30 minutes)
 * backgroundTaskManager.registerTask(
 *   'dataCleanup',
 *   TASK_TYPES.SCHEDULED,
 *   async (context) => {
 *     console.log('Cleaning up data in', context);
 *     await cleanupOldData();
 *   },
 *   {
 *     interval: 30 * 60 * 1000, // 30 minutes
 *     enabled: true
 *   }
 * );
 * 
 * // Register a push-triggered task
 * backgroundTaskManager.registerTask(
 *   'handleNewMessage',
 *   TASK_TYPES.PUSH_TRIGGERED,
 *   async (context) => {
 *     console.log('Handling new message push in', context);
 *     await downloadLatestMessages();
 *   },
 *   {
 *     pushTriggers: ['new_message', 'urgent_notification']
 *   }
 * );
 * 
 * // Register a one-time delayed task
 * backgroundTaskManager.registerTask(
 *   'initialSetup',
 *   TASK_TYPES.ONE_TIME,
 *   async (context) => {
 *     console.log('Running initial setup in', context);
 *     await performInitialSetup();
 *   },
 *   {
 *     delay: 5000 // Run in 5 seconds
 *   }
 * );
 * 
 * // Check system status
 * console.log(backgroundTaskManager.getTaskStatus());
 * 
 * // Clean up when app shuts down
 * backgroundTaskManager.cleanup();
 */
```