Understanding navigation in React Native applications is like learning to design the flow of movement through a complex building. Just as an architect must consider how people will move between rooms, floors, and sections while ensuring they never get lost and can always find their way back, you need to carefully design how users move through your app's various screens and features.

Let me guide you through this journey by starting with the fundamental concepts and building up to sophisticated navigation patterns that can handle the most complex mobile application requirements.

## The Foundation: Understanding Navigation in Mobile Context

Before diving into specific navigation libraries, it's essential to understand what makes mobile navigation unique and challenging. Unlike web applications where users can type URLs, use browser back buttons, or open multiple tabs, mobile apps exist in a more constrained environment where navigation must be entirely designed and controlled by the developer.

Think of mobile navigation like designing a museum experience. Visitors enter through a lobby, move through various exhibits in a logical sequence, can step into side galleries when interested, and always need a clear path back to where they came from or to the main areas. The navigation system must be intuitive enough that visitors never feel lost, yet flexible enough to accommodate different interests and browsing patterns.

Mobile apps face additional complexity because users interact through touch gestures, have limited screen space, and expect certain platform-specific behaviors. iOS users expect to swipe from the left edge to go back, while Android users expect a dedicated back button to work predictably. These platform conventions must be respected while maintaining a consistent experience across your app.

React Native applications inherit these challenges and add the complexity of managing navigation state in a JavaScript environment while providing native-feeling transitions and gestures. This is where navigation libraries become essential, providing tested solutions to these complex requirements while allowing you to focus on your app's unique features.

## React Navigation: The Comprehensive Navigation Solution

React Navigation has emerged as the most widely adopted and sophisticated navigation library for React Native applications. Think of React Navigation like having a master architect who not only designs the basic structure of your building but also provides specialized blueprints for elevators, escalators, emergency exits, and accessibility features.

The library's philosophy centers around component-based navigation where each navigation pattern is implemented as a React component that manages its own state and behavior. This approach aligns naturally with React's component model and makes navigation systems composable and predictable.

React Navigation handles the complex aspects of mobile navigation automatically. It manages the navigation state, provides smooth transitions between screens, handles hardware back button behavior on Android, implements platform-specific gestures, and provides hooks and utilities for navigation-aware components. This comprehensive approach means you can implement sophisticated navigation patterns without deep knowledge of the underlying platform complexities.

Let's begin with the most fundamental navigation pattern and build our understanding progressively.

## Stack Navigation: The Foundation of Mobile Navigation

Stack navigation represents the most basic and essential navigation pattern in mobile applications. Imagine a stack of cards where you can place new cards on top and remove cards from the top, but you always see only the topmost card. This metaphor perfectly captures how stack navigation works in mobile apps.

When a user opens your app, they see the initial screen. When they tap a button to view details about something, a new screen slides in from the right and covers the previous screen. The previous screen remains in memory beneath the new one, ready to be revealed when the user navigates back. This creates a natural hierarchy where users can drill down into details and easily return to where they came from.

Here's how you implement a basic stack navigation system:

```javascript
import React from 'react';
import { View, Text, TouchableOpacity, StyleSheet } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

// Create the stack navigator instance
// Think of this as creating the framework that will manage your screen stack
const Stack = createNativeStackNavigator();

// Home screen component - this will be our starting point
function HomeScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Welcome to Our App</Text>
      <Text style={styles.description}>
        This is the home screen where users start their journey.
      </Text>
      
      {/* Navigation actions that push new screens onto the stack */}
      <TouchableOpacity 
        style={styles.button}
        onPress={() => {
          // Push a new screen onto the stack with parameters
          navigation.navigate('Details', {
            itemId: 1,
            itemTitle: 'First Item',
            itemDescription: 'This is a detailed view of the first item.'
          });
        }}
      >
        <Text style={styles.buttonText}>View Item Details</Text>
      </TouchableOpacity>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => {
          // Navigate to a different screen type
          navigation.navigate('Profile', {
            userId: 'current-user'
          });
        }}
      >
        <Text style={styles.buttonText}>View Profile</Text>
      </TouchableOpacity>
    </View>
  );
}

// Details screen - demonstrates receiving and using navigation parameters
function DetailsScreen({ route, navigation }) {
  // Extract parameters passed from the previous screen
  const { itemId, itemTitle, itemDescription } = route.params;
  
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>{itemTitle}</Text>
      <Text style={styles.description}>{itemDescription}</Text>
      <Text style={styles.metadata}>Item ID: {itemId}</Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => {
          // Navigate deeper into the stack - this creates a three-level hierarchy
          navigation.navigate('Comments', {
            itemId: itemId,
            commentCount: 23
          });
        }}
      >
        <Text style={styles.buttonText}>View Comments (23)</Text>
      </TouchableOpacity>
      
      <TouchableOpacity 
        style={[styles.button, styles.secondaryButton]}
        onPress={() => {
          // Go back to the previous screen
          // This pops the current screen off the stack
          navigation.goBack();
        }}
      >
        <Text style={styles.buttonText}>Go Back</Text>
      </TouchableOpacity>
    </View>
  );
}

// Profile screen - shows how different screen types can coexist in the same stack
function ProfileScreen({ route, navigation }) {
  const { userId } = route.params;
  
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>User Profile</Text>
      <Text style={styles.description}>Profile for user: {userId}</Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => {
          // Navigate to settings, which could lead to a different navigation pattern
          navigation.navigate('Settings');
        }}
      >
        <Text style={styles.buttonText}>Edit Settings</Text>
      </TouchableOpacity>
    </View>
  );
}

// Comments screen - demonstrates deep navigation hierarchies
function CommentsScreen({ route, navigation }) {
  const { itemId, commentCount } = route.params;
  
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Comments</Text>
      <Text style={styles.description}>
        Showing {commentCount} comments for item {itemId}
      </Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => {
          // Navigate back to the home screen, clearing the entire stack above it
          // This is useful for "start over" or "go to main menu" actions
          navigation.popToTop();
        }}
      >
        <Text style={styles.buttonText}>Back to Home</Text>
      </TouchableOpacity>
    </View>
  );
}

// Settings screen with customized header
function SettingsScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Settings</Text>
      <Text style={styles.description}>Configure your app preferences here.</Text>
    </View>
  );
}

// Main app component that sets up the navigation structure
export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator
        // Configure default options that apply to all screens in this stack
        screenOptions={{
          headerStyle: {
            backgroundColor: '#6200EE', // Material Design primary color
          },
          headerTintColor: '#FFFFFF', // White text and icons
          headerTitleStyle: {
            fontWeight: 'bold',
            fontSize: 18,
          },
          // Enable the gesture to swipe back on iOS
          gestureEnabled: true,
          // Customize the transition animation
          animation: 'slide_from_right',
        }}
      >
        {/* Define each screen in the stack */}
        {/* The first screen becomes the initial screen */}
        <Stack.Screen 
          name="Home" 
          component={HomeScreen}
          options={{
            title: 'Welcome', // Custom title for the header
            headerStyle: {
              backgroundColor: '#3700B3', // Darker shade for home screen
            },
          }}
        />
        
        <Stack.Screen 
          name="Details" 
          component={DetailsScreen}
          options={({ route }) => ({
            // Dynamic title based on the data passed to this screen
            title: route.params.itemTitle,
            // Add a custom right button to the header
            headerRight: () => (
              <TouchableOpacity onPress={() => console.log('Share pressed')}>
                <Text style={{ color: '#FFFFFF', marginRight: 15 }}>Share</Text>
              </TouchableOpacity>
            ),
          })}
        />
        
        <Stack.Screen 
          name="Profile" 
          component={ProfileScreen}
          options={{
            title: 'My Profile',
          }}
        />
        
        <Stack.Screen 
          name="Comments" 
          component={CommentsScreen}
          options={{
            title: 'Comments',
          }}
        />
        
        <Stack.Screen 
          name="Settings" 
          component={SettingsScreen}
          options={{
            title: 'Settings',
            // Present this screen as a modal on iOS
            presentation: 'modal',
          }}
        />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

// Styles for consistent visual design
const styles = StyleSheet.create({
  screenContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
    backgroundColor: '#FFFFFF',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#1C1C1E',
    marginBottom: 12,
    textAlign: 'center',
  },
  description: {
    fontSize: 16,
    color: '#3C3C43',
    textAlign: 'center',
    marginBottom: 24,
    lineHeight: 22,
  },
  metadata: {
    fontSize: 14,
    color: '#8E8E93',
    marginBottom: 24,
  },
  button: {
    backgroundColor: '#007AFF',
    paddingHorizontal: 24,
    paddingVertical: 12,
    borderRadius: 8,
    marginVertical: 8,
    minWidth: 200,
  },
  secondaryButton: {
    backgroundColor: '#8E8E93',
  },
  buttonText: {
    color: '#FFFFFF',
    fontSize: 16,
    fontWeight: '600',
    textAlign: 'center',
  },
});
```

