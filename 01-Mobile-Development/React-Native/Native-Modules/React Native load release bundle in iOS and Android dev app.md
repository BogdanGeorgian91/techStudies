October 2023

When using React Native on Android you can enter the dev menu and choose not to use a development JS bundle. Usually useful to catch performance regressions in your JS code. For iOS this option is not present on the dev menu, but you can still achieve it if you hack around the native code.

In your AppDelegate, replace

```obj-c
return [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index"];
```

with

```obj-c
NSString *packagerServerHostPort = [[RCTBundleURLProvider sharedSettings] packagerServerHostPort];
return [RCTBundleURLProvider jsBundleURLForBundleRoot:@"index"
    packagerHost:packagerServerHostPort
      enableDev:NO
    enableMinification:NO];
```