# 1. The Bridge Isn’t Dead — It’s Just Hiding in Plain Sight

React Native’s “new architecture” (TurboModules, JSI) promised to retire the infamous “bridge” bottleneck. But here’s the truth: **the bridge still exists for 80% of apps**.

- **Why?** Most third-party libraries (including popular ones like `react-native-maps` or `react-native-video`) still rely on the old bridge. Every time you call a native module, data gets serialized into JSON and passed between threads—a hidden tax on performance.
- **The Fix:** Audit your dependencies. Use tools like [react-native-bundle-visualizer](https://github.com/IjzerenHein/react-native-bundle-visualizer) to identify bridge-heavy libraries. Replace them with JSI-powered alternatives (e.g., `react-native-reanimated v2` for animations).

# 2. The “Invisible” Thread War: JS vs. UI vs.… Background?

React Native runs on three threads: JavaScript (JS), UI (Native), and a shadow thread for layout calculations. But here’s where things get messy:

- **Background Thread Sabotage:** Ever noticed laggy animations even with `useNativeDriver: true`? Some native modules (like Bluetooth or GPS handlers) quietly hog the UI thread. Use `react-native-performance` to profile thread activity and catch offenders.
- **Case Study:** A food delivery app saw a 40% drop in frame rates when location updates fired. Solution? Offload non-UI tasks to **Web Workers**(yes, in React Native — here’s [how](https://github.com/devfd/react-native-workers)).

# 3. Memory Leaks: Not Just a Native Problem

“JavaScript manages memory automatically,” they said. But React Native’s hybrid environment creates **memory traps**:

- **Event Listener Zombies:** Subscribing to `AppState` or `Keyboard` events without cleanup? Those callbacks keep component corpses alive. Always use `useEffect`’s cleanup function.
- **Native Side Leaks:** A `setInterval` in JS that triggers a native module can create a reference loop. Tools like **Xcode’s Memory Graph Debugger** or **Android Profiler** are your best friends.

# 4. The Dark Side of “Fast Refresh”

Fast Refresh is magical — until it isn’t.

- **Stale Closure Syndrome:** Hot reloading can preserve outdated state or context, leading to phantom bugs. If your app behaves oddly after a refresh, **fully restart the Metro bundler**.
- **Pro Tip:** Use `react-native-debugger` to monitor state changes and network requests in real time.

# 5. The Final Boss: Unnecessary Re-renders… From Native Modules

You’ve memoized every component, but your app still re-renders like crazy. The culprit? **Native modules emitting events too frequently**.

- Example: A `scrollViewDidScroll` event firing 60 times/sec can flood the JS thread. Throttle events at the native layer or use `NativeEventEmitter.setListener` with caution.
- **Nuclear Option:** Rewrite the native module to batch events (Swift/Obj-C or Kotlin/Java).