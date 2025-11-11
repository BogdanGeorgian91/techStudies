Below is a deep-dive reference on **how JavaScript talks to the native world inside React Native**, contrasting the **legacy “asynchronous bridge” architecture** with the **current New Architecture built on JSI, TurboModules, and Fabric**. 
The two models coexist in 0.74+, so you’ll often see both paths in one app.

---
## **1. Execution environments**

|**Concern**|**Legacy bridge**|**New architecture**|
|---|---|---|
|JavaScript runtime|JavaScriptCore (iOS) or Hermes / V8 (Android)|Same runtimes, but wrapped by **JSI** (JavaScript Interface) C++ layer|
|Thread model|- JS thread (executes React code)  - Native modules thread - Main/UI thread|Same three threads **plus**: - Fabric Mount (UI) thread - TurboModules thread pool Any of them can make **direct** calls through JSI|

---
## **2. Legacy architecture (“the bridge”)**

### **2.1 How the bridge works**

1. **Message creation (JS thread)**
    
    - React reconciler runs; produces commands (e.g. createView, setNativeProps) and module calls (NativeModules.Camera.takePicture()).
        
    - Each call is stored as a **JS object** and collected into an array during the current frame.
        
    
2. **Batched hand-off to native (Bridge)**
    
    - At the end of a frame, the array is **JSON-serialized** and posted across the bridge to the **native modules queue**. 
        
    
3. **Native side processing**
    
    - On the modules queue, React Native looks up the Java/Kotlin/Obj-C RCTBridgeModule instance by **string key**, unpacks the JSON payload, and invokes the selector / method via reflection or Java JNI.
        
    - UI operations are handed to the **main thread UIManager** which creates/updates UIView/android.view.View.
        
    
4. **Native → JS events**
    
    - Gesture recognizers, device sensors, etc. build a JSON object and queue it back over the bridge. JS receives it via DeviceEventEmitter.

### **2.2 Characteristics & constraints**

|**Property**|**Details**|
|---|---|
|**Always asynchronous**|No sync return values; a method can only resolve via callback/promise after the round-trip.|
|**Heavy serialization**|Every value (numbers, arrays, functions) is marshalled to JSON → strings → re-parsed.|
|**Copy-based memory**|JS and native cannot share data structures or pointers.|
|**Lookup by name**|Each call includes module & method **strings**; reflection adds overhead.|
|**Thread hops**|Typical call path: JS thread → Bridge queue → Native module thread → Main thread (if UI) → …|
|**Performance ceiling**|~3–4 k calls /s before jank; large props (e.g. 1 MB images) spike GC.|

---

## **3. The New Architecture (JSI + TurboModules + Fabric)**


> _Goal: remove the bridge, make crossing the boundary feel like calling a C++ function._


### **3.1 JSI: the new conduit**

- **JSI** is a minimal C++ API implemented by each JS engine (Hermes, JSC, V8).
    
- It lets native code **own actual jsi::Object handles** and lets JS hold **HostObjects**, i.e. C++ back-references. 
    
- Because both sides see the same heap, calls can be **synchronous** or asynchronous without JSON.
    

```
graph TD
  JS[JavaScript]
  Cpp[C++ Host Object]
  Native[Objective-C / Kotlin / C++]
  JS --jsi::Function call--> Cpp
  Cpp --direct invoke--> Native
  Native --optional callback--> JS
```

### **3.2 TurboModules (logic layer)**

|**Feature**|**How it works**|
|---|---|
|**Typed spec**|You define a Flow/TS interface; **CodeGen** emits C++ stubs + Kotlin/Obj-C classes.|
|**Lookup by ID**|Modules are stored in a C++ registry keyed by **numeric ID** (faster than strings).|
|**Lazy init**|A module is constructed the first time JS imports it (global.nativeTurboModuleProxy('Camera')).|
|**Sync methods**|If the spec marks a method sync, the C++ stub returns a value immediately to JSI.|
|**Threading**|Each call can run on a dedicated **TurboModules thread pool** or the UI thread if annotated.|

### **3.3 Fabric (render/UI layer)**

- **Shadow nodes in C++**: The React reconciler (still in JS) builds a shadow tree of ShadowNodes in C++ via JSI.
    
- **Concurrent render**: Multiple trees can be prepared off-thread; commits are atomic.
    
- **Mount phase**: Fabric Mount thread diffs the committed ShadowTree against actual views and issues **batched view mutations** to the platform UI layer.
    
- **Events**: Native widgets send **RCTModernEventEmitter** objects directly into the JS runtime, no JSON needed. 
    

  

### **3.4 Data-flow walkthrough (New Arch)**

|**Step**|**JS call path**|**Native → JS event**|
|---|---|---|
|1|import {Camera} from 'react-native-camera' triggers a **TurboModule lookup** (getModule('Camera')).|Platform widget posts C++ Event struct.|
|2|JSI host function calls Camera.takePictureSync().|Event is queued to the Fabric event beat; on next tick, JS callback executes on **JS thread**.|
|3|TurboModule C++ stub immediately forwards to Kotlin/Obj-C implementation (can be sync).||
|4|Return value (e.g. a jsi::Object wrapping a C++ Photo) is handed back through JSI without serialization.||

### **3.5 Advantages over the bridge**

|**Legacy pain**|**New Architecture fix**|
|---|---|
|JSON serialization overhead|**Zero serialization**; shared memory objects|
|Asynchronous-only|Sync & async both supported|
|Reflection string lookup|ID-based registry, compile-time code-gen|
|One global bridge queue|Dedicated thread pools per concern|
|No concurrent UI|Fabric supports React 18 concurrent rendering|

Performance wins of **2×–5×** on call throughput and large prop passing have been measured in RN 0.71+ benchmarks. 

---

## **4. Interoperability & migration**

1. **Gradual opt-in** – You can enable the New Architecture with newarch_enabled=true flag; legacy modules still work under the old bridge.
    
2. **Bridgeless mode** – Eventually you can remove the Java-level ReactBridge. When the last bridged module is gone, the bridge code path is stripped by the linker.
    
3. **Turbo-only modules** – Third-party libraries (e.g. React Native Reanimated 3, RN Maps 2.0) increasingly ship TurboModule/Fabric-ready builds alongside legacy shims.
    
4. **Testing tips**
    
    - Use Flipper > RN New Arch plugin to inspect Fabric trees and TurboModule calls.
        
    - Enable Hermes inspector to step into C++ HostFunctions directly.
---

## **5. Key take-aways**

- **Legacy bridge** is a _batched, JSON-serialized, asynchronous_ queue between the JS thread and native modules; simple but costly.
    
- **JSI** turns the boundary into a **direct C++ call frame**, letting both sides share objects and return synchronously.
    
- **TurboModules** modernise _logic modules_ with typed code-gen and thread control; **Fabric** rewrites the _UI pipeline_ for concurrent rendering and atomic mounts.
    
- The New Architecture is backwards compatible, but the long-term path is **bridgeless React Native**.

This should give you a complete mental model—and the vocabulary—to reason about performance issues, debug cross-boundary bugs, or plan a migration of your own libraries.