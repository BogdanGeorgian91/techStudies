Metro is the JavaScript bundler used in React Native development. Its main role is to take your project’s JavaScript code and all its dependencies, transpile and bundle them into a single optimized JavaScript file (or bundle) that can be efficiently executed on a mobile device[3][4][5].

**How Metro Works:**
- **Dependency Resolution:** Metro analyzes your codebase to build a dependency graph, identifying all modules required from the entry point[3][4][5].
- **Transformation:** Each module is transpiled (typically using Babel) into a format compatible with the target platform (Android/iOS)[3][4][5].
- **Serialization:** All transformed modules are combined (serialized) into one or more bundles, which are then loaded by the React Native app on the device[4][5].

**Key Features:**
- Fast incremental builds and sub-second reloads for a smooth developer experience[5].
- Handles modern JavaScript features and assets (like images) for mobile platforms[5].
- Configurable via `metro.config.js` in your project[3].

In summary, Metro is the "secret sauce" that bundles and prepares your React Native JavaScript code to run efficiently on mobile devices, similar to how Webpack works for web applications[3][4][5].

Sources
[1] Metro config for out of tree platforms · React Native for Windows + macOS https://microsoft.github.io/react-native-windows/docs/metro-config-out-tree-platforms
[2] Bundling | React Native Developer Tools https://microsoft.github.io/rnx-kit/docs/guides/bundling
[3] Demystifying Metro Builder: React Native’s Bundler https://blog.stackademic.com/demystifying-metro-builder-react-natives-bundler-d218ae84b113?gi=3bfb1e4327a1
[4] Concepts | Metro https://metrobundler.dev/docs/concepts/
[5] Metro Bundler in React native | Codementor https://www.codementor.io/@rishabhsharma984/metro-bundler-in-react-native-wub92qcp5
[6] RNR 292 - RNR Explains: Metro Bundler https://www.youtube.com/watch?v=M0I5i-Y_8HI
[7] Metro · React Native https://reactnative.dev/docs/metro
[8] metro-bundler https://www.npmjs.com/package/metro-bundler/v/0.7.1
