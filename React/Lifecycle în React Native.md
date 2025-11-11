### **Ce este Lifecycle-ul?**

**Lifecycle** (ciclul de viață) reprezintă seria de etape prin care trece o componentă React Native de la crearea ei până la distrugere:

- **Montare** (Mounting) - componenta este creată și inserată în UI
- **Actualizare** (Updating) - componenta se re-renderează când se schimbă props/state
- **Demontare** (Unmounting) - componenta este înlăturată din UI

---

## **Class Components - Metode Lifecycle**

### **1. MONTARE (Mounting)**

```javascript
class MyComponent extends React.Component {
  // 1. Constructor - prima metodă apelată
  constructor(props) {
    super(props);
    this.state = { 
      data: null,
      loading: true 
    };
    console.log('1. Constructor');
  }

  // 2. Se apelează ÎNAINTE de render
  static getDerivedStateFromProps(props, state) {
    console.log('2. getDerivedStateFromProps');
    // Rar folosită - actualizează state bazat pe props
    return null; // sau return { newState: value }
  }

  // 3. Renderează componenta
  render() {
    console.log('3. Render');
    return (
      <View>
        <Text>{this.state.data}</Text>
      </View>
    );
  }

  // 4. Se apelează DUPĂ ce componenta a fost montată în DOM
  componentDidMount() {
    console.log('4. ComponentDidMount');
    // Locul ideal pentru:
    // - Fetch date de la API
    // - Subscriptions
    // - Timers
    
    fetch('https://api.example.com/data')
      .then(res => res.json())
      .then(data => {
        this.setState({ data, loading: false });
      });
  }
}
```

### **2. ACTUALIZARE (Updating)**

```javascript
class MyComponent extends React.Component {
  // Se apelează când props sau state se schimbă
  
  // 1. Decide dacă componenta ar trebui re-renderată
  shouldComponentUpdate(nextProps, nextState) {
    // Optimizare performanță - previne re-render inutile
    if (this.state.count === nextState.count) {
      return false; // Nu re-renderiza
    }
    return true; // Re-renderizează
  }

  // 2. Se apelează înainte de render (rar folosită)
  getSnapshotBeforeUpdate(prevProps, prevState) {
    // Capturează informații înainte de actualizarea DOM
    // ex: poziția scroll-ului
    return null; // sau o valoare care merge la componentDidUpdate
  }

  // 3. Se apelează DUPĂ ce componenta s-a actualizat
  componentDidUpdate(prevProps, prevState, snapshot) {
    // Operații după actualizare:
    // - Fetch date noi dacă props s-au schimbat
    // - Operații DOM
    
    if (prevProps.userId !== this.props.userId) {
      this.fetchUserData(this.props.userId);
    }
  }
}
```

### **3. DEMONTARE (Unmounting)**

```javascript
class MyComponent extends React.Component {
  componentDidMount() {
    // Setăm listeners/timers
    this.timer = setInterval(() => {
      this.setState({ time: Date.now() });
    }, 1000);
    
    this.subscription = EventEmitter.subscribe('event', this.handleEvent);
  }

  // Se apelează ÎNAINTE ca componenta să fie distrusă
  componentWillUnmount() {
    console.log('ComponentWillUnmount');
    // CRUCIAL pentru cleanup:
    // - Anulează timers
    // - Anulează subscriptions
    // - Anulează requests
    
    clearInterval(this.timer);
    this.subscription.remove();
  }
}
```

---

## **Functional Components - React Hooks**

### **Hook-uri echivalente pentru Lifecycle**

```javascript
import React, { useState, useEffect, useLayoutEffect } from 'react';

function MyComponent(props) {
  // Constructor → useState pentru inițializare state
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  // componentDidMount + componentDidUpdate + componentWillUnmount → useEffect
  useEffect(() => {
    console.log('Component mounted sau updated');
    
    // Cod pentru componentDidMount și componentDidUpdate
    fetchData();
    
    // Cleanup function = componentWillUnmount
    return () => {
      console.log('Component will unmount');
      // Cleanup code aici
    };
  }, []); // [] = rulează doar la mount (ca componentDidMount)

  // useEffect cu dependențe
  useEffect(() => {
    // Rulează când userId se schimbă (ca componentDidUpdate)
    if (props.userId) {
      fetchUserData(props.userId);
    }
  }, [props.userId]); // Re-rulează când userId se schimbă

  const fetchData = async () => {
    try {
      const response = await fetch('https://api.example.com/data');
      const result = await response.json();
      setData(result);
      setLoading(false);
    } catch (error) {
      console.error(error);
    }
  };

  // Render
  return (
    <View>
      {loading ? <Text>Loading...</Text> : <Text>{data}</Text>}
    </View>
  );
}
```

### **useEffect - Scenarii comune**

