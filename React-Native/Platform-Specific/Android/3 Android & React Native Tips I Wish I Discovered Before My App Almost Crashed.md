# Android Tip #1: StrictMode is Your Overprotective Parent (And You’ll Thank Them Later)

Remember that time you accidentally loaded a 4K image on the UI thread and wondered why your app felt like a slideshow? Enter `StrictMode`.

Most tutorials treat it like a “nice-to-have,” but enabling it early is like having a tiny angel scream “DON’T TOUCH THAT” before you ship a performance disaster.

```
// In your Application class onCreate():  
StrictMode.setThreadPolicy(  
StrictMode.ThreadPolicy.Builder()  
.detectDiskReads()  
.detectDiskWrites()  
.detectNetwork()  
.penaltyLog()  
.build()  
)
```

**Why it’s gold:** It catches crimes against UX _before_ they happen — like main-thread database calls or leaked files. I once shipped an animation that tanked FPS. StrictMode flagged it during testing. My users? They never knew.

# React Native Tip #1: Hermes Isn’t Just a Greek God — It’s Your Memory Savior

When I first heard “Hermes makes React Native faster,” I shrugged. Then I tried it.

**The magic:** Hermes isn’t just about speed — it slashes your app’s memory usage by up to **40%**. Forgot to enable it? Same. Here’s how to fix that:

1. In `android/app/build.gradle`:

```
project.ext.react = [    
    enableHermes: true  // <- Change this to true    
]
```

1. In `ios/Podfile`:

```
use_react_native!(    
   :hermes_enabled => true    
)
```

**Why it’s gold:** My mid-tier test device went from gasping for RAM to running smoother than a TikTok dance tutorial. Plus, Hermes plays nicer with ProGuard, shrinking APK size.

# Android Tip #2: ADB’s Secret Network Throttling Superpower

“But it works on Wi-Fi!” _— famous last words before a 1-star review._

Instead of praying your users have perfect signal, simulate their pain with ADB:

```
adb shell settings put global captive_portal_mode 2  # Simulate no network    
adb shell settings put global captive_portal_mode 1  # Slow AF network    
adb shell settings put global captive_portal_mode 0  # Back to normal
```

**Why it’s gold:** Caught a race condition where my app assumed network calls would _always_ succeed. Spoiler: they don’t.

# The Bonus Tip: “Pressable” > “Touchable” (Fight Me)

`TouchableOpacity` was my ride-or-die—until I realized `Pressable` lets me:

- Add ripple effects _without_ wrapping in a View.
- Track press duration (hello, UX micro-interactions!).
- Style pressed states without hacky conditionals.

```
<Pressable    
  onPress={() => {}}    
  android_ripple={{ color: '#your_hex_here' }}    
  style={({ pressed }) => ({    
    opacity: pressed ? 0.5 : 1,    
  })}    
/>
```

**Why it’s gold:** My UI code got 30% cleaner, and my designer stopped sending me “gentle reminders” about Android ripple consistency.

# Wait, One More Thing…

If you take nothing else from this post: **use** `**adb logcat**` **over Android Studio’s logcat**. Filtering logs by tag/priority is faster than waiting for the IDE to index your life.

```
adb logcat -s "MyAppTag:*" *:E  # Show only your app’s logs + errors
```