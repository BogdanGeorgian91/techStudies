# Debugging React Native in Production Mode

Kevin Sullivan edited this page on Aug 31, 2018 · [3 revisions](https://github.com/EdgeApp/edge-react-gui/wiki/Debugging-React-Native-in-Production-Mode/_history)

# Debugging React Native Production-mode Issues

[](https://github.com/EdgeApp/edge-react-gui/wiki/Debugging-React-Native-in-Production-Mode#debugging-react-native-production-mode-issues)

If your app works fine as a React Native debug build, but fails as a React Native production build, you have a problem. Production builds eliminate almost all of the logging and debugging tools you really need to figure out what's going on. Fortunately, there's a way to use a production-mode Javascript bundle inside a debug-mode React Native app!

## Building a Fake Release Bundle

[](https://github.com/EdgeApp/edge-react-gui/wiki/Debugging-React-Native-in-Production-Mode#building-a-fake-release-bundle)

The first step is to use the React Native packager to build a production-mode Javascript bundle. In one terminal window, do:

```
react-native start --nonPersistent
```

Then, in another window, do:

```
curl -o index.bundle localhost:8081/index.bundle\?platform\=ios\&dev\=false\&minify\=false
curl -o index.bundle localhost:8081/index.bundle\?platform\=android\&dev\=false\&minify\=false
```

You can use different combinations of the `dev` and `minify` flags to tweak the build. Sometimes the problem is minification, and sometimes it's the `dev` setting. The `dev` setting does three things:

- Defines the `__DEV__` variable. This affects a huge amount of stuff at run-time, and explains most of the dev-mode behavior differences.
- Enables the `babel-plugin-transform-react-jsx-source` syntax transformation. This puts JSX source locations into component props.
- Puts the original source file names in the bundle output.

In the future, once React merges [this pull request](https://github.com/facebook/react-native/pull/16456), you can use `react-native build` command to bypass the extra cURL step.

## Faking the React Native Packager

[](https://github.com/EdgeApp/edge-react-gui/wiki/Debugging-React-Native-in-Production-Mode#faking-the-react-native-packager)

Now that you have your production-mode JS bundle, you need to run the bundle inside the debug-mode native code. The easiest way to do this is to fake the React Native packager's HTTP interface. Just do an `npm i express` to install Express.js, then save the following code to your project directory and run it with `node`:

```js
const express = require('express')
const app = express()

// Log all requested URL's:
app.use(function (req, res, next) {
  console.log(req.url)
  res.setHeader('Content-Type', 'application/javascript')
  next()
})

// Fake the packager:
app.use('/', express.static('.'))
app.use('/assets', express.static('.'))
app.get('/status', function (req, res) {
  res.send('packager-status:running')
})

app.listen(8081, function () {
  console.log('Serving currency directory as localhost:8081!')
})
```

Now you can launch the debug-mode native app using the normal `react-native run-ios`. The native app talks to the fake packager using the following endpoints, which serve the bundle we made earlier:

- localhost:8081/index.bundle?platform=ios&dev=true&minify=false - The main JS bundle
- localhost:8081/assets/path/to/some/[image@2x.png](mailto:image@2x.png)?platform=ios&hash=beecdc37460ca27566cd3c5625140b08 - Individual non-JS assets
- localhost:8081/status - Indicates that the packager is running

If you try to do remote debugging, it won't work in this setup. Debugging requires the following extra endpoints, which the fake packager doesn't support:

- ws://localhost:8081/debugger-proxy?role=client
- ws://localhost:8081/debugger-proxy?role=debugger&name=Chrome
- localhost:8081/launch-js-devtools
- localhost:8081/open-stack-frame
- localhost:8081/symbolicate

Since there is no remote debugging, you'll need to do your debugging using console logging. You can use `react-native log-ios` to see the logs, or use the Xcode logging window. If you get a weird error message every second, you might try:

```
react-native log-ios | grep -v nw_connection_get_connected_socket_block_invoke
```

This just hides the error message so you aren't distracted by it. The error itself is apparently harmless.

## Hacking Around in Bundle Land

[](https://github.com/EdgeApp/edge-react-gui/wiki/Debugging-React-Native-in-Production-Mode#hacking-around-in-bundle-land)

Now that you have a single giant bundle file, it's time to start debugging the problem. Just edit the bundle directly, then reload the app using the debug menu. The fake package server will always serve the latest file from disk. Working directly in the bundle is generally easier than editing the original source code, since it avoids the packaging step, and since all the stack traces will have bundle line numbers.

The normal `console.log` function doesn't work during the early initialization process. In this case, you can use the following code to send stuff to the native logs:

```js
global.nativeLoggingHook('your string', 'log')
```

Other React Native goodies available on the global object at first boot-up include:

- nativeFlushQueueImmediate
- nativeCallSyncHook
- nativeLoggingHook
- nativePerformanceNow
- nativeInjectHMRUpdate
- nativeModuleProxy

Besides these, the native code also provides the following standard JS objects (at least on iOS):

> Array, ArrayBuffer, Atomics, Boolean, console, DataView, Date, decodeURI, decodeURIComponent, encodeURI, encodeURIComponent, Error, escape, eval, EvalError, Float32Array, Float64Array, Function, Infinity, Int16Array, Int32Array, Int8Array, Intl, isFinite, isNaN, JSON, Map, Math, NaN, Number, Object, parseFloat, parseInt, Promise, Proxy, RangeError, ReferenceError, Reflect, RegExp, Set, SharedArrayBuffer, String, Symbol, SyntaxError, TypeError, Uint16Array, Uint32Array, Uint8Array, Uint8ClampedArray, undefined, unescape, URIError, WeakMap, WeakSet

Anything not on the list comes from the Javascript bundle, including things like `fetch` and `__DEV__`. When the `__DEV__` flag is set, the React Native runtime hacks up the native console (and probably a lot of other stuff) with a bunch of debugging features. This all happens in Javascript.

The native-code modules enter the system via the `global.nativeModuleProxy` object. Thus, the native code has very little influence on the Javascript code. If you are seeing breaks in your production-mode app, it's probably because of something going on in Javascript. If the problem lies in native code, you can't reproduce it with this setup, since the native code is still running in debug mode.

## Android

[](https://github.com/EdgeApp/edge-react-gui/wiki/Debugging-React-Native-in-Production-Mode#android)

On Android, the builtin JS objects are a bit weaker:

> Array, ArrayBuffer, Boolean, console, DataView, Date, decodeURI, decodeURIComponent, encodeURI, encodeURIComponent, Error, escape, eval, EvalError, Float32Array, Float64Array, Function, Infinity, Int16Array, Int32Array, Int8Array, isFinite, isNaN, JSON, Map, Math, NaN, Number, Object, parseFloat, parseInt, RangeError, ReferenceError, RegExp, Set, String, SyntaxError, TypeError, Uint16Array, Uint32Array, Uint8Array, Uint8ClampedArray, undefined, unescape, URIError, WeakMap

There is also a different list of native goodies created by React Native:

- nativeFlushQueueImmediate
- nativeCallSyncHook
- nativeLoggingHook
- nativePerformanceNow
- nativeQPLMarkerStart
- nativeQPLMarkerEnd
- nativeQPLMarkerTag
- nativeQPLMarkerAnnotate
- nativeQPLMarkerNote
- nativeQPLMarkerCancel
- nativeQPLTimestamp
- nativeModuleProxy