# 1. “Why Is My App Randomly Crashing in Production?!” (Hint: It’s Not Your Code)

You tested it. QA approved it. Then App Store reviews flood in: _“Crashes on launch!!!”_ Sound familiar?

**The Silent Killer**: iOS **Memory Pressure Crashes**.  
React Native’s bridge can spike memory usage when passing large data chunks (like base64 images). iOS kills your app without a stack trace, leaving you clueless.

**The Fix**:

- Use `react-native-fast-image` for off-main-thread image decoding.
- **Nuclear Option**: Monitor memory pressure with native code:

```
// Add to AppDelegate.swift    
NotificationCenter.default.addObserver(    
    forName: UIApplication.didReceiveMemoryWarningNotification,    
    object: nil,    
    queue: .main    
) { _ in    
    RCTLogWarn("MEMORY WARNING: Free up resources NOW!")    
}
```

_Pro Tip_: Test with Xcode’s “Simulate Memory Warning” (Debug → Simulate Memory Warning).

# 2. The App Store Rejection You Never Saw Coming

Your app passed TestFlight. Apple’s review team says: _“Violates Guideline 5.1.1 (Data Collection).”_ But you’re not even using ads!

**The Culprit**: **Third-Party Libraries with Covert Trackers**.  
Libraries like `react-native-firebase` or analytics tools often bundle iOS frameworks that declare tracking permissions _automatically_. Apple’s automated scans detect this.

**The Escape Hatch**:

- Audit your Podfile: Run `npx react-native-community/cli pod-report` to list all native dependencies.
- Add _required reason APIs_ to `Info.plist` for ANY tracking (even accidental):

```
<key>NSPrivacyTracking</key>    
<true/>    
<key>NSPrivacyTrackingDomains</key>    
<array/>
```

_Warning_: Miss this and Apple will reject your app on resubmission.

# 3. “Debugging Performance? LOL.” Master the Flamegraph

Your app feels sluggish, but the React Native Profiler shows nothing. Time to go deeper.

**The Real Issue**: **Native Thread Contention**.  
JavaScript isn’t the bottleneck — your UI thread is blocked by heavy synchronous native modules (e.g., file I/O).

**Diagnose Like a Pro**:
1. Open Xcode → Instruments → “System Trace”.
2. Filter to your app’s process.
3. Look for `Main Thread` blocks longer than 16ms (1 frame drop).  
    _Gotcha_: `RCTUIManager` operations should be sub-millisecond. If not, audit your native components.

# 4. The OTA Update Nightmare (When CodePush Backfires)

You pushed a critical fix via CodePush. But 30% of users are stuck on the old version.

**Why?** iOS **Aggressive Memory Cleanup**.  
Backgrounded apps >3 days get their JavaScript bundle purged. On relaunch, they redownload the update… _if the network is perfect_.

**Nuclear Proofing**:

- Add a native “version fallback” check:
```
// On app launch    
const {nativeBuildVersion} = require('react-native').NativeModules.Constants;    
if (nativeBuildVersion !== lastStoredVersion) {    
  CodePush.restartApp(); // Force consistency    
}
```

# 5. “Why Does My App Drain 40% Battery?!” The Background Zombie

Users complain about battery drain. You’re not even using location!

**The Hidden Vampire**: **Unoptimized Native Modules**.  
Libraries like `react-native-geolocation` or `audio` can keep the iOS location/audio background flag active _even when unused_.

**Slay the Zombie**:
1. Open your project’s `.entitlements` file.
2. Remove “Location updates” and “Audio background modes” unless _absolutely_ required.
3. Test with Xcode’s “Energy Log” tool (Debug → Logging).

**Conclusion: Become the iOS Whisperer**  
React Native iOS isn’t “write once, run anywhere” — it’s “debug everywhere, but _smartly_.” These pitfalls aren’t in docs or tutorials. They’re earned through late-night crashes and App Store rejections.