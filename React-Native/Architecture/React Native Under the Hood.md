# React Native Under the Hood: How Your JS, iOS, and Android Code Run Together?

Ever wondered how React Native actually runs your code?  
When working with React Native, it's essential to understand how JavaScript, iOS, and Android handle code execution and compilation.

Unlike native platforms that use compiled languages, JavaScript does not have a traditional build system but undergoes a different transformation process. You write JavaScript/TypeScript, but it controls native iOS and Android components. 

It's a multi-stage process involving transpilation, bundling, JavaScript execution, communication, and native rendering.

## [](https://dev.to/nour_abdou/react-native-under-the-hood-how-your-js-ios-and-android-code-run-together-3k2e#the-flow)The Flow

1- Development: Write TS code.  
2- Build: TS -> JS (transpilation) -> JS Bundle (Metro).  
3- Launch: Native app shell starts, loads JS bundle into a JS Engine (Hermes/JSC).  
4- Execution: JS code runs, React determines UI structure.  
5- Rendering: React instructions are sent via Bridge/JSI to native View Managers, which create/update native UI elements (UIView, android.view.View).     
6- Interaction: User interacts with native UI elements -> Events sent via Bridge/JSI back to JS -> JS event handlers run -> State updates -> UI potentially re-renders (back to step 5).  
7- Native APIs: JS calls Native Modules -> Bridge/JSI relays call to native code -> Native code executes platform API -> Result sent back via Bridge/JSI to JS.

Let's break down the execution and compilation flow for each part.

---

## [](https://dev.to/nour_abdou/react-native-under-the-hood-how-your-js-ios-and-android-code-run-together-3k2e#javascript-the-brains-)JavaScript (The Brains 🧠)

While JavaScript is typically an interpreted language, unlike natively compiled languages like Swift or Kotlin, React Native involves a form of "compilation" steps before your code actually runs on the device.  
But In case your application is written in TypeScript benefiting from static typing, interfaces, and other TS features, the first step is: 

### [](https://dev.to/nour_abdou/react-native-under-the-hood-how-your-js-ios-and-android-code-run-together-3k2e#typescript-to-javascript-transformation-build-time)📌 TypeScript to JavaScript Transformation **(Build Time)**

- Before your code can run, it needs to be converted into JavaScript, which is what React Native's runtime environment understands. The TS compiler (tsc) is responsible for this.
- During the build process (when you run npx react-native run-ios or run-android), tsc (often invoked implicitly by the Metro bundler) reads your .ts and .tsx files. It performs type checking and then removes all the TypeScript-specific syntax (like type annotations, interfaces, etc.).

➡️ Output: The output of this step is standard JavaScript code (.js) that is functionally equivalent to your TypeScript code, just without the type information.

**Now**, Your JS logic is transpiled and bundled by Metro during the build Time. If using Hermes, it's further compiled to optimized bytecode.   
This final package runs within a dedicated JS environment on the device, communicating with the native side to control the app's UI and functionality: 

### [](https://dev.to/nour_abdou/react-native-under-the-hood-how-your-js-ios-and-android-code-run-together-3k2e#javascript-bundling-build-time)📌 JavaScript Bundling **(Build Time)**

React Native uses Metro, a specialized JavaScript bundler, to handle this. Metro performs two key tasks:

- Transpilation: Due to evolving JavaScript (ECMAScript) standards and the use of JSX, your code needs to be transformed into a version that the target JavaScript engine (e.g., Hermes) can understand. Metro uses Babel under the hood for this; Babel rewrites modern JavaScript syntax and JSX into compatible code, guided by React Native's specific Babel preset to match the capabilities of the engine being used.
- Bundling: Metro bundles all your JavaScript files and dependencies into one (or a few) efficient JS files (bundles), optimized for loading at runtime (understandable for JS engine).

➡️ Output: The JS Bundle; This is typically a single .js file (e.g., index.bundle) containing all the application logic ready to be executed.   

