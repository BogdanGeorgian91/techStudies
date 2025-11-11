Here’s your text converted into **Markdown with annotations** for clarity:

````
# Inspect Logs to See What Happened

If Core Data with CloudKit doesn’t appear to be syncing:  

- ✅ Confirm that you’re testing on **two unlocked devices**.  
- ✅ Ensure both devices are **logged into the same iCloud account**.  
- ✅ Check that you have a **good wireless internet connection**.  

⚠️ **Note:** Push notifications may get dropped or deferred, so don’t rely on them for testing.  

Instead, **watch system logs** to observe the status and result of expected activity.  

---

## Log Commands

### 1. Filter CloudKit logs  
See operations on the `cloudd` process for your container:

```bash
log stream --info --debug --predicate 'process = "cloudd" and message contains[cd] "containerID=com.mycontainer"'
````

---

### **2. Filter Core Data logs**

See mirroring delegate’s setup, exports, and imports for your process:

```
log stream --info --debug --predicate 'process = "myprocess" and (subsystem = "com.apple.coredata" or subsystem = "com.apple.cloudkit")'
```

---
### **3. Monitor both CloudKit and Core Data logs**

At the same time:

```
log stream --info --debug --predicate '(process = "myprocess" and (subsystem = "com.apple.coredata" or subsystem = "com.apple.cloudkit")) or (process = "cloudd" and message contains[cd] "container=com.mycontainer")'
```

---

## **📖 More Info**

For more information about logging, see _Viewing Log Messages_.