This stack navigation example demonstrates several important concepts. The navigation prop provides methods to move between screens, while the route prop contains information about the current screen including any parameters passed to it. The NavigationContainer wraps the entire navigation structure and manages the navigation state, and each screen can customize its appearance through the options prop.

Notice how the navigation system maintains a clear hierarchy. Users can navigate from Home to Details to Comments, and at any point they can go back to previous screens or jump all the way back to the home screen. This creates a predictable and intuitive navigation experience that users can understand immediately.

## Tab Navigation: Organizing Primary Features

While stack navigation handles hierarchical movement through your app, tab navigation addresses the need to organize primary features that users might want to access frequently and in any order. Think of tab navigation like the main sections of a newspaper, where readers might want to jump directly to Sports, Business, or Entertainment without having to navigate through a hierarchical structure.

Tab navigation typically appears at the bottom of the screen on iOS and can appear at the top or bottom on Android. Each tab represents a primary area of your app's functionality, and tapping a tab instantly switches to that section. Users expect tab navigation to remember where they were in each section, so switching between tabs should preserve the navigation state within each tab's stack.

Here's how you implement a comprehensive tab navigation system:

```javascript
import React from 'react';
import { View, Text, TouchableOpacity, StyleSheet } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
// Icon library for tab icons - you would install this separately
import Ionicons from 'react-native-vector-icons/Ionicons';

// Create navigator instances
const Tab = createBottomTabNavigator();
const Stack = createNativeStackNavigator();

// Home tab content - this is actually a stack navigator within a tab
function HomeStackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen 
        name="HomeMain" 
        component={HomeScreen}
        options={{ title: 'Home' }}
      />
      <Stack.Screen 
        name="Article" 
        component={ArticleScreen}
        options={{ title: 'Article' }}
      />
    </Stack.Navigator>
  );
}

// Search tab content - another stack within a tab
function SearchStackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen 
        name="SearchMain" 
        component={SearchScreen}
        options={{ title: 'Search' }}
      />
      <Stack.Screen 
        name="SearchResults" 
        component={SearchResultsScreen}
        options={{ title: 'Results' }}
      />
    </Stack.Navigator>
  );
}

// Favorites tab - simpler single-screen tab
function FavoritesStackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen 
        name="FavoritesMain" 
        component={FavoritesScreen}
        options={{ title: 'Favorites' }}
      />
    </Stack.Navigator>
  );
}

// Profile tab with multiple possible screens
function ProfileStackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen 
        name="ProfileMain" 
        component={ProfileScreen}
        options={{ title: 'Profile' }}
      />
      <Stack.Screen 
        name="EditProfile" 
        component={EditProfileScreen}
        options={{ title: 'Edit Profile' }}
      />
      <Stack.Screen 
        name="Settings" 
        component={SettingsScreen}
        options={{ title: 'Settings' }}
      />
    </Stack.Navigator>
  );
}

// Individual screen components
function HomeScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Home Feed</Text>
      <Text style={styles.description}>
        Latest articles and updates from your followed topics.
      </Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => {
          // Navigate within this tab's stack
          navigation.navigate('Article', {
            articleId: 1,
            title: 'Understanding React Navigation'
          });
        }}
      >
        <Text style={styles.buttonText}>Read Featured Article</Text>
      </TouchableOpacity>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => {
          // Navigate to a different tab
          // The Search tab will show its default screen
          navigation.navigate('Search');
        }}
      >
        <Text style={styles.buttonText}>Search Articles</Text>
      </TouchableOpacity>
    </View>
  );
}

function ArticleScreen({ route, navigation }) {
  const { articleId, title } = route.params;
  
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>{title}</Text>
      <Text style={styles.description}>
        Article content would appear here. This demonstrates how each tab
        can have its own navigation stack with multiple screens.
      </Text>
      <Text style={styles.metadata}>Article ID: {articleId}</Text>
    </View>
  );
}

function SearchScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Search</Text>
      <Text style={styles.description}>
        Enter search terms to find articles that interest you.
      </Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => {
          // Navigate to search results within this tab
          navigation.navigate('SearchResults', {
            query: 'React Navigation',
            resultCount: 42
          });
        }}
      >
        <Text style={styles.buttonText}>Search for "React Navigation"</Text>
      </TouchableOpacity>
    </View>
  );
}

function SearchResultsScreen({ route, navigation }) {
  const { query, resultCount } = route.params;
  
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Search Results</Text>
      <Text style={styles.description}>
        Found {resultCount} results for "{query}"
      </Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => {
          // Navigate to a specific article from search results
          navigation.navigate('Article', {
            articleId: 123,
            title: 'Advanced React Navigation Patterns'
          });
        }}
      >
        <Text style={styles.buttonText}>View Top Result</Text>
      </TouchableOpacity>
    </View>
  );
}

function FavoritesScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Your Favorites</Text>
      <Text style={styles.description}>
        Articles you've saved for later reading.
      </Text>
    </View>
  );
}

function ProfileScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Your Profile</Text>
      <Text style={styles.description}>
        Manage your account and preferences.
      </Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => {
          navigation.navigate('EditProfile');
        }}
      >
        <Text style={styles.buttonText}>Edit Profile</Text>
      </TouchableOpacity>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => {
          navigation.navigate('Settings');
        }}
      >
        <Text style={styles.buttonText}>Settings</Text>
      </TouchableOpacity>
    </View>
  );
}

function EditProfileScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Edit Profile</Text>
      <Text style={styles.description}>
        Update your profile information here.
      </Text>
    </View>
  );
}

function SettingsScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Settings</Text>
      <Text style={styles.description}>
        Configure app preferences and account settings.
      </Text>
    </View>
  );
}

// Main app component with tab navigation
export default function App() {
  return (
    <NavigationContainer>
      <Tab.Navigator
        screenOptions={({ route }) => ({
          // Configure tab bar icons dynamically based on the route name
          tabBarIcon: ({ focused, color, size }) => {
            let iconName;
            
            // Choose appropriate icons for each tab
            if (route.name === 'Home') {
              iconName = focused ? 'home' : 'home-outline';
            } else if (route.name === 'Search') {
              iconName = focused ? 'search' : 'search-outline';
            } else if (route.name === 'Favorites') {
              iconName = focused ? 'heart' : 'heart-outline';
            } else if (route.name === 'Profile') {
              iconName = focused ? 'person' : 'person-outline';
            }
            
            return <Ionicons name={iconName} size={size} color={color} />;
          },
          // Customize tab bar appearance
          tabBarActiveTintColor: '#007AFF', // Active tab color
          tabBarInactiveTintColor: '#8E8E93', // Inactive tab color
          tabBarStyle: {
            backgroundColor: '#FFFFFF',
            borderTopWidth: 1,
            borderTopColor: '#E5E5EA',
            height: 88, // Standard iOS tab bar height
            paddingBottom: 34, // Account for iPhone safe area
          },
          tabBarLabelStyle: {
            fontSize: 10,
            fontWeight: '600',
          },
          // Hide the header for the tab navigator since each stack has its own headers
          headerShown: false,
        })}
      >
        {/* Define each tab */}
        <Tab.Screen 
          name="Home" 
          component={HomeStackScreen}
          options={{
            tabBarLabel: 'Home',
            // Badge to show unread count
            tabBarBadge: 3,
          }}
        />
        
        <Tab.Screen 
          name="Search" 
          component={SearchStackScreen}
          options={{
            tabBarLabel: 'Search',
          }}
        />
        
        <Tab.Screen 
          name="Favorites" 
          component={FavoritesStackScreen}
          options={{
            tabBarLabel: 'Favorites',
          }}
        />
        
        <Tab.Screen 
          name="Profile" 
          component={ProfileStackScreen}
          options={{
            tabBarLabel: 'Profile',
          }}
        />
      </Tab.Navigator>
    </NavigationContainer>
  );
}

// Reuse the same styles from the previous example
const styles = StyleSheet.create({
  screenContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
    backgroundColor: '#FFFFFF',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#1C1C1E',
    marginBottom: 12,
    textAlign: 'center',
  },
  description: {
    fontSize: 16,
    color: '#3C3C43',
    textAlign: 'center',
    marginBottom: 24,
    lineHeight: 22,
  },
  metadata: {
    fontSize: 14,
    color: '#8E8E93',
    marginBottom: 24,
  },
  button: {
    backgroundColor: '#007AFF',
    paddingHorizontal: 24,
    paddingVertical: 12,
    borderRadius: 8,
    marginVertical: 8,
    minWidth: 200,
  },
  buttonText: {
    color: '#FFFFFF',
    fontSize: 16,
    fontWeight: '600',
    textAlign: 'center',
  },
});
```

