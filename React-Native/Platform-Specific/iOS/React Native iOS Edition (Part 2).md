# 1. Xcode Provisioning Profiles: The Silent Killer

I used to think Xcode was my friend. Then I met **provisioning profiles**.

**The Problem:**  
After weeks of coding, I tried archiving my app for TestFlight. Xcode threw 12 errors, all cryptic: _“No matching provisioning profile found”_, _“Code signing failed”_. Turns out, my Apple Developer account had expired, my certificates were invalid, and my profiles were tangled like last year’s Christmas lights.

**The Fix:**

- **Automate with Fastlane.** Set up `match` to sync certificates and profiles across your team:

```
lane :sync_certs do  
  match(type: "development", readonly: true)  
end
```

- **Always check “Automatically manage signing”** in Xcode _before_ adding capabilities like Push Notifications.

**Lesson Learned:**  
Provisioning profiles are the IRS of iOS development — mess them up, and you’ll pay. Automate early.

# 2. The SafeAreaView Isn’t Just for Notches

iPhones have notches, dynamic islands, and rounded corners. Your app _will_look broken if you ignore them.

**The Problem:**  
I designed a sleek header for my app. On my iPhone 13, it looked perfect. Then a user sent a screenshot from an iPhone 14 Pro: the header was _under_the dynamic island, hiding critical buttons.

**The Fix:**

- **Wrap EVERYTHING in** `**SafeAreaView**`**:**

```
import { SafeAreaView } from 'react-native';  
  
const App = () => (  
  <SafeAreaView style={{ flex: 1 }}>  
    {/* Your content */}  
  </SafeAreaView>  
);
```

- Test on **multiple device sizes** using Xcode’s Preview Simulator.

**Lesson Learned:**  
Assume every iPhone has a notch. Because soon, they all will.

# 3. Universal Links: The iOS-Only Headache

Deep linking worked on Android. iOS? _“Where’s my data?!”_

**The Problem:**  
My app needed to handle `https://myapp.com/share/123` links. Android opened the app instantly. iOS opened Safari. Why? Because I forgot the `**apple-app-site-association**` file.

**The Fix:**

1. Host a JSON file at `https://myapp.com/.well-known/apple-app-site-association` (no extensions!):

```
{  
  "applinks": {  
    "apps": [],  
    "details": [{ "appID": "TEAMID.com.myapp", "paths": ["/share/*"] }]  
  }  
}
```

1. Add the **Associated Domains** capability in Xcode: `applinks:myapp.com`.

**Lesson Learned:**  
iOS universal links are like a secret handshake. Miss one step, and you’re locked out.

# 4. Swift Modules Are Worth the Learning Curve

I avoided Swift because Objective-C looked familiar. Big mistake.

**The Problem:**  
I needed a native module to handle ARKit. All tutorials used Swift. My Objective-C code worked… until Xcode updated and broke my bridging header.

**The Fix:**

1. Create a `MyARKitModule.swift` file:

```
@objc(MyARKitModule)  
class MyARKitModule: NSObject {  
  @objc static func requiresMainQueueSetup() -> Bool { return true }  
    
  @objc func startSession(_ resolve: RCTPromiseResolveBlock) {  
    // ARKit logic here  
  }  
}
```

1. Add it to your `Podfile`’s target.

**Lesson Learned:**  
Swift is the future. Embrace it, or spend hours fixing deprecated Objective-C code.

# 5. Memory Leaks Will Crash Your App (Silently)

React Native’s garbage collection isn’t perfect. iOS devices have _zero_ tolerance for memory hogs.

**The Problem:**  
Users reported my app crashing after 10 minutes of use. Xcode’s **Memory Debugger** showed a retain cycle in a native module — I’d forgotten to weakify `self` in a block.

**The Fix:**

- **Use Instruments’ Allocations tool** to track leaks.
- In Objective-C, **always weakify strong references:**

```
__weak typeof(self) weakSelf = self;  
[self doSomething:^{  
  __strong typeof(weakSelf) strongSelf = weakSelf;  
  [strongSelf handleResult];  
}];
```

**Lesson Learned:**  
iOS doesn’t forgive memory mistakes. Profile early, profile often.

# 6. The App Store Rejects Apps for Dumb Reasons

Your app works. Your tests pass. Apple says **“nope.”**

**The Problem:**  
Apple rejected my app twice:

1. **Missing IPv6 support** (my backend wasn’t configured).
2. **“App previews”** used iPhone 15 footage, but I didn’t have a 15.

**The Fix:**

- Test on **iOS’s “Wi-Fi Assist”** (it uses cellular data when Wi-Fi is weak).
- Use Apple’s **Transporter app** to pre-check metadata.

**Lesson Learned:**  
The App Store is a capricious deity. Sacrifice a goat (or read the guidelines twice).

# Final Thoughts: iOS Development is a Marathon

React Native for iOS is full of “gotchas” that’ll make you question your career choices. But once you master Xcode quirks, Swift modules, and Apple’s ever-changing rules, you’ll ship apps that feel _truly_ native.