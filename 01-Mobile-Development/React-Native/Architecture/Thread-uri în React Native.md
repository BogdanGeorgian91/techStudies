React Native folosește **arhitectură multi-thread** pentru a separa diferite tipuri de operații și a menține UI-ul fluid.

---

## **Thread-urile Principale (3-4 Core Threads)**

### **1. UI Thread (Main Thread)**

```
┌─────────────────────────────────┐
│         UI/MAIN THREAD          │
│                                 │
│ • Renderare UI nativ           │
│ • Gestiune evenimente touch    │
│ • Animații native              │
│ • Update-uri ecran (60 FPS)    │
└─────────────────────────────────┘
```

**Responsabilități:**

- Toate operațiile UI native (iOS UIKit / Android View System)
- Procesare input utilizator (touch, gesturi)
- Desenare pe ecran
- **CRITICAL**: Trebuie să rămână liber pentru 60 FPS

**Exemplu blocare UI Thread:**

```javascript
// ❌ GREȘIT - Blochează UI Thread
<TouchableOpacity 
  onPress={() => {
    // Operație grea pe Main Thread
    for(let i = 0; i < 1000000; i++) {
      // UI îngheață aici
    }
  }}
>
```

---

### **2. JavaScript Thread**

```
┌─────────────────────────────────┐
│      JAVASCRIPT THREAD          │
│                                 │
│ • Execută cod JavaScript        │
│ • React reconciliation          │
│ • Business logic               │
│ • API calls                    │
└─────────────────────────────────┘
```

**Responsabilități:**

- Rulează întregul cod JavaScript/React
- Calculează ce trebuie randat (Virtual DOM)
- Gestionează state și props
- Execută hooks și lifecycle methods

**Exemplu:**

```javascript
// Toate acestea rulează pe JS Thread
const MyComponent = () => {
  const [data, setData] = useState([]); // JS Thread
  
  useEffect(() => {
    // Fetch rulează pe JS Thread
    fetchData().then(result => {
      setData(result); // State update pe JS Thread
    });
  }, []);
  
  // Render logic pe JS Thread
  return <View>{data.map(item => <Text>{item}</Text>)}</View>;
};
```

---

### **3. Shadow Thread (Layout Thread)**

```
┌─────────────────────────────────┐
│        SHADOW THREAD            │
│                                 │
│ • Calcule layout (Yoga)        │
│ • Shadow Tree management       │
│ • Flexbox calculations         │
│ • Măsurători componente        │
└─────────────────────────────────┘
```

**Responsabilități:**

- Calculează layout-ul folosind **Yoga** (engine Flexbox)
- Menține Shadow Tree (reprezentare virtuală a UI)
- Transformă stiluri React în layout nativ

**Exemplu calcule Shadow Thread:**

```javascript
// Shadow Thread calculează acest layout
<View style={{ 
  flex: 1, 
  flexDirection: 'row',
  justifyContent: 'space-between' 
}}>
  <View style={{ width: '50%' }} /> {/* Yoga calculează 50% din părinte */}
  <View style={{ flex: 1 }} />      {/* Yoga calculează spațiul rămas */}
</View>
```

---

### **4. Native Modules Thread**

```
┌─────────────────────────────────┐
│    NATIVE MODULES THREAD        │
│                                 │
│ • Module native custom          │
│ • Operații I/O                 │
│ • Database queries             │
│ • Heavy computations           │
└─────────────────────────────────┘
```

**Pentru operații grele, se creează thread-uri separate:**

```java
// Android - Thread separat pentru operații grele
@ReactMethod
public void heavyOperation(Promise promise) {
    new Thread(new Runnable() {
        @Override
        public void run() {
            // Operație grea pe thread separat
            String result = performHeavyCalculation();
            promise.resolve(result);
        }
    }).start();
}
```

---

## **Thread-uri Adiționale**

### **5. Network Thread**

```javascript
// Requests HTTP rulează pe thread separat
fetch('https://api.example.com/data')
  .then(response => response.json()) // Network thread
  .then(data => setData(data));      // Callback pe JS thread
```

### **6. AsyncStorage/Database Thread**

```javascript
// AsyncStorage operations pe thread separat
AsyncStorage.setItem('key', 'value'); // Background thread
SQLite.executeSql(query);             // Database thread
```

### **7. Animation Threads**

```javascript
// Animated API poate rula pe UI Thread
Animated.timing(animatedValue, {
  toValue: 100,
  duration: 1000,
  useNativeDriver: true // Rulează pe UI Thread, nu JS Thread!
}).start();

// Reanimated 2 - Worklets pe UI Thread
const animatedStyle = useAnimatedStyle(() => {
  // Acest cod rulează pe UI Thread!
  return {
    transform: [{ translateX: withSpring(offset.value) }]
  };
});
```