This tab navigation example demonstrates a sophisticated pattern where each tab contains its own stack navigator. This allows users to navigate deeply within one tab's content, switch to another tab, and then return to find their place preserved in the original tab. Notice how you can navigate between tabs programmatically and how each tab maintains its own navigation state independently.

The tab bar configuration shows how to customize appearance, add icons, include badges for notifications, and adapt to platform conventions. The dynamic icon selection based on focus state provides visual feedback that helps users understand which section they're currently viewing.

## Drawer Navigation: Organizing Secondary Features

Drawer navigation provides a space-efficient way to organize secondary features and navigation options that don't warrant permanent tab bar real estate. Think of the drawer like a filing cabinet that slides out from the side of your desk, providing access to important documents and tools that you need occasionally but don't want cluttering your main workspace.

The drawer pattern is particularly useful for features like settings, help documentation, user account management, and navigation shortcuts that experienced users appreciate but new users don't need immediately visible. When implemented well, drawer navigation creates a clean main interface while keeping advanced functionality easily accessible.

```javascript
import React from 'react';
import { View, Text, TouchableOpacity, StyleSheet, ScrollView } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createDrawerNavigator, DrawerContentScrollView, DrawerItemList, DrawerItem } from '@react-navigation/drawer';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import Ionicons from 'react-native-vector-icons/Ionicons';

const Drawer = createDrawerNavigator();
const Stack = createNativeStackNavigator();

// Custom drawer content component that provides more control over the drawer's appearance
function CustomDrawerContent(props) {
  return (
    <DrawerContentScrollView {...props} contentContainerStyle={styles.drawerContainer}>
      {/* Header section with user information */}
      <View style={styles.drawerHeader}>
        <View style={styles.avatarContainer}>
          <Text style={styles.avatarText}>JD</Text>
        </View>
        <Text style={styles.userName}>John Doe</Text>
        <Text style={styles.userEmail}>john.doe@example.com</Text>
      </View>
      
      {/* Standard drawer items */}
      <View style={styles.drawerItemsContainer}>
        <DrawerItemList {...props} />
        
        {/* Custom drawer items for special actions */}
        <DrawerItem
          label="Help & Support"
          onPress={() => props.navigation.navigate('Help')}
          icon={({ color, size }) => (
            <Ionicons name="help-circle-outline" color={color} size={size} />
          )}
          labelStyle={styles.drawerItemLabel}
        />
        
        <DrawerItem
          label="Send Feedback"
          onPress={() => {
            // Handle feedback action
            console.log('Send feedback pressed');
            props.navigation.closeDrawer();
          }}
          icon={({ color, size }) => (
            <Ionicons name="chatbubble-outline" color={color} size={size} />
          )}
          labelStyle={styles.drawerItemLabel}
        />
      </View>
      
      {/* Footer section with app version and logout */}
      <View style={styles.drawerFooter}>
        <DrawerItem
          label="Sign Out"
          onPress={() => {
            // Handle sign out
            console.log('Sign out pressed');
          }}
          icon={({ color, size }) => (
            <Ionicons name="log-out-outline" color={color} size={size} />
          )}
          labelStyle={[styles.drawerItemLabel, styles.signOutLabel]}
        />
        <Text style={styles.versionText}>Version 1.0.0</Text>
      </View>
    </DrawerContentScrollView>
  );
}

// Stack navigators for each main section
function HomeStackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen 
        name="HomeMain" 
        component={HomeScreen}
        options={({ navigation }) => ({
          title: 'Dashboard',
          headerLeft: () => (
            <TouchableOpacity
              onPress={() => navigation.openDrawer()}
              style={styles.menuButton}
            >
              <Ionicons name="menu" size={24} color="#007AFF" />
            </TouchableOpacity>
          ),
        })}
      />
      <Stack.Screen 
        name="Details" 
        component={DetailsScreen}
        options={{ title: 'Details' }}
      />
    </Stack.Navigator>
  );
}

function ProjectsStackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen 
        name="ProjectsMain" 
        component={ProjectsScreen}
        options={({ navigation }) => ({
          title: 'Projects',
          headerLeft: () => (
            <TouchableOpacity
              onPress={() => navigation.openDrawer()}
              style={styles.menuButton}
            >
              <Ionicons name="menu" size={24} color="#007AFF" />
            </TouchableOpacity>
          ),
        })}
      />
      <Stack.Screen 
        name="ProjectDetails" 
        component={ProjectDetailsScreen}
        options={{ title: 'Project Details' }}
      />
    </Stack.Navigator>
  );
}

function TasksStackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen 
        name="TasksMain" 
        component={TasksScreen}
        options={({ navigation }) => ({
          title: 'Tasks',
          headerLeft: () => (
            <TouchableOpacity
              onPress={() => navigation.openDrawer()}
              style={styles.menuButton}
            >
              <Ionicons name="menu" size={24} color="#007AFF" />
            </TouchableOpacity>
          ),
        })}
      />
    </Stack.Navigator>
  );
}

function CalendarStackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen 
        name="CalendarMain" 
        component={CalendarScreen}
        options={({ navigation }) => ({
          title: 'Calendar',
          headerLeft: () => (
            <TouchableOpacity
              onPress={() => navigation.openDrawer()}
              style={styles.menuButton}
            >
              <Ionicons name="menu" size={24} color="#007AFF" />
            </TouchableOpacity>
          ),
        })}
      />
    </Stack.Navigator>
  );
}

function SettingsStackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen 
        name="SettingsMain" 
        component={SettingsScreen}
        options={({ navigation }) => ({
          title: 'Settings',
          headerLeft: () => (
            <TouchableOpacity
              onPress={() => navigation.openDrawer()}
              style={styles.menuButton}
            >
              <Ionicons name="menu" size={24} color="#007AFF" />
            </TouchableOpacity>
          ),
        })}
      />
    </Stack.Navigator>
  );
}

// Screen components
function HomeScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Dashboard</Text>
      <Text style={styles.description}>
        Welcome to your project management dashboard. Access all your work
        through the navigation drawer or the quick actions below.
      </Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => navigation.navigate('Details', { itemId: 1 })}
      >
        <Text style={styles.buttonText}>View Recent Activity</Text>
      </TouchableOpacity>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => navigation.navigate('Projects')}
      >
        <Text style={styles.buttonText}>Go to Projects</Text>
      </TouchableOpacity>
      
      <TouchableOpacity 
        style={[styles.button, styles.secondaryButton]}
        onPress={() => navigation.openDrawer()}
      >
        <Text style={styles.buttonText}>Open Menu</Text>
      </TouchableOpacity>
    </View>
  );
}

function DetailsScreen({ route, navigation }) {
  const { itemId } = route.params;
  
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Activity Details</Text>
      <Text style={styles.description}>
        Showing details for item {itemId}. This demonstrates how drawer navigation
        works with nested stack navigation.
      </Text>
    </View>
  );
}

function ProjectsScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Your Projects</Text>
      <Text style={styles.description}>
        Manage and track all your active projects from this central location.
      </Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => navigation.navigate('ProjectDetails', { 
          projectId: 'proj-001',
          projectName: 'Mobile App Development'
        })}
      >
        <Text style={styles.buttonText}>View Mobile App Project</Text>
      </TouchableOpacity>
    </View>
  );
}

function ProjectDetailsScreen({ route, navigation }) {
  const { projectId, projectName } = route.params;
  
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>{projectName}</Text>
      <Text style={styles.description}>
        Project ID: {projectId}
      </Text>
      <Text style={styles.description}>
        Detailed project information and tasks would be displayed here.
      </Text>
    </View>
  );
}

function TasksScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Task Management</Text>
      <Text style={styles.description}>
        View and manage all your assigned tasks across different projects.
      </Text>
    </View>
  );
}

function CalendarScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Calendar View</Text>
      <Text style={styles.description}>
        Schedule and track your meetings, deadlines, and important dates.
      </Text>
    </View>
  );
}

function SettingsScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>App Settings</Text>
      <Text style={styles.description}>
        Configure your preferences and account settings.
      </Text>
    </View>
  );
}

function HelpScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Help & Support</Text>
      <Text style={styles.description}>
        Find answers to common questions and get support for the app.
      </Text>
    </View>
  );
}

// Main app component with drawer navigation
export default function App() {
  return (
    <NavigationContainer>
      <Drawer.Navigator
        drawerContent={(props) => <CustomDrawerContent {...props} />}
        screenOptions={{
          // Configure drawer appearance and behavior
          drawerStyle: {
            backgroundColor: '#FFFFFF',
            width: 280, // Standard drawer width
          },
          drawerActiveTintColor: '#007AFF',
          drawerInactiveTintColor: '#8E8E93',
          drawerLabelStyle: {
            fontSize: 16,
            fontWeight: '500',
          },
          // Hide header since each stack provides its own
          headerShown: false,
          // Configure drawer icon appearance
          drawerItemStyle: {
            marginVertical: 2,
          },
        }}
      >
        {/* Primary navigation items - these appear as main sections */}
        <Drawer.Screen 
          name="Home" 
          component={HomeStackScreen}
          options={{
            drawerLabel: 'Dashboard',
            drawerIcon: ({ color, size }) => (
              <Ionicons name="home-outline" color={color} size={size} />
            ),
          }}
        />
        
        <Drawer.Screen 
          name="Projects" 
          component={ProjectsStackScreen}
          options={{
            drawerLabel: 'Projects',
            drawerIcon: ({ color, size }) => (
              <Ionicons name="folder-outline" color={color} size={size} />
            ),
          }}
        />
        
        <Drawer.Screen 
          name="Tasks" 
          component={TasksStackScreen}
          options={{
            drawerLabel: 'Tasks',
            drawerIcon: ({ color, size }) => (
              <Ionicons name="checkbox-outline" color={color} size={size} />
            ),
          }}
        />
        
        <Drawer.Screen 
          name="Calendar" 
          component={CalendarStackScreen}
          options={{
            drawerLabel: 'Calendar',
            drawerIcon: ({ color, size }) => (
              <Ionicons name="calendar-outline" color={color} size={size} />
            ),
          }}
        />
        
        {/* Secondary navigation items */}
        <Drawer.Screen 
          name="Settings" 
          component={SettingsStackScreen}
          options={{
            drawerLabel: 'Settings',
            drawerIcon: ({ color, size }) => (
              <Ionicons name="settings-outline" color={color} size={size} />
            ),
          }}
        />
        
        <Drawer.Screen 
          name="Help" 
          component={HelpScreen}
          options={{
            drawerLabel: 'Help',
            drawerIcon: ({ color, size }) => (
              <Ionicons name="help-circle-outline" color={color} size={size} />
            ),
          }}
        />
      </Drawer.Navigator>
    </NavigationContainer>
  );
}

// Comprehensive styles for the drawer navigation example
const styles = StyleSheet.create({
  screenContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
    backgroundColor: '#FFFFFF',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#1C1C1E',
    marginBottom: 12,
    textAlign: 'center',
  },
  description: {
    fontSize: 16,
    color: '#3C3C43',
    textAlign: 'center',
    marginBottom: 24,
    lineHeight: 22,
  },
  button: {
    backgroundColor: '#007AFF',
    paddingHorizontal: 24,
    paddingVertical: 12,
    borderRadius: 8,
    marginVertical: 8,
    minWidth: 200,
  },
  secondaryButton: {
    backgroundColor: '#8E8E93',
  },
  buttonText: {
    color: '#FFFFFF',
    fontSize: 16,
    fontWeight: '600',
    textAlign: 'center',
  },
  menuButton: {
    marginLeft: 15,
    padding: 5,
  },
  // Drawer-specific styles
  drawerContainer: {
    flex: 1,
  },
  drawerHeader: {
    backgroundColor: '#F2F2F7',
    padding: 20,
    borderBottomWidth: 1,
    borderBottomColor: '#E5E5EA',
  },
  avatarContainer: {
    width: 60,
    height: 60,
    borderRadius: 30,
    backgroundColor: '#007AFF',
    justifyContent: 'center',
    alignItems: 'center',
    marginBottom: 12,
  },
  avatarText: {
    color: '#FFFFFF',
    fontSize: 24,
    fontWeight: 'bold',
  },
  userName: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#1C1C1E',
    marginBottom: 4,
  },
  userEmail: {
    fontSize: 14,
    color: '#8E8E93',
  },
  drawerItemsContainer: {
    flex: 1,
    paddingTop: 10,
  },
  drawerItemLabel: {
    fontSize: 16,
    fontWeight: '500',
  },
  drawerFooter: {
    borderTopWidth: 1,
    borderTopColor: '#E5E5EA',
    paddingTop: 10,
  },
  signOutLabel: {
    color: '#FF3B30',
  },
  versionText: {
    fontSize: 12,
    color: '#8E8E93',
    textAlign: 'center',
    marginTop: 10,
    marginBottom: 10,
  },
});
```

