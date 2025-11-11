# 1. The Bridge’s Silent Tax: JSON Isn’t Free

Every time you pass data between JavaScript and native modules, React Native serializes it to JSON. This seems harmless until you’re shuttling massive objects (think deeply nested API responses) across the bridge repeatedly. The result? Unseen latency and garbage collection thrashing.

**The Fix**:

- Flatten data structures _before_ they cross the bridge.
- Use batched updates for native modules (e.g., `NativeModules.YourModule.processBatch([...items])`).
- For frequent communication, leverage `WritableNativeArray` or `WritableNativeMap` to bypass JSON serialization entirely.

# 2. The Animation Illusion: “Use Native Driver” Isn’t a Magic Spell

You’ve heard “always use the native driver for animations,” but what happens when your app still stutters? The devil’s in the details: **not all style properties are offloadable**. Animating `flexGrow` or `zIndex`, for example, forces JavaScript thread involvement, defeating the purpose.

**The Fix**:

- Stick to transform and opacity animations where possible.
- Use `useNativeDriver: true` _only_ with supported properties (check React Native’s docs for the latest list).
- For unsupported cases, consider pre-computing animations with libraries like Reanimated 3.

# 3. The Navigation Minefield: Stack Bloat

Navigation libraries like React Navigation simplify routing but hide a dirty secret: **unmounted screens aren’t always destroyed**. Stack navigators keep previous screens in memory by default, leading to hidden component trees, stale event listeners, and memory leaks.

**The Fix**:

- Use `detachInactiveScreens: true` in stack navigators to unmask this issue.
- Manually cleanup subscriptions/interval timers in `useEffect` return functions.
- For tab navigators, opt for `lazy: true` and `unmountOnBlur: true` to avoid background tab bloat.

# 4. The CSS Paradox: Flexbox Overkill

Flexbox is React Native’s layout workhorse, but complex nested flex structures can force the UI thread to perform layout calculations multiple times per frame. One developer’s “responsive grid” is another’s 16ms dropped frame.

**The Fix**:

- Simplify layouts: Replace nested `Views` with `<FlatList numColumns={2} />`for grids.
- Use fixed dimensions where possible (yes, even in the age of responsive design).
- Test with **React Native’s LayoutAnimation** to visualize rendering bottlenecks.

# 5. The Third-Party Lie: “Optimized” Libraries

That slick-looking npm package with “performance” in its description? It might be importing 15 deprecated sub-dependencies, including a polyfill for `Object.entries` that bloats your bundle. Worse, some libraries trigger synchronous bridge calls under the hood.

**The Fix**:
- Audit dependencies with `npm ls --depth=5` to spot nested bloat.
- Prefer lightweight alternatives (e.g., `date-fns` over `moment`).
- Use `react-native-bundle-visualizer` to see exactly what’s in your bundle.

# 6. The Debugger Deception: Profiling Blind Spots

React Native’s performance tools (e.g., Flipper) are lifesavers — until they lie to you. For example, the JavaScript thread might appear idle in profiles while the app freezes, because the real villain is **UI thread congestion** (e.g., excessive shadows or complex gradients).

**The Fix**:
- Enable **GPU Overdraw Visualization** on Android to spot redundant rendering.
- On iOS, use Xcode’s **Core Animation Instrument** to track off-screen passes.
- Test on low-end devices _during development_, not just simulators.

# The Uncomfortable Truth: Performance Isn’t a Feature. It’s a Habit.

React Native’s “silent killers” thrive in complacency. They don’t announce themselves with crashes or warnings — they chip away at your app’s quality until users abandon ship. The solution isn’t a one-time audit; it’s baking performance hygiene into your workflow:

- Profile **early and often**, even if the app “feels” fast.
- Question every dependency, animation, and layout.
- Embrace the uncomfortable: Sometimes, dropping React Native for a native screen is the right call.