### **8. Hermes Background Thread (dacă e activat)**

```javascript
// Hermes JavaScript engine threads
// - Compilation thread
// - Garbage collection thread
// - Concurrent marking thread
```

---

## **Comunicarea între Thread-uri**

### **Vechea Arhitectură - Bridge**

```
JS Thread ←→ Bridge (JSON) ←→ Native Thread
         Serializare/Deserializare
         ASINCRON & SLOW
```

### **Noua Arhitectură - JSI**

```
JS Thread ←→ JSI (C++) ←→ Native Thread
         Direct Memory Access
         SINCRON & FAST
```

---

## **Vizualizare Practică**

```javascript
// Exemplu complet - ce se întâmplă pe fiecare thread

function ChatScreen() {
  const [messages, setMessages] = useState([]); // JS Thread
  
  // 1. User apasă buton - UI Thread
  const handleSend = () => {
    // 2. Handler rulează - JS Thread
    const newMessage = { text: input, timestamp: Date.now() };
    
    // 3. Update state - JS Thread
    setMessages([...messages, newMessage]);
    
    // 4. Re-render - JS Thread calculează VDOM
    // 5. Layout - Shadow Thread calculează poziții
    // 6. Update UI - UI Thread desenează pe ecran
    
    // 7. Save to database - Native Module Thread
    AsyncStorage.setItem('messages', JSON.stringify(messages));
    
    // 8. Network request - Network Thread
    fetch('/api/send', { method: 'POST', body: newMessage });
    
    // 9. Animation - UI Thread (cu useNativeDriver)
    Animated.spring(sendButtonScale, {
      toValue: 1.2,
      useNativeDriver: true
    }).start();
  };
  
  return (
    <View>
      <FlatList data={messages} /> {/* UI Thread pentru scroll */}
      <TextInput onChangeText={setText} /> {/* UI Thread pentru input */}
      <TouchableOpacity onPress={handleSend}> {/* UI Thread pentru touch */}
        <Text>Send</Text>
      </TouchableOpacity>
    </View>
  );
}
```

---

## **Monitoring Thread-uri**

### **iOS - Xcode**

```bash
# În Xcode Debugger poți vedea toate thread-urile:
1. com.facebook.react.JavaScript (JS Thread)
2. main (UI Thread)  
3. com.facebook.react.ShadowQueue (Shadow Thread)
4. NSOperationQueue (Network)
5. com.facebook.react.AsyncLocalStorageQueue
```

### **Android - Android Studio**

```bash
# În Android Studio Profiler:
1. main (UI Thread)
2. mqt_js (JS Thread)
3. mqt_native_modules
4. AsyncTask #1-5 (Thread pool)
5. OkHttp (Network threads)
```

### **React Native Debugger**

```javascript
// Vezi performanța per thread
import { PerformanceObserver } from 'perf_hooks';

const obs = new PerformanceObserver((items) => {
  items.getEntries().forEach((entry) => {
    console.log(`${entry.name}: ${entry.duration}ms`);
  });
});
obs.observe({ entryTypes: ['measure'] });
```

---

## **Optimizări Thread-uri**

### **1. Folosește Native Driver pentru Animații**

```javascript
// ✅ BUN - Rulează pe UI Thread
Animated.timing(animation, {
  toValue: 100,
  useNativeDriver: true // Nu blochează JS Thread!
}).start();

// ❌ RĂU - Rulează pe JS Thread
Animated.timing(animation, {
  toValue: 100,
  useNativeDriver: false // Blochează JS Thread
}).start();
```

### **2. InteractionManager pentru Operații Grele**

```javascript
// Amână operații grele după ce animațiile s-au terminat
InteractionManager.runAfterInteractions(() => {
  // Operație grea care nu va afecta animațiile
  processLargeDataSet();
});
```

### **3. Worklets pentru UI Thread Operations**

```javascript
// Reanimated 2 - Cod care rulează direct pe UI Thread
const gesture = Gesture.Pan()
  .onUpdate((e) => {
    'worklet'; // Rulează pe UI Thread!
    translateX.value = e.translationX;
  });
```

---

## **Răspuns Sumar**

**React Native folosește minim 3-4 thread-uri principale:**

1. **UI/Main Thread** - Rendering și touch events
2. **JavaScript Thread** - Cod React/JavaScript
3. **Shadow Thread** - Layout calculations
4. **Native Modules Thread** - Operații native custom

**Plus thread-uri adiționale pentru:**

- Network requests
- AsyncStorage/Database
- Animations (când folosesc native driver)
- Background tasks
- Garbage collection (Hermes)

În total, o aplicație React Native tipică poate avea **10-20+ thread-uri** active, în funcție de complexitate și funcționalități utilizate.