This drawer navigation example showcases several advanced concepts. The custom drawer content component provides complete control over the drawer's appearance, allowing you to add user information, custom actions, and branded styling. Each main section uses its own stack navigator, creating a hierarchy where users can navigate deeply within sections while maintaining access to the global navigation through the drawer.

Notice how the drawer integrates with the header through custom menu buttons, providing multiple ways for users to access the navigation. The drawer also demonstrates how to organize navigation items into logical groups and include both standard navigation and special actions like sign out or feedback.

## Combining Navigation Patterns: Real-World Architecture

Most sophisticated React Native applications don't use just one navigation pattern but instead combine multiple patterns to create intuitive and powerful navigation systems. Think of this like designing a large office building where you might have elevators for moving between floors, hallways for moving between departments on the same floor, and emergency exits for special situations.

A common pattern for complex apps involves tabs for primary features, stacks within each tab for hierarchical content, and a drawer for secondary features and settings. This creates a navigation architecture that accommodates both casual browsing and power user workflows.

```javascript
import React from 'react';
import { View, Text, TouchableOpacity, StyleSheet } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { createDrawerNavigator } from '@react-navigation/drawer';
import Ionicons from 'react-native-vector-icons/Ionicons';

const Tab = createBottomTabNavigator();
const Stack = createNativeStackNavigator();
const Drawer = createDrawerNavigator();

// The main tab navigator that contains the primary app features
function MainTabNavigator() {
  return (
    <Tab.Navigator
      screenOptions={({ route }) => ({
        tabBarIcon: ({ focused, color, size }) => {
          let iconName;
          
          if (route.name === 'Feed') {
            iconName = focused ? 'home' : 'home-outline';
          } else if (route.name === 'Discover') {
            iconName = focused ? 'compass' : 'compass-outline';
          } else if (route.name === 'Create') {
            iconName = focused ? 'add-circle' : 'add-circle-outline';
          } else if (route.name === 'Activity') {
            iconName = focused ? 'heart' : 'heart-outline';
          } else if (route.name === 'Profile') {
            iconName = focused ? 'person' : 'person-outline';
          }
          
          return <Ionicons name={iconName} size={size} color={color} />;
        },
        tabBarActiveTintColor: '#007AFF',
        tabBarInactiveTintColor: '#8E8E93',
        headerShown: false,
        tabBarStyle: {
          backgroundColor: '#FFFFFF',
          borderTopWidth: 1,
          borderTopColor: '#E5E5EA',
        },
      })}
    >
      <Tab.Screen name="Feed" component={FeedStackNavigator} />
      <Tab.Screen name="Discover" component={DiscoverStackNavigator} />
      <Tab.Screen 
        name="Create" 
        component={CreateStackNavigator}
        options={{
          // Special styling for the create button
          tabBarButton: (props) => (
            <TouchableOpacity
              {...props}
              style={[props.style, styles.createTabButton]}
            >
              <Ionicons name="add" size={24} color="#FFFFFF" />
            </TouchableOpacity>
          ),
        }}
      />
      <Tab.Screen name="Activity" component={ActivityStackNavigator} />
      <Tab.Screen name="Profile" component={ProfileStackNavigator} />
    </Tab.Navigator>
  );
}

// Stack navigator for the Feed tab
function FeedStackNavigator() {
  return (
    <Stack.Navigator>
      <Stack.Screen 
        name="FeedMain" 
        component={FeedScreen}
        options={({ navigation }) => ({
          title: 'Feed',
          headerLeft: () => (
            <TouchableOpacity
              onPress={() => navigation.getParent('drawer').openDrawer()}
              style={styles.headerButton}
            >
              <Ionicons name="menu" size={24} color="#007AFF" />
            </TouchableOpacity>
          ),
          headerRight: () => (
            <TouchableOpacity
              onPress={() => navigation.navigate('Messages')}
              style={styles.headerButton}
            >
              <Ionicons name="chatbubble-outline" size={24} color="#007AFF" />
            </TouchableOpacity>
          ),
        })}
      />
      <Stack.Screen 
        name="PostDetail" 
        component={PostDetailScreen}
        options={{ title: 'Post' }}
      />
      <Stack.Screen 
        name="UserProfile" 
        component={UserProfileScreen}
        options={{ title: 'Profile' }}
      />
      <Stack.Screen 
        name="Messages" 
        component={MessagesScreen}
        options={{ title: 'Messages' }}
      />
    </Stack.Navigator>
  );
}

// Stack navigator for the Discover tab
function DiscoverStackNavigator() {
  return (
    <Stack.Navigator>
      <Stack.Screen 
        name="DiscoverMain" 
        component={DiscoverScreen}
        options={({ navigation }) => ({
          title: 'Discover',
          headerLeft: () => (
            <TouchableOpacity
              onPress={() => navigation.getParent('drawer').openDrawer()}
              style={styles.headerButton}
            >
              <Ionicons name="menu" size={24} color="#007AFF" />
            </TouchableOpacity>
          ),
        })}
      />
      <Stack.Screen 
        name="SearchResults" 
        component={SearchResultsScreen}
        options={{ title: 'Search Results' }}
      />
      <Stack.Screen 
        name="TopicDetail" 
        component={TopicDetailScreen}
        options={{ title: 'Topic' }}
      />
    </Stack.Navigator>
  );
}

// Stack navigator for the Create tab
function CreateStackNavigator() {
  return (
    <Stack.Navigator>
      <Stack.Screen 
        name="CreateMain" 
        component={CreateScreen}
        options={{ 
          title: 'Create Post',
          presentation: 'modal', // Present as modal on iOS
        }}
      />
      <Stack.Screen 
        name="CreatePhoto" 
        component={CreatePhotoScreen}
        options={{ title: 'Add Photo' }}
      />
      <Stack.Screen 
        name="CreateVideo" 
        component={CreateVideoScreen}
        options={{ title: 'Create Video' }}
      />
    </Stack.Navigator>
  );
}

// Stack navigator for the Activity tab
function ActivityStackNavigator() {
  return (
    <Stack.Navigator>
      <Stack.Screen 
        name="ActivityMain" 
        component={ActivityScreen}
        options={{ title: 'Activity' }}
      />
      <Stack.Screen 
        name="NotificationDetail" 
        component={NotificationDetailScreen}
        options={{ title: 'Notification' }}
      />
    </Stack.Navigator>
  );
}

// Stack navigator for the Profile tab
function ProfileStackNavigator() {
  return (
    <Stack.Navigator>
      <Stack.Screen 
        name="ProfileMain" 
        component={ProfileScreen}
        options={({ navigation }) => ({
          title: 'Profile',
          headerRight: () => (
            <TouchableOpacity
              onPress={() => navigation.getParent('drawer').openDrawer()}
              style={styles.headerButton}
            >
              <Ionicons name="settings-outline" size={24} color="#007AFF" />
            </TouchableOpacity>
          ),
        })}
      />
      <Stack.Screen 
        name="EditProfile" 
        component={EditProfileScreen}
        options={{ title: 'Edit Profile' }}
      />
      <Stack.Screen 
        name="Followers" 
        component={FollowersScreen}
        options={{ title: 'Followers' }}
      />
      <Stack.Screen 
        name="Following" 
        component={FollowingScreen}
        options={{ title: 'Following' }}
      />
    </Stack.Navigator>
  );
}

// Individual screen components
function FeedScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Your Feed</Text>
      <Text style={styles.description}>
        Latest posts from people you follow.
      </Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => navigation.navigate('PostDetail', { 
          postId: 1, 
          postTitle: 'Amazing Sunset Photo' 
        })}
      >
        <Text style={styles.buttonText}>View Featured Post</Text>
      </TouchableOpacity>
    </View>
  );
}

function PostDetailScreen({ route, navigation }) {
  const { postId, postTitle } = route.params;
  
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>{postTitle}</Text>
      <Text style={styles.description}>Post ID: {postId}</Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => navigation.navigate('UserProfile', { 
          userId: 'photographer123',
          userName: 'Amazing Photographer'
        })}
      >
        <Text style={styles.buttonText}>View Photographer's Profile</Text>
      </TouchableOpacity>
    </View>
  );
}

function UserProfileScreen({ route, navigation }) {
  const { userId, userName } = route.params;
  
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>{userName}</Text>
      <Text style={styles.description}>User ID: {userId}</Text>
      <Text style={styles.description}>
        Professional photographer sharing amazing moments.
      </Text>
    </View>
  );
}

function MessagesScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Messages</Text>
      <Text style={styles.description}>
        Your private conversations with other users.
      </Text>
    </View>
  );
}

function DiscoverScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Discover</Text>
      <Text style={styles.description}>
        Explore trending topics and find new content.
      </Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => navigation.navigate('SearchResults', { 
          query: 'photography',
          resultCount: 156
        })}
      >
        <Text style={styles.buttonText}>Search Photography</Text>
      </TouchableOpacity>
    </View>
  );
}

function SearchResultsScreen({ route, navigation }) {
  const { query, resultCount } = route.params;
  
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Search Results</Text>
      <Text style={styles.description}>
        Found {resultCount} results for "{query}"
      </Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => navigation.navigate('TopicDetail', { 
          topicId: 'photography-tips',
          topicName: 'Photography Tips'
        })}
      >
        <Text style={styles.buttonText}>View Top Topic</Text>
      </TouchableOpacity>
    </View>
  );
}

function TopicDetailScreen({ route, navigation }) {
  const { topicId, topicName } = route.params;
  
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>{topicName}</Text>
      <Text style={styles.description}>Topic ID: {topicId}</Text>
      <Text style={styles.description}>
        Learn and share photography techniques with the community.
      </Text>
    </View>
  );
}

function CreateScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Create New Post</Text>
      <Text style={styles.description}>
        Share your amazing moments with the community.
      </Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => navigation.navigate('CreatePhoto')}
      >
        <Text style={styles.buttonText}>Add Photo</Text>
      </TouchableOpacity>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => navigation.navigate('CreateVideo')}
      >
        <Text style={styles.buttonText}>Create Video</Text>
      </TouchableOpacity>
    </View>
  );
}

function CreatePhotoScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Add Photo</Text>
      <Text style={styles.description}>
        Select and edit a photo to share.
      </Text>
    </View>
  );
}

function CreateVideoScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Create Video</Text>
      <Text style={styles.description}>
        Record or upload a video to share.
      </Text>
    </View>
  );
}

function ActivityScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Activity</Text>
      <Text style={styles.description}>
        See who liked, commented, and followed you.
      </Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => navigation.navigate('NotificationDetail', { 
          notificationId: 1,
          notificationType: 'like'
        })}
      >
        <Text style={styles.buttonText}>View Latest Activity</Text>
      </TouchableOpacity>
    </View>
  );
}

function NotificationDetailScreen({ route, navigation }) {
  const { notificationId, notificationType } = route.params;
  
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Activity Detail</Text>
      <Text style={styles.description}>
        Notification {notificationId} - Type: {notificationType}
      </Text>
    </View>
  );
}

function ProfileScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Your Profile</Text>
      <Text style={styles.description}>
        Manage your profile and view your posts.
      </Text>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => navigation.navigate('EditProfile')}
      >
        <Text style={styles.buttonText}>Edit Profile</Text>
      </TouchableOpacity>
      
      <TouchableOpacity 
        style={styles.button}
        onPress={() => navigation.navigate('Followers')}
      >
        <Text style={styles.buttonText}>View Followers</Text>
      </TouchableOpacity>
    </View>
  );
}

function EditProfileScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Edit Profile</Text>
      <Text style={styles.description}>
        Update your profile information and settings.
      </Text>
    </View>
  );
}

function FollowersScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Followers</Text>
      <Text style={styles.description}>
        People who follow your account.
      </Text>
    </View>
  );
}

function FollowingScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Following</Text>
      <Text style={styles.description}>
        People you follow.
      </Text>
    </View>
  );
}

// Drawer navigator screens
function SettingsScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Settings</Text>
      <Text style={styles.description}>
        Configure your app preferences and account settings.
      </Text>
    </View>
  );
}

function HelpScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>Help & Support</Text>
      <Text style={styles.description}>
        Get help with using the app and contact support.
      </Text>
    </View>
  );
}

function AboutScreen({ navigation }) {
  return (
    <View style={styles.screenContainer}>
      <Text style={styles.title}>About</Text>
      <Text style={styles.description}>
        Learn more about our app and company.
      </Text>
    </View>
  );
}

// Main app component that combines all navigation patterns
export default function App() {
  return (
    <NavigationContainer>
      <Drawer.Navigator
        id="drawer"
        screenOptions={{
          drawerPosition: 'left',
          drawerStyle: {
            backgroundColor: '#FFFFFF',
            width: 280,
          },
          drawerActiveTintColor: '#007AFF',
          drawerInactiveTintColor: '#8E8E93',
          headerShown: false,
        }}
      >
        {/* Main app content */}
        <Drawer.Screen 
          name="MainApp" 
          component={MainTabNavigator}
          options={{
            drawerLabel: 'Home',
            drawerIcon: ({ color, size }) => (
              <Ionicons name="home-outline" color={color} size={size} />
            ),
          }}
        />
        
        {/* Secondary screens accessible through drawer */}
        <Drawer.Screen 
          name="Settings" 
          component={SettingsScreen}
          options={{
            drawerIcon: ({ color, size }) => (
              <Ionicons name="settings-outline" color={color} size={size} />
            ),
          }}
        />
        
        <Drawer.Screen 
          name="Help" 
          component={HelpScreen}
          options={{
            drawerIcon: ({ color, size }) => (
              <Ionicons name="help-circle-outline" color={color} size={size} />
            ),
          }}
        />
        
        <Drawer.Screen 
          name="About" 
          component={AboutScreen}
          options={{
            drawerIcon: ({ color, size }) => (
              <Ionicons name="information-circle-outline" color={color} size={size} />
            ),
          }}
        />
      </Drawer.Navigator>
    </NavigationContainer>
  );
}

// Comprehensive styles
const styles = StyleSheet.create({
  screenContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
    backgroundColor: '#FFFFFF',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#1C1C1E',
    marginBottom: 12,
    textAlign: 'center',
  },
  description: {
    fontSize: 16,
    color: '#3C3C43',
    textAlign: 'center',
    marginBottom: 24,
    lineHeight: 22,
  },
  button: {
    backgroundColor: '#007AFF',
    paddingHorizontal: 24,
    paddingVertical: 12,
    borderRadius: 8,
    marginVertical: 8,
    minWidth: 200,
  },
  buttonText: {
    color: '#FFFFFF',
    fontSize: 16,
    fontWeight: '600',
    textAlign: 'center',
  },
  headerButton: {
    marginHorizontal: 15,
    padding: 5,
  },
  createTabButton: {
    top: -10,
    backgroundColor: '#007AFF',
    borderRadius: 25,
    width: 50,
    height: 50,
    justifyContent: 'center',
    alignItems: 'center',
    shadowColor: '#000000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.25,
    shadowRadius: 3.84,
    elevation: 5,
  },
});
```

