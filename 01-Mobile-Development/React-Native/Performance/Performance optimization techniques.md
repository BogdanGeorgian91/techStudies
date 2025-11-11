# Rendering Optimizations

✅ Use React.memo or PureComponent
	•	Prevents unnecessary re-renders of functional or class components when props haven’t changed.

✅ Use useCallback and useMemo
	•	Prevents functions and values from being re-created on every render.

✅ Avoid Inline Functions/Objects in JSX

// ❌ Inefficient
<Button onPress={() => doSomething()} />

// ✅ Efficient
const handlePress = useCallback(() => doSomething(), []);
<Button onPress={handlePress} />

✅ Use FlatList or SectionList for rendering lists
	•	Virtualized and efficient list rendering
	•	Use keyExtractor and avoid scrollEnabled={false} unless needed

✅ Enable removeClippedSubviews in FlatList for long lists


# Memory, Resource Management

✅ Unmount unused screens
	•	Use navigation options like unmountOnBlur: true in React Navigation.

✅ Clean up timers, intervals, and subscriptions
	•	Use useEffect clean-up functions

✅ Don’t load all images/resources at once
	•	Use lazy loading and conditional rendering

⸻

# Image Optimization

✅ Compress images before bundling
	•	Use tools like TinyPNG or ImageOptim

✅ Use react-native-fast-image
	•	A performant image component with caching support

✅ Use smaller resolution images for low DPI devices

⸻

# Navigation Optimization

✅ Use React Navigation with native stack (@react-navigation/native-stack)
	•	Faster transitions than JS-based stack

✅ Defer navigation-heavy screens
	•	Load complex screens only when needed

⸻

# Bundle Size & JS Performance

✅ Enable Hermes engine
	•	Smaller JS bundle, faster startup and memory usage

✅ Remove unused libraries
	•	Tree-shake libraries like lodash, moment.js

✅ Code split or lazy-load non-critical features

✅ Avoid heavy computation in JS thread
	•	Move to native modules or background thread (e.g., using JSThread, worker_threads, or react-native-reanimated)

⸻

# Avoid Reflows & Layout Thrashing

✅ Minimize frequent layout changes (e.g., resizing, flex direction changes)

✅ Batch state updates where possible

⸻

# Animations

✅ Use react-native-reanimated (v2+)
	•	Offloads animations to native thread

✅ Avoid Animated.timing(...) on high-frequency components unless necessary

⸻

# Startup Time Optimization

✅ Use AppRegistry.runApplication only after initial setup is ready

✅ Reduce JS bundle size

✅ Load less data on startup; defer API calls

⸻

# Networking

✅ Cache API results where possible

✅ Debounce frequent API calls (e.g., with lodash.debounce)

✅ Use a fast, efficient library like react-query or TanStack Query to handle caching, retries, and pagination

⸻

# Native-Specific Optimizations

✅ Optimize native-modules – avoid large synchronous calls

✅ Minimize bridging between JS <-> Native

✅ Avoid unnecessary renders triggered by native events (e.g. sensors, GPS)

⸻

# Debugging Tools for Performance

Tool	Use
Flipper	Inspect component re-renders, network, layout, etc.
React DevTools	Analyze props/state and see render tree
Hermes Profiler	Analyze JS performance and memory
Xcode Instruments	Measure memory, CPU, and UI performance on iOS
Android Profiler	Track UI jank and slow renders on Android


⸻

# Bonus: Build Config
	•	Use release builds (--variant=release) to benchmark true performance
	•	Disable console.log() in production
