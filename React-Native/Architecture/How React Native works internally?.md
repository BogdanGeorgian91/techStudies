## Introduction 

React Native has been successfully adopted by hundreds of businesses worldwide, including Uber, Discord, Microsoft, and Facebook, and is used across a whole range of industries.

It is a JavaScript-based mobile app framework that allows you to build natively-rendered mobile apps for iOS and Android, using the same codebase.

Since Java and Objective C are strongly typed languages while Javascript is not, React Native can be considered as a set of components, where each component represents the corresponding native views and components. 

For example, a native TextInput will have a corresponding RN component which can be directly imported into the JS code and used like any other React component. Hence, the developer will be writing the code just like for any other React web app but the output will be a native application.

---

## [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#old-react-native-architecture-amp-threading-model)Old React Native architecture & Threading Model 

### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#architecture)Architecture 

There are three main parts:

#### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#native-side)Native side

Most of the native code in case of `iOS` is written in `Objective C` or `Swift`, while in the case of `Android` it is written in `Java` or `Kotlin`. 

#### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#javascript-side-js-vm)Javascript side (JS VM)

The JS Virtual Machine that runs all our JavaScript code. On iOS/Android simulators and devices, React Native uses JavaScriptCore, which is the JavaScript engine that powers Safari. JavaScriptCore is an open source JavaScript engine originally built for WebKit.

In case of debugging mode, the JavaScript code runs within **Chrome** itself (instead of the JavaScriptCore on the device) and communicates with native code via **WebSocket**. Here, it will use the V8 engine. This allows us to see a lot of information on the Chrome debugging tools like network requests, console logs, etc. 

#### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#react-native-bridge)React Native Bridge

React Native bridge is a C++/Java Component which is responsible for communication between the Native and Javascript thread. Like a custom protocol used for message passing. It can be imagined as a bus where the producer layer sent some data for the consumer layer. The consumer could read the data, deserialize it and execute the required operations.

### Threading Model

When a RN application is launched, it spawns up the following threading queues:

#### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#ui-threadmain-thread-native-queue)UI Thread/Main thread (Native Queue)

This is the place where the native code is executed. It loads the app and starts the JS thread to execute the Javascript code.   
All the native modules lie in the startup, which means they will always be bundled if the user wants to access them. In other words, the native thread listens to the UI events like `'press'`, `'touch'`, etc. These events are then passed to the JS thread via the RN Bridge.

#### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#shadow-thread)Shadow Thread

It is the place where the layout of your application is calculated using React library. This cross-platform framework handles this task with the help of Facebook’s own layout engine called Yoga. It transforms flexbox layouts, calculates them and sends them to the app’s interface. It is basically like a mathematical engine which finally decides on how to compute the view positions, to be passed back to the main thread to render the view.

#### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#javascript-thread-js-queue)JavaScript thread (JS Queue)

This is the place where main bundled JS thread runs; where the entire JavaScript code is placed and compiled. The JS thread runs all the business logic, i.e., the code we write in React Native. And when the app is bundled for production, the `JavaScriptCore` runs the bundle when the user starts the app.

### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#working-process)Working process 

In most cases, a developer would write the entire React Native application in Javascript.

1. At the first start of the app, the main thread starts execution and starts loading JS bundles.
2. The JS thread sends the information on what needs to be rendered onto the screen. 
3. Then the Shadow thread uses this information to compute the layouts.
4. And then sends layout parameters/objects to the main(UI) thread.
5. Since only the main thread is able to render something on the screen, shadow thread should send generated layout to the main thread, and only then UI renders.

---

## [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#old-architectures-issues%E2%80%8B)Old Architecture's Issues​ 

The Old Architecture used to work by serializing all the data that has to be passed from the JS layer to the native layer using The Bridge. 

The Bridge had some limitations:

- **It was asynchronous:** one layer submitted the data to the bridge and asynchronously "waited" for the other layer to process them, even when this was not really necessary.
- **It was single threaded:** JS used to work on a single thread, therefore the computation that happened in that world had to be performed on that single thread.
- **It imposed extra overheads:** everytime one layer had to use the other one, it had to serialize some data. The other layer had to deserialize them. The chosen format was JSON, for its simplicity and human-readability, but despite being lightweight, it was a cost to pay.

**Say an image file converted into base64 string!!!**

---

## [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#new-architectures-improvements)New Architecture's Improvements 

The New Architecture eliminate the bridge and let the UI be controlled directly from the JavaScript Interface (JSI) using native code. The JSI is an interface that allows a JavaScript object to hold a reference to a C++ and viceversa.

This idea allowed to unlock several benefits:

- **Synchronous execution:** it is now possibile to execute synchronously those functions that should not have been asynchronous in the first place.
- **Concurrency:** it is possible from JavaScript to invoke functions that are executed on different thread.
- **Lower overhead:** there are no serialization taxes to pay, since the New Architecture don't have to serialize/deserialize the data
- **Code sharing:** by introducing C++, it is now possible to abstract all the platform agnostic code and to share it with ease between the platforms.
- **Type safety:** to make sure that JS can properly invoke methods on C++ objects and viceversa, a layer of code automatically generated has been added. The code is generated starting from some JS specification that must be typed through Flow or TypeScript.

---

## [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#new-react-native-architecture-amp-threading-model)New React Native architecture & Threading Model 

### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#fabric-renderer)Fabric Renderer 

RN Fabric allows the UI thread (where UI is drawn) to be in sync with the JS thread (where the UI is programmed). In other words, it lets React talk to each platform and manage its host view instances!

The render pipeline can be broken into three general phases:

#### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#render)Render

React executes product logic which creates a React Element Trees in JavaScript; each element is a JS object that describes what should appear on the screen, and includes props, styles, and children.  
From this tree, the renderer creates a React Shadow Tree in C++; which consists of React Shadow Nodes; each node is an object that represents a React Host Component to be mounted, and contains props that originate from JavaScript. They also contain layout information (x, y, width, height). 

#### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#commit)Commit

After a React Shadow Tree is fully created, the renderer triggers a commit. This promotes both the React Element Tree and the newly created React Shadow Tree as the “next tree” to be mounted. This also schedules calculation of its layout information.

#### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#mount)Mount

The React Shadow Tree, now with the results of layout calculation, is transformed into a Host View Tree.  
**The host** here represent the platform (e.g. Android, iOS). On Android, the host views are instances of android.view.ViewGroup, android.widget.TextView, etc. which are the building blocks of the host view tree. The size and location of each host view are based on LayoutMetrics calculated with Yoga, and the style and content of each host view are based on information from the React Shadow Tree.

### Threading Model 

React Native renderer is designed to be thread safe. Every update in React creates or clones new objects in the renderer instead of updating data structures. This allows the framework to expose thread safe and synchronous APIs to React.

The renderer uses three different threads:

#### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#ui-threadmain-thread)UI Thread/Main thread

The only thread that can manipulate [host views](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#host%20view).

#### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#javascript-thread)JavaScript thread

This is where React’s render phase is executed.

#### [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#background-thread)Background thread

Thread dedicated to layout.

---

## [](https://dev.to/nour_abdou/how-react-native-works-internally-4o42#conclusion)Conclusion 

In this article we had conceptual overview of how React Native's internals work. From the old school to the current one, which brings an improvement in startup time, and developer experience, with improved interoperability between all threads.