### [](https://dev.to/nour_abdou/react-native-under-the-hood-how-your-js-ios-and-android-code-run-together-3k2e#running-the-javascript-runtime)📌 Running the JavaScript **(Runtime)**

When you build your React Native app, a Native "shell" application is created for both iOS (using Objective-C/Swift) and Android (using Java/Kotlin). This shell contains the necessary native code to host React Native.

#### [](https://dev.to/nour_abdou/react-native-under-the-hood-how-your-js-ios-and-android-code-run-together-3k2e#execution-js-engine-javascriptcore-vs-hermes)Execution JS Engine (JavaScriptCore vs. Hermes):

This native shell embeds a JS engine (a program that executes JS code). React Native can use different engines:

- JavaScriptCore (JSC): Historically common, it's the engine used by Safari. It's also available on Android but less used now.
    
- Hermes: An open-source JavaScript engine optimized by Meta specifically for running React Native appsand It's often the default for new React Native projects on both platforms.  
    A key advantage of Hermes is that during the app build phase, it pre-compiles the JavaScript bundle into bytecode.   
    This bytecode format allows for significantly faster app startup times (Time To Interactive - TTI) and reduced memory usage compared to interpreting raw JS or using Just-In-Time (JIT) compilation on the device.
    

➡️ So.. When you launch the app, the native shell starts the JS engine and loads the JS bundle created by Metro. Then, The JS engine starts executing your application's code, beginning with your entry point (usually index.js). React starts rendering your components in memory.

---

We'll talk now about Communication & Native Modules and Native Components.

## [](https://dev.to/nour_abdou/react-native-under-the-hood-how-your-js-ios-and-android-code-run-together-3k2e#ios-swiftobjectivec-the-looks-amp-feel-on-apple)iOS (Swift/Objective-C - The Looks & Feel on Apple🍎)

Unlike JavaScript, Swift and Objective-C are compiled languages. The iOS build system compiles source code (including the native parts of React Native itself and any native modules you add) into machine code before execution, ensuring optimized performance on Apple devices.

React Native for iOS relies on Xcode's build system, which uses Clang compiler (for Objective-C) and LLVM toolchain (for Swift) to convert human-readable source code into an optimized binary specific to the iPhone/iPad processor (such as .app bundle).

Here are the key steps:

- Source Compilation: Swift and Objective-C files are compiled into machine code using Clang/LLVM.
- Linking: All compiled code, system frameworks, and external libraries (from CocoaPods) are linked together.
- Signing: Apple requires apps to be signed with valid certificates and provisioning profiles before they can be run on a device.
- Packaging: The final binary (.ipa file) is generated, containing the app and its resources.

➡️ Result: This compiled code is packaged into the .app bundle that gets installed on the user's device. It runs directly on the hardware when the app starts.

## [](https://dev.to/nour_abdou/react-native-under-the-hood-how-your-js-ios-and-android-code-run-together-3k2e#android-kotlinjava-the-looks-amp-feel-on-android-)Android (Kotlin/Java - The Looks & Feel on Android 🤖)

Similar to iOS (but a bit more layered process), Android uses a compiled build system, relying on Gradle, which automates the process of building, testing, and packaging an Android app.

React Native for Android compiles Java/Kotlin source code into Dalvik Executable (DEX) format, which is optimized for execution by the Android Runtime (ART).

Here are the key steps:

- Source Compilation: Java/Kotlin files are compiled into .class bytecode files using the javac (for Java) and kotlinc (for Kotlin) compilers.
- DEX Conversion: The compiled .class files (JVM Bytecode) are converted into .dex files (Dalvik Bytecode), optimized for the Android Runtime using the DEX compiler (d8).
- Resource Processing: XML layouts, images, and other assets are bundled.
- Linking: External libraries and dependencies (managed by Gradle) are linked.
- Signing: Android requires apps to be signed with a valid keystore file.
- Packaging: The final .apk or .aab file is generated for installation on devices.

