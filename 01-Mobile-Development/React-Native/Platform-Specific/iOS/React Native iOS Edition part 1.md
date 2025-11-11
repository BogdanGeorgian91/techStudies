# 1. “Just Use Expo” Isn’t Always the Answer

I remember my first React Native project. Everyone said, “Use Expo! It’s magic!” And it _is_ — until you need a native module Expo doesn’t support.

**The Problem:**  
I built a fitness app that required a custom Bluetooth module to connect to heart rate monitors. Expo’s managed workflow? No Bluetooth support. I had to eject, which meant rewriting my entire build process, dealing with Xcode errors I didn’t understand, and losing Expo’s over-the-air updates.

**The Fix:**

- **Research before choosing Expo.** If your app needs hardware access (camera, Bluetooth, sensors), check [Expo’s module list](https://docs.expo.dev/versions/latest/).
- Use `**expo prebuild**` to generate native files _without_ fully ejecting. You get the best of both worlds.

**Lesson Learned:**  
Expo is fantastic for prototypes and apps that stay within its sandbox. But if you’re building something complex, start with the bare React Native template.

# 2. iOS Build Times Will Test Your Patience

React Native’s “hot reloading” is a blessing… until you realize iOS builds take _forever_.

**The Problem:**  
I once spent 3 hours debugging why my app wouldn’t compile, only to realize I’d misspelled a font name in a style sheet. Xcode’s error messages? Cryptic.

**Cache your Pods.** Add this to your `Podfile`:

```
installer.pods_project.build_configurations.each do |config|  
  config.build_settings["CCACHE_ENABLED"] = "YES"  
end
```

- This reduces rebuild times by reusing cached dependencies.
- Use `**.xcfilelist**` to only recompile files that changed.

**Lesson Learned:**  
Optimize your Xcode project early. A 10-minute setup saves hours over time.

# 3. Permissions on iOS Are Not Optional

Android lets you prototype without asking for permissions upfront. iOS? Not so much.

**The Problem:**  
I built a location-based app and tested it on Android first. Everything worked! Then I tried iOS… and the app crashed instantly. Why? Because I forgot to add `NSLocationWhenInUseUsageDescription` to my `Info.plist`.

**The Fix:**

- **Use** `**react-native-permissions**`**.** This library handles platform-specific permission flows:

```
import { check, PERMISSIONS, request } from 'react-native-permissions';  
  
const requestLocationPermission = async () => {  
  const status = await check(PERMISSIONS.IOS.LOCATION_WHEN_IN_USE);  
  if (status === 'denied') {  
    await request(PERMISSIONS.IOS.LOCATION_WHEN_IN_USE);  
  }  
};
```

- **Update** `**Info.plist**` **early.** Apple will reject your app if descriptions are missing.

**Lesson Learned:**  
iOS permissions are a minefield. Handle them _before_ writing a single line of feature code.

# 4. Navigation Will Bite You (Especially on iOS)

React Navigation’s `stack` and `tab` navigators work great—until you realize iOS users expect _specific_ behaviors.

**The Problem:**  
My app’s back button worked on Android, but iOS users kept getting stuck. Why? Because iOS doesn’t have a hardware back button, and my custom header didn’t align with Apple’s design guidelines.

**The Fix:**

- **Use** `**react-navigation-shared-element**` **for iOS transitions.** Smooth animations matter.
- Implement the `**headerBackTitleVisible: false**` option to avoid awkward “Back” text:

```
<Stack.Screen  
  name="Home"  
  component={HomeScreen}  
  options={{ headerBackTitleVisible: false }}  
/>
```

- Test **swipe gestures** rigorously. iOS users expect to swipe back from the left edge.

**Lesson Learned:**  
iOS navigation isn’t just about functionality — it’s about adhering to platform-specific UX patterns.

# 5. Lists Are Performance Killers (If You’re Not Careful)

`FlatList` is your friend… until it isn’t.

**The Problem:**  
I built a social media feed with hundreds of image-heavy posts. On Android, it was smooth. On iOS? Laggy scrolls and frozen frames.

**The Fix:**

- **Always use** `**initialNumToRender**` **and** `**windowSize**`**:**
```
<FlatList  
  data={posts}  
  initialNumToRender={5}  
  windowSize={10}  
  //   
/>
```
- This limits how many items render offscreen.
- **Avoid anonymous functions in** `**renderItem**`**:** They cause unnecessary re-renders.

**Lesson Learned:**  
iOS’s JavaScript thread is single-threaded. Optimize lists _aggressively_.

# 6. Native Modules Are Easier Than You Think

I avoided native iOS code like the plague — until I had to get the device’s battery level.

**The Problem:**  
No existing NPM package did what I needed. I had to write my own Objective-C module.

**The Fix:**

1. Create a `BatteryManager.m` file:

```
#import "React/RCTBridgeModule.h"  
  
@interface RCT_EXTERN_MODULE(BatteryManager, NSObject)  
RCT_EXTERN_METHOD(getBatteryLevel:(RCTPromiseResolveBlock)resolve reject:(RCTPromiseRejectBlock)reject)  
@end
```

1. Implement in Swift/Objective-C, then call it in JS:

```
import { NativeModules } from 'react-native';  
const { BatteryManager } = NativeModules;  
  
const level = await BatteryManager.getBatteryLevel();
```

**Lesson Learned:**  
Writing native modules feels intimidating but React Native’s bridge API is straightforward.

# Final Thoughts: Embrace the iOS Quirks

React Native for iOS isn’t “write once, run anywhere” — it’s _“write once, debug everywhere.”_ But once you learn its quirks (like mandatory permissions, navigation nuances, and list optimizations), you’ll ship apps faster than ever.