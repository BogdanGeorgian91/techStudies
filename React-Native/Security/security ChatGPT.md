Below is a security-minded checklist that maps out **the main ways React Native apps get hacked** and the **defences you can (and should) layer on**.

It’s organised by attack surface so you can drop straight into the areas you’re responsible for (client, network, backend, CI/CD, etc.).

|**#**|**Attack category**|**Typical exploits on React Native**|**What it really looks like in the wild**|**Mitigations / hardening steps**|
|---|---|---|---|---|
|**1**|**Reverse engineering & static inspection**|• Decompile the APK → pull the index.android.bundle / Hermes byte-code and harvest API keys, feature flags, or proprietary algorithms.• Dump iOS IPA and run rn-decompile to pretty-print JS.• Extract secret endpoints from android/app/src/**/Strings.xml or exported constants in native modules.|Attackers script this as part of credential-stuffing campaigns or to clone your paywall logic.|🛡 Enable Proguard/R8 + Hermes byte-code in release mode.🛡 Don’t hard-code secrets—inject at run-time from a secured backend or remote config.🛡 Obfuscate JS with metro-minify (esbuild/terser) and symbol-strip native code.|
|**2**|**Dynamic hooking & runtime tampering**|• Frida / Objection inject JS or hook Objective-C/Java methods to bypass paywalls, disable SSL pinning, fake in-app purchases.• Patch the JS bundle on-device (e.g. via Lucky Patcher) to change logic branches.|Very common in “mod” APK repos and grey-market stores.|🛡 Implement jailbreak/root detection and device integrity checks (SafetyNet/Play Integrity API, DeviceCheck, App Attest).🛡 Add RASP libraries (e.g. Guardsquare iXGuard, DataDome, or Appdome) to detect hooking frameworks.🛡 Validate business-critical actions server-side (never trust client decisions).|
|**3**|**Dev / debug features left on**|• Production build accidentally ships with the React Native dev menu enabled (__DEV__ leakage).• Metro bundler port open on the LAN → attacker pushes a malicious bundle over Wi-Fi.|Rare but catastrophic: “coffee shop” attackers demoing RCE.|🛡 release builds **MUST** set devDisabled=true (RCT_DEV = 0, RCT_ENABLE_INSPECTOR=0).🛡 Block all debug ports with an App Firewall (Android) and ATS on iOS.|
|**4**|**Insecure local storage**|• Tokens saved in AsyncStorage or Realm DB can be copied from a rooted device.• SQLite databases stored in plain text contain PII or GDPR data.|Stolen refresh-tokens used to pull additional user data.|🛡 Store sensitive items in Keychain/Keystore (via react-native-keychain, react-native-secure-storage, MMKV secure).🛡 Encrypt SQLite/Realm (SQLCipher, OnDeviceEncryption).🛡 Clear all caches on logout/background if the app processes highly-sensitive info.|
|**5**|**Network-layer attacks**|• Man-in-the-middle on public Wi-Fi: captures REST/GraphQL traffic if you fell back to HTTP or accept invalid certs.• TLS-pinning bypassed with a patched OkHttpClient or NSURLSession swizzling.|Automated credential harvesting or session hijack.|🛡 Enforce HTTPS everywhere; set ATS with NSAllowsArbitraryLoads = false.🛡 Use certificate pinning (react-native-ssl-pinning, OkHttp CertificatePinner).🛡 Implement short-lived JWTs with refresh tokens and detect reuse.|
|**6**|**Deep-link & Intent abuse**|• Craft a malicious URI (myapp://pay?amount=99999) and trick the OS into launching the app, skipping auth screens.• Android exported=true Activities allow arbitrary Intent extras.|Phishing links in SMS/email redirecting users into privileged flows.|🛡 Validate ALL parameters arriving through deep links/intents; require user re-auth for sensitive actions.🛡 Add android:exported="false" unless absolutely required.🛡 Prefix custom schemes with your brand (reduce risk of scheme collisions).|
|**7**|**WebView & JavaScript-bridge exploits**|• Injected JavaScript via window.postMessage XSS in a hybrid screen.• Exposed addJavascriptInterface on Android leaks native APIs to the page.|Ad SDKs running arbitrary JS inside your WebView.|🛡 Use safe originWhitelist, disable injectedJavaScript where possible.🛡 On Android, only expose minimal interfaces and annotate with @JavascriptInterface selectively.🛡 Apply CSP headers on first-party web content.|
|**8**|**Over-the-air (OTA) update hijacking**|• If you use CodePush/EAS Updates and the update server/API keys are compromised, an attacker can push a trojan bundle.|Users seamlessly install a back-doored JS build.|🛡 Sign every bundle and validate signatures in native code before load.🛡 Protect update keys (App Center, Expo) in secret managers; rotate on suspicion.🛡 Allow OTA only for non-critical code paths; force full-store updates for security patches.|
|**9**|**Supply-chain & dependency attacks**|• Malicious NPM package (event-stream-style) or compromised GitHub action injects credential stealer during CI.• Compromised CocoaPod inserts malicious static lib.|CI builds shipping back-doors unnoticed.|🛡 Lock dependency hashes with npm audit signatures / pnpm lockfile, Renovate + SCA scanners (Snyk, Dependabot).🛡 Run CI in isolated runners; sign artifacts after a reproducible build.🛡 Use private NPM registries and Podspec mirrors.|
|**10**|**Backend/API weaknesses surfaced by mobile**|• GraphQL introspection left enabled → attacker enumerates types and sensitive mutations.• Server trusts client-side “isAdmin” flag or price calculations.|Classic OWASP API Top 10 fallout.|🛡 Assume the mobile client is hostile—re-validate all params, permissions, prices server-side.🛡 Rate-limit auth & mutation endpoints, monitor for anomalous mobile fingerprints.|
|**11**|**Social-engineering & UX abuse**|• Fake “system update” modal in the app asks the user for credentials.• Abusing the permission request flow to trick acceptance.|User hands over MFA codes to attacker-controlled screen.|🛡 Follow platform HIG for permission dialogs; never roll your own credential UI.🛡 Employ anti-phishing copy/brand guardrails and certificate transparency.|

---

### **Defence-in-Depth Quick Wins**

1. **Ship proper release builds** (Hermes, Proguard, no debug flags) **on every CI run**—never sign a “local quick build”.
    
2. **Pin your TLS certificates** and monitor cert transparency logs for look-alikes.
    
3. **Keep secrets off the device**—secure storage + backend exchange only when necessary.
    
4. **Instrument your code**: add analytics events around jail-break detection, SSL-pinning failures, unexpected OTA payloads, etc.
    
5. **Automate mobile security linting**: run MobSF / OWASP MAST in CI for each build; fail on high-risk issues.
    
6. **Threat-model each new feature** with STRIDE or OWASP Mobile Top 10 in mind before writing any code.
    
7. **Plan a rollback path**: if an OTA or store build is compromised, be able to remotely disable critical flows or invalidate tokens globally.
    

---

#### **How to use this list**

- **Interview prep:** Be ready to walk through one or two attack scenarios end-to-end, then propose layered mitigations.
    
- **On the job:** Treat it as a living checklist—every user story that touches a row here needs a sign-off that risks are mitigated.
    
- **Security reviews:** Map pentest findings to these categories to spot the patterns you still need to address.