➡️ Result: This compiled/optimized code is packaged into an .apk or .aab file for distribution.

### [](https://dev.to/nour_abdou/react-native-under-the-hood-how-your-js-ios-and-android-code-run-together-3k2e#communication-the-bridge-jsi-runtime)📌 Communication: The Bridge / JSI **(Runtime)**

This is the core of how JavaScript interacts with the native platform. There are two main architectures:  
1- The Bridge (Older Architecture):  
The Bridge acts as a message broker between your JS code and the native iOS/Android platform.  
It asynchronously sends serialized data (commands, UI updates) from JS to Native.  
Then Native code executes these commands and sends results or user interaction events back to JS.  
But, Its Asynchronous nature can lead to latency, serialization/deserialization overhead ("Bridge Tax"), and potential bottlenecks if too many messages are sent. 

2- JavaScript Interface (JSI) (New Architecture - Fabric & TurboModules):  
React Native's new architecture replaces the asynchronous Bridge with the JavaScript Interface (JSI).  
JSI allows direct, synchronous calls between JavaScript and Native (via C++), eliminating serialization overhead.  
This powers Fabric (a faster UI rendering system) and TurboModules (faster, lazy-loaded native modules).  
The result is significantly improved performance, responsiveness, and better feature support.

For more about these architectures, Checkout my Article about [How React Native works internally?](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#New%20Architecture's%20Improvements)

### [](https://dev.to/nour_abdou/react-native-under-the-hood-how-your-js-ios-and-android-code-run-together-3k2e#native-modules-and-native-components-runtime)📌 Native Modules and Native Components (Runtime)

- Native Modules Expose native platform APIs (like camera, geolocation, storage, Bluetooth, custom native code) to your JavaScript code.  
    When your JS calls NativeModules.MyCamera.takePicture(), the JSI translates this into a call to the corresponding native Objective-C/Swift (iOS) or Java/Kotlin (Android) method. The native code executes, and the result is sent back to JS.
    
- Native Components (View Managers): These are the counterparts to React components like `<View>, <Text>, <Image>`, etc.  
    When React decides to render a `<View>`, it doesn't create an HTML `<div>`, but it sends instructions (via JSI) to the native side.   
        
    -> **On iOS**, a native "View Manager" receives these instructions and creates/updates an actual native `UIView`.  
    -> **On Android**, a native "View Manager" creates/updates an actual native `android.view.View`.   
    

This ensures that your UI looks and feels native because it is using native platform UI elements. The layout logic (like Flexbox) is calculated (often on a background thread) and then applied to these native views on the main UI thread.   

---

### [](https://dev.to/nour_abdou/react-native-under-the-hood-how-your-js-ios-and-android-code-run-together-3k2e#running-the-application-on-the-device)Running the application on the device

Testing is an essential part of the mobile app development workflow. Unlike web applications, which can be tested directly in a browser, mobile apps require simulators (emulators in Android) or physical devices for testing. Simulators allow developers to run and debug apps in a controlled environment that mimics real-world device behavior.  
■ **Android**  
You can interact with Android emulators using the following methods:

- Android Studio AVD Manager: The built-in tool for managing Android emulators. You can create, configure, and launch virtual devices here.
- Command Line (adb and emulator commands):
- emulator -list-avds—lists all available virtual devices.
- emulator @Pixel_6_API_34—launches a specific device.
- adb devices—lists running emulators and connected physical devices. CLI tools such as Expo CLI or React Native Community CLI use these commands under the hood to provide a better developer experience.

■ **iOS**  
You can interact with iOS simulators via the following:

- Xcode: Apple's built-in tool for running iOS apps on a simulated device. You can launch it via Xcode > Open Developer Tool > Simulator.
- Command Line (xcrun simctl commands): ➡️ xcrun simctl list—lists available simulators. ➡️ xcrun simctl boot "iPhone 15"—boots a specific simulator. ➡️ xcrun simctl shutdown all—shuts down all running simulators.