This comprehensive example demonstrates how to combine multiple navigation patterns into a cohesive system. The drawer provides access to secondary features and settings, tabs organize the primary app functionality, and stacks within each tab handle hierarchical navigation. Notice how navigation between different parts of the system works seamlessly, and how you can access parent navigators using methods like `getParent('drawer').openDrawer()`.

## Alternative Navigation Libraries and Specialized Solutions

While React Navigation has become the de facto standard for React Native navigation, understanding alternative libraries helps you make informed decisions for specific use cases and appreciate the unique advantages that different approaches offer.

React Native Navigation by Wix represents the most significant alternative to React Navigation. Think of it like choosing between two different architectural approaches for building a bridge. React Navigation builds a bridge using standardized components and JavaScript coordination, while React Native Navigation builds a bridge using native materials and native construction techniques.

React Native Navigation provides navigation that is entirely powered by native navigation controllers on each platform. This means that every screen in your app is a native screen, and navigation transitions use the native animation engines. For apps that require the absolute best performance and the most native-feeling interactions, this approach can provide advantages.

```javascript
// React Native Navigation example (Wix)
import { Navigation } from 'react-native-navigation';

// Register screens with the navigation system
Navigation.registerComponent('HomeScreen', () => HomeScreen);
Navigation.registerComponent('ProfileScreen', () => ProfileScreen);

// Start the app with a tab-based structure
Navigation.setRoot({
  root: {
    bottomTabs: {
      children: [{
        stack: {
          children: [{
            component: {
              name: 'HomeScreen',
              options: {
                topBar: {
                  title: {
                    text: 'Home'
                  }
                }
              }
            }
          }],
          options: {
            bottomTab: {
              text: 'Home',
              icon: require('./assets/home-icon.png')
            }
          }
        }
      }, {
        stack: {
          children: [{
            component: {
              name: 'ProfileScreen'
            }
          }],
          options: {
            bottomTab: {
              text: 'Profile',
              icon: require('./assets/profile-icon.png')
            }
          }
        }
      }]
    }
  }
});

// Navigate to a new screen
Navigation.push('HomeScreen', {
  component: {
    name: 'DetailScreen',
    passProps: {
      itemId: 123
    },
    options: {
      topBar: {
        title: {
          text: 'Details'
        }
      }
    }
  }
});
```

