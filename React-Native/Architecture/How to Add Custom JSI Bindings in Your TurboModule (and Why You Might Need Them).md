
The React Native New Architecture, powered by Fabric and TurboModules, brings major improvements in performance and developer experience. For most cases, TurboModules are the go-to solution: type-safe, auto-generated bindings that eliminate boilerplate and bridge overhead. But sometimes you need more. Sometimes the abstractions don't cover your exact use case.

That's where the JavaScript Interface (JSI) comes in!

![[JSI_RN.png]]

JSI is the low-level API that underpins TurboModules. It lets you inject custom functions directly into the JavaScript runtime, hold references to JS objects, and move data without serialization. It's the tool you reach for when TurboModules alone aren't enough.

## Why Go Beyond TurboModules?

Here's a real example. While working on audio transcription, we needed to pass a large chunk of binary data (an ArrayBuffer) from JavaScript into a native speech-to-text module. TurboModules didn't yet provide direct support for ArrayBuffer. Falling back to base64 encoding wasn't an option. It was too slow and memory-heavy. The data was already in memory; we just needed to expose it directly.

This is where custom JSI bindings shine. Instead of unnecessary conversions, they give you access to the JS engine's memory.

## The Escape Hatch: RCTTurboModuleWithJSIBindings

React Native provides a protocol exactly for these scenarios. If your TurboModule implements `RCTTurboModuleWithJSIBindings`, you can inject your own bindings when the module initializes.

### Step 1. Declare conformance in your header

```objc
#import <ReactCommon/RCTTurboModuleWithJSIBindings.h>

@interface MyCustomModule <RCTTurboModuleWithJSIBindings>
@end
```

### Step 2. Implement the protocol method

```objc
- (void)installJSIBindingsWithRuntime:(facebook::jsi::Runtime &)rt
                          callInvoker:(const std::shared_ptr<facebook::react::CallInvoker> &)jsInvoker {
  rt.global().setProperty(runtime, "__MyCustomGlobal", "Hello World!");
}
```

This gives you direct access to the JSI runtime and call invoker.

## Example: Passing an ArrayBuffer to Native

With JSI, we can create a native function that directly accepts an ArrayBuffer from JavaScript without any serialization overhead. We can inject a global function, `__native__processBytes__(arrayBuffer)`, that can access the raw data.

Here's how you can do it, step-by-step, inside the `installJSIBindingsWithRuntime` method:

### Step 1: Define the Host Function

First, create a JSI function from a C++ lambda. This function will be callable from JavaScript. The body of the function is where you'll add the logic to handle the ArrayBuffer.

```cpp
auto processFunc = jsi::Function::createFromHostFunction(
    rt,
    jsi::PropNameID::forAscii(rt, "__native__processBytes__"),
    1, // expects one argument (arrayBuffer)
    [](jsi::Runtime &rt, const jsi::Value &thisVal, const jsi::Value *args, size_t count) -> jsi::Value {
      // function body goes here
      return jsi::Value::undefined();
    });
```

Let's quickly break down the arguments passed to `createFromHostFunction`:

- `rt`: This is a reference to the JSI Runtime, the execution context for the JavaScript world. You need it to create and interact with any JS values.
- `jsi::PropNameID::forAscii(...)`: This creates an identifier for the function's name. JSI uses this ID for faster lookups than using raw strings.
- `1`: This is the paramCount, telling the JSI runtime that your function expects one argument from JavaScript.
- `[](...) { ... }`: This is the C++ lambda that acts as the Host Function. It's the native code that runs when your function is called from JavaScript. It receives its own arguments to access the runtime, the JS `this` context, and the arguments passed from JS.

### Step 2: Validate Input and Access Raw Data

Inside the lambda, validate that the first argument is an ArrayBuffer.

```cpp
if (count < 1 || !args[0].isObject() || !args[0].asObject(rt).isArrayBuffer(rt)) {
  throw jsi::JSError(rt, "First argument must be an ArrayBuffer");
}
```

Then, get a direct, zero-copy pointer to its underlying data.

```cpp
auto arrayBuffer = args[0].asObject(rt).getArrayBuffer(rt);

const void *data = arrayBuffer.data(rt);
size_t size = arrayBuffer.size(rt);

NSLog(@"Received ArrayBuffer of size: %zu", size);
```

### Step 3: Register the Function Globally

Finally, attach your newly created JSI function to the global JavaScript object, making it accessible as `global.__native__processBytes__`.

```cpp
rt.global().setProperty(rt, "__native__processBytes__", std::move(processFunc));
```

### Step 4: Call it from JavaScript

```javascript
const bytes = new Uint8Array([1, 2, 3, 4]);
global.__native__processBytes__(bytes.buffer);
```

This demonstrates the core concept: accessing JavaScript's memory directly from native code.

For a more advanced example covering handling array buffers and returning a promise, you can refer to the code inside React Native AI.

## Why This Matters

TurboModules cover most native integration needs. But when you hit an edge case, such as unsupported type or advanced optimizations, JSI bindings give you the flexibility to:

- Expose custom global functions
- Work with raw JS engine objects
- Bridge async native APIs directly into Promises
- Optimize data transfer without serialization

## Array Buffers are coming to Turbo Modules

We're actively working on bringing first-class ArrayBuffer support directly to TurboModules. This edge case is very popular, so we decided to bring a solution to everyone, allowing you to use a much simpler CodeGen instead.

## Closing Thoughts

If TurboModules are the safe, paved road, JSI is the open terrain. You rarely need it, but knowing how to reach for it unlocks the full potential of React Native's native layer.