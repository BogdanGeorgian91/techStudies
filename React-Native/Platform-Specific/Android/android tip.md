This always helps me. To set it up, connect your Android to your phone. Use "adb devices" to get the device ID of the Android. Then use "adb logcat -v threadtime DEVICE_ID" to view the logs

===

Yea stay away from expo go . Try this
npx react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res --reset-cache true && cd android/ && chmod +x ./gradlew && ./gradlew clean && ./gradlew assembleDebug && cd ..

And the. Should see a .apk in Android folder somewhere.. use that .apk instead

And