The Wix approach requires more setup and has a steeper learning curve, but it can provide better performance for complex navigation hierarchies and more native-feeling transitions. However, it also requires more platform-specific configuration and doesn't integrate as naturally with React's component model.

For specific use cases, specialized navigation solutions can provide better experiences than general-purpose libraries. Gesture-based navigation libraries like `react-native-gesture-handler` combined with `react-native-reanimated` can create custom navigation patterns that aren't possible with standard navigation libraries.

```javascript
// Custom gesture-based navigation using react-native-gesture-handler
import { PanGestureHandler, State } from 'react-native-gesture-handler';
import Animated, { 
  useSharedValue, 
  useAnimatedGestureHandler,
  useAnimatedStyle,
  withSpring,
  runOnJS
} from 'react-native-reanimated';

function CustomSwipeNavigator({ screens, initialIndex = 0 }) {
  const translateX = useSharedValue(0);
  const currentIndex = useSharedValue(initialIndex);
  
  const gestureHandler = useAnimatedGestureHandler({
    onStart: (_, context) => {
      context.startX = translateX.value;
    },
    onActive: (event, context) => {
      translateX.value = context.startX + event.translationX;
    },
    onEnd: (event) => {
      const shouldGoNext = event.translationX < -50 && currentIndex.value < screens.length - 1;
      const shouldGoPrevious = event.translationX > 50 && currentIndex.value > 0;
      
      if (shouldGoNext) {
        currentIndex.value += 1;
        translateX.value = withSpring(-currentIndex.value * screenWidth);
      } else if (shouldGoPrevious) {
        currentIndex.value -= 1;
        translateX.value = withSpring(-currentIndex.value * screenWidth);
      } else {
        translateX.value = withSpring(-currentIndex.value * screenWidth);
      }
    },
  });
  
  const animatedStyle = useAnimatedStyle(() => {
    return {
      transform: [{ translateX: translateX.value }],
    };
  });
  
  return (
    <PanGestureHandler onGestureEvent={gestureHandler}>
      <Animated.View style={[styles.container, animatedStyle]}>
        {screens.map((Screen, index) => (
          <View key={index} style={styles.screen}>
            <Screen />
          </View>
        ))}
      </Animated.View>
    </PanGestureHandler>
  );
}
```