```javascript
function ExampleComponent() {
  const [count, setCount] = useState(0);

  // 1. Rulează după FIECARE render
  useEffect(() => {
    console.log('Rulează după fiecare render');
  });

  // 2. Rulează doar O DATĂ (componentDidMount)
  useEffect(() => {
    console.log('Rulează doar la mount');
    
    return () => {
      console.log('Cleanup la unmount');
    };
  }, []);

  // 3. Rulează când dependențele se schimbă
  useEffect(() => {
    console.log('Count s-a schimbat:', count);
  }, [count]);

  // 4. Cleanup pentru subscriptions
  useEffect(() => {
    const timer = setInterval(() => {
      console.log('Timer tick');
    }, 1000);

    // IMPORTANT: Cleanup function
    return () => {
      clearInterval(timer);
    };
  }, []);

  // 5. Event listeners
  useEffect(() => {
    const handleScroll = () => console.log('Scrolling');
    
    window.addEventListener('scroll', handleScroll);
    
    return () => {
      window.removeEventListener('scroll', handleScroll);
    };
  }, []);
}
```

### **useLayoutEffect - Sincron cu DOM**

```javascript
function MeasureComponent() {
  const [dimensions, setDimensions] = useState({});
  const ref = useRef();

  // useLayoutEffect rulează SINCRON după DOM update
  // Folosit pentru măsurători DOM
  useLayoutEffect(() => {
    if (ref.current) {
      setDimensions({
        width: ref.current.offsetWidth,
        height: ref.current.offsetHeight
      });
    }
  }, []);

  return <View ref={ref}>...</View>;
}
```

---

## **Exemple Practice Complete**

### **1. Fetch Date cu Loading și Error Handling**

```javascript
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let cancelled = false;

    const fetchUser = async () => {
      try {
        setLoading(true);
        const response = await fetch(`/api/users/${userId}`);
        const data = await response.json();
        
        // Verifică dacă componenta nu a fost demontată
        if (!cancelled) {
          setUser(data);
          setError(null);
        }
      } catch (err) {
        if (!cancelled) {
          setError(err.message);
        }
      } finally {
        if (!cancelled) {
          setLoading(false);
        }
      }
    };

    fetchUser();

    // Cleanup - anulează request-ul dacă componenta se demontează
    return () => {
      cancelled = true;
    };
  }, [userId]);

  if (loading) return <ActivityIndicator />;
  if (error) return <Text>Error: {error}</Text>;
  if (!user) return <Text>No user found</Text>;

  return (
    <View>
      <Text>{user.name}</Text>
      <Text>{user.email}</Text>
    </View>
  );
}
```

### **2. Timer cu Cleanup**

```javascript
function CountdownTimer({ initialSeconds }) {
  const [seconds, setSeconds] = useState(initialSeconds);

  useEffect(() => {
    if (seconds <= 0) return;

    const timer = setInterval(() => {
      setSeconds(prev => {
        if (prev <= 1) {
          clearInterval(timer);
          return 0;
        }
        return prev - 1;
      });
    }, 1000);

    return () => clearInterval(timer);
  }, [seconds]);

  return (
    <View>
      <Text style={styles.timer}>
        {Math.floor(seconds / 60)}:{(seconds % 60).toString().padStart(2, '0')}
      </Text>
    </View>
  );
}
```

### **3. Keyboard Listener în React Native**

```javascript
function KeyboardAwareComponent() {
  const [keyboardHeight, setKeyboardHeight] = useState(0);

  useEffect(() => {
    const showSubscription = Keyboard.addListener('keyboardDidShow', (e) => {
      setKeyboardHeight(e.endCoordinates.height);
    });
    
    const hideSubscription = Keyboard.addListener('keyboardDidHide', () => {
      setKeyboardHeight(0);
    });

    // Cleanup
    return () => {
      showSubscription.remove();
      hideSubscription.remove();
    };
  }, []);

  return (
    <View style={{ paddingBottom: keyboardHeight }}>
      <TextInput placeholder="Type here..." />
    </View>
  );
}
```

---

## **Când să folosești ce?**

### **Class Components:**

- Proiecte legacy
- Când ai nevoie de Error Boundaries
- Cod existent care nu poate fi refactorizat

### **Functional Components + Hooks:**

- **Recomandarea actuală** pentru cod nou
- Mai simplu și mai ușor de citit
- Mai ușor de testat
- Mai puțin cod boilerplate
- Reutilizare mai bună a logicii (custom hooks)

### **Reguli importante pentru useEffect:**

1. **Declară toate dependențele** în array-ul de dependențe
2. **Cleanup întotdeauna** pentru timers, listeners, subscriptions
3. **Evită memory leaks** - verifică dacă componenta e montată înainte de setState
4. **Nu uita array-ul gol `[]`** pentru comportament de componentDidMount
5. **Folosește multiple useEffect** pentru logică separată

Lifecycle-ul este esențial pentru gestionarea corectă a resurselor, performanței și pentru evitarea memory leaks în aplicațiile React Native.