This custom approach allows for navigation patterns that aren't available in standard libraries, such as continuous scrolling between screens, physics-based interactions, or novel gesture combinations.

## Navigation Best Practices and Decision Guidelines

Understanding when to use different navigation patterns requires considering your users' mental models, the information architecture of your app, and the specific interaction patterns that best serve your content.

Use stack navigation for hierarchical relationships where users drill down into details and expect to retrace their steps. Examples include browsing products in an e-commerce app, reading articles in a news app, or navigating through settings screens. Stack navigation works best when there's a clear parent-child relationship between screens and users typically want to return to where they came from.

Tab navigation suits scenarios where you have distinct, equally important sections that users might want to access in any order. Social media apps use tabs for feed, search, creation, and profile because these are all primary functions that users access regularly. Tab navigation works best when you have three to five primary sections that represent different aspects of your app's core functionality.

Drawer navigation excels for organizing secondary features, settings, and navigation shortcuts that power users appreciate but casual users don't need immediately visible. Productivity apps often use drawers for features like preferences, help documentation, account management, and advanced tools that would clutter the main interface if always visible.

Modal navigation serves specific purposes like creation flows, authentication, or temporary overlay content that interrupts the main navigation flow. Use modals when you need users to complete a specific task before returning to their previous context, or when showing content that doesn't belong in the main navigation hierarchy.

Consider the cognitive load that your navigation system places on users. Complex nested navigation can confuse users and make them feel lost, while overly simplified navigation might make important features hard to discover. The best navigation systems feel invisible to users, allowing them to focus on content and tasks rather than figuring out how to move around your app.

Performance considerations also influence navigation decisions. Deep navigation stacks consume more memory, while complex tab structures with multiple nested stacks can impact startup time. Profile your app's navigation performance and consider optimizations like lazy loading screens, reducing the number of screens kept in memory, or using simpler navigation patterns for performance-critical sections.

Platform conventions deserve careful attention because users develop expectations based on other apps they use. iOS users expect swipe-from-edge gestures to go back, while Android users expect a dedicated back button. Tab bars typically appear at the bottom on iOS and can appear at the top or bottom on Android. Respecting these conventions helps your app feel familiar and intuitive to users.

Accessibility considerations require ensuring that navigation is usable with screen readers, voice control, and other assistive technologies. This means providing proper labels for navigation elements, maintaining logical focus order, and ensuring that all navigation functions are accessible through multiple interaction methods.

React Navigation has evolved into a comprehensive, flexible, and well-supported solution that handles the vast majority of navigation use cases in React Native applications. Its component-based approach aligns naturally with React's mental model, its extensive customization options accommodate diverse design requirements, and its active community provides ongoing support and development.

Remember that navigation is not just about moving between screens but about creating a logical, predictable system that helps users understand your app's structure and accomplish their goals efficiently. The best navigation systems disappear into the background, allowing users to focus on the content and tasks that brought them to your app in the first place.