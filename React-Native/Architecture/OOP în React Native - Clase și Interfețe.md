### **Concepte Fundamentale OOP**

**OOP (Object-Oriented Programming)** se bazează pe 4 piloni principali:

1. **Încapsulare** - Gruparea datelor și metodelor într-o unitate
2. **Moștenire** - Reutilizarea codului prin extindere
3. **Polimorfism** - Același interface, comportamente diferite
4. **Abstractizare** - Ascunderea detaliilor de implementare

---

## **Clase în React Native**

### **1. Class Components (Legacy)**

```javascript
// Clasă React Component
class UserProfile extends React.Component {
  // Constructor - Încapsulare
  constructor(props) {
    super(props);
    // State privat al clasei
    this.state = {
      user: null,
      loading: true,
      error: null
    };
    
    // Binding pentru metode
    this.fetchUser = this.fetchUser.bind(this);
  }

  // Lifecycle Methods - Moștenite din React.Component
  componentDidMount() {
    this.fetchUser();
  }

  componentDidUpdate(prevProps) {
    if (prevProps.userId !== this.props.userId) {
      this.fetchUser();
    }
  }

  // Metode private
  async fetchUser() {
    try {
      this.setState({ loading: true });
      const response = await fetch(`/api/users/${this.props.userId}`);
      const user = await response.json();
      this.setState({ user, loading: false });
    } catch (error) {
      this.setState({ error: error.message, loading: false });
    }
  }

  // Metodă publică
  render() {
    const { user, loading, error } = this.state;
    
    if (loading) return <ActivityIndicator />;
    if (error) return <Text>Error: {error}</Text>;
    
    return (
      <View>
        <Text>{user?.name}</Text>
        <Button title="Refresh" onPress={this.fetchUser} />
      </View>
    );
  }
}
```

---

### **2. Clase TypeScript cu Interfețe**

```typescript
// INTERFEȚE - Contracte pentru structura obiectelor
interface User {
  id: string;
  name: string;
  email: string;
  avatar?: string; // Proprietate opțională
}

interface UserProfileProps {
  userId: string;
  onUserLoad?: (user: User) => void;
}

interface UserProfileState {
  user: User | null;
  loading: boolean;
  error: string | null;
}

// CLASĂ cu TypeScript
class UserProfile extends Component<UserProfileProps, UserProfileState> {
  // Type safety pentru state
  state: UserProfileState = {
    user: null,
    loading: true,
    error: null
  };

  async componentDidMount(): Promise<void> {
    await this.loadUser();
  }

  private async loadUser(): Promise<void> {
    try {
      const user = await this.fetchUserData(this.props.userId);
      this.setState({ user, loading: false });
      
      // Type safety pentru callback
      this.props.onUserLoad?.(user);
    } catch (error) {
      this.setState({ 
        error: error instanceof Error ? error.message : 'Unknown error',
        loading: false 
      });
    }
  }

  private async fetchUserData(userId: string): Promise<User> {
    const response = await fetch(`/api/users/${userId}`);
    if (!response.ok) throw new Error('Failed to fetch user');
    return response.json();
  }

  render(): ReactNode {
    // TypeScript știe tipurile
    const { user, loading, error } = this.state;
    // ...
  }
}
```

---

## **Interfețe în TypeScript**

### **1. Interfețe pentru Props și State**

```typescript
// Interfață simplă
interface ButtonProps {
  title: string;
  onPress: () => void;
  disabled?: boolean;
  style?: ViewStyle;
}

// Interfață extinsă
interface IconButtonProps extends ButtonProps {
  icon: string;
  iconSize?: number;
  iconColor?: string;
}

// Interfață cu generics
interface ListProps<T> {
  data: T[];
  renderItem: (item: T) => ReactElement;
  keyExtractor: (item: T) => string;
}

// Utilizare în component funcțional
const IconButton: React.FC<IconButtonProps> = ({ 
  title, 
  onPress, 
  icon, 
  iconSize = 24 
}) => {
  return (
    <TouchableOpacity onPress={onPress}>
      <Icon name={icon} size={iconSize} />
      <Text>{title}</Text>
    </TouchableOpacity>
  );
};
```

---

### **2. Interfețe pentru Modele de Date**

```typescript
// Interfețe pentru domeniul aplicației
interface Message {
  id: string;
  text: string;
  timestamp: Date;
  sender: User;
  status: 'sent' | 'delivered' | 'read';
}

interface Chat {
  id: string;
  participants: User[];
  messages: Message[];
  lastMessage?: Message;
  unreadCount: number;
}

// Interfață pentru API Response
interface ApiResponse<T> {
  data: T;
  status: number;
  message?: string;
  error?: string;
}

// Type Guards pentru validare
function isUser(obj: any): obj is User {
  return obj && 
    typeof obj.id === 'string' && 
    typeof obj.name === 'string';
}

// Utilizare
const response: ApiResponse<User> = await api.getUser();
if (isUser(response.data)) {
  // TypeScript știe că e User
  console.log(response.data.name);
}
```

---

## **Clase Service/Manager**

### **1. Singleton Pattern - Manager Classes**

```typescript
// NavigationManager - Singleton
class NavigationManager {
  private static instance: NavigationManager;
  private navigationRef: React.RefObject<NavigationContainerRef>;

  private constructor() {
    this.navigationRef = React.createRef();
  }

  static getInstance(): NavigationManager {
    if (!NavigationManager.instance) {
      NavigationManager.instance = new NavigationManager();
    }
    return NavigationManager.instance;
  }

  navigate(screen: string, params?: object): void {
    this.navigationRef.current?.navigate(screen, params);
  }

  goBack(): void {
    this.navigationRef.current?.goBack();
  }

  reset(routes: any[]): void {
    this.navigationRef.current?.reset({
      index: 0,
      routes
    });
  }
}

// Utilizare
const nav = NavigationManager.getInstance();
nav.navigate('Profile', { userId: '123' });
```

---

### **2. Service Classes pentru Business Logic**

```typescript
// API Service Class
interface IApiService {
  get<T>(endpoint: string): Promise<T>;
  post<T>(endpoint: string, data: any): Promise<T>;
  put<T>(endpoint: string, data: any): Promise<T>;
  delete(endpoint: string): Promise<void>;
}

class ApiService implements IApiService {
  private baseURL: string;
  private token: string | null = null;

  constructor(baseURL: string) {
    this.baseURL = baseURL;
  }

  setAuthToken(token: string): void {
    this.token = token;
  }

  private getHeaders(): HeadersInit {
    return {
      'Content-Type': 'application/json',
      ...(this.token && { 'Authorization': `Bearer ${this.token}` })
    };
  }

  async get<T>(endpoint: string): Promise<T> {
    const response = await fetch(`${this.baseURL}${endpoint}`, {
      method: 'GET',
      headers: this.getHeaders()
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    return response.json();
  }

  async post<T>(endpoint: string, data: any): Promise<T> {
    const response = await fetch(`${this.baseURL}${endpoint}`, {
      method: 'POST',
      headers: this.getHeaders(),
      body: JSON.stringify(data)
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    return response.json();
  }

  async put<T>(endpoint: string, data: any): Promise<T> {
    // Similar implementation
    return {} as T;
  }

  async delete(endpoint: string): Promise<void> {
    // Implementation
  }
}

// Utilizare
const api = new ApiService('https://api.example.com');
api.setAuthToken('jwt-token');

const users = await api.get<User[]>('/users');
const newUser = await api.post<User>('/users', { name: 'John' });
```

---

### **3. Repository Pattern**

```typescript
// Abstract Repository
abstract class Repository<T> {
  protected api: ApiService;
  protected endpoint: string;

  constructor(api: ApiService, endpoint: string) {
    this.api = api;
    this.endpoint = endpoint;
  }

  async getAll(): Promise<T[]> {
    return this.api.get<T[]>(this.endpoint);
  }

  async getById(id: string): Promise<T> {
    return this.api.get<T>(`${this.endpoint}/${id}`);
  }

  async create(item: Partial<T>): Promise<T> {
    return this.api.post<T>(this.endpoint, item);
  }

  async update(id: string, item: Partial<T>): Promise<T> {
    return this.api.put<T>(`${this.endpoint}/${id}`, item);
  }

  async delete(id: string): Promise<void> {
    return this.api.delete(`${this.endpoint}/${id}`);
  }
}

// Concrete Repository
class UserRepository extends Repository<User> {
  constructor(api: ApiService) {
    super(api, '/users');
  }

  // Metode specifice pentru User
  async getUserByEmail(email: string): Promise<User> {
    return this.api.get<User>(`${this.endpoint}/email/${email}`);
  }

  async updateAvatar(userId: string, avatarUrl: string): Promise<User> {
    return this.api.put<User>(`${this.endpoint}/${userId}/avatar`, {
      avatar: avatarUrl
    });
  }
}

// Utilizare
const userRepo = new UserRepository(api);
const user = await userRepo.getById('123');
const users = await userRepo.getAll();
await userRepo.updateAvatar('123', 'new-avatar.jpg');
```

---

## **Moștenire și Polimorfism**

```typescript
// Clasă de bază abstractă
abstract class BaseScreen<P = {}, S = {}> extends Component<P, S> {
  protected abstract screenName: string;
  
  componentDidMount() {
    this.trackScreen();
    this.initialize();
  }

  protected abstract initialize(): void;

  private trackScreen(): void {
    Analytics.track('Screen_View', {
      screen: this.screenName,
      timestamp: Date.now()
    });
  }

  protected showError(message: string): void {
    Alert.alert('Error', message);
  }

  protected showLoading(): void {
    // Show loading indicator
  }

  protected hideLoading(): void {
    // Hide loading indicator
  }
}

// Clasă derivată
class ProfileScreen extends BaseScreen<ProfileProps, ProfileState> {
  protected screenName = 'Profile';

  protected initialize(): void {
    // Implementare specifică
    this.loadUserProfile();
  }

  private async loadUserProfile(): Promise<void> {
    this.showLoading();
    try {
      // Load profile
    } catch (error) {
      this.showError('Failed to load profile');
    } finally {
      this.hideLoading();
    }
  }

  render() {
    return <View>{/* UI */}</View>;
  }
}
```

---

## **Factory Pattern cu Clase**

```typescript
// Factory pentru crearea componentelor
interface INotification {
  id: string;
  message: string;
  show(): void;
  dismiss(): void;
}

class ToastNotification implements INotification {
  constructor(public id: string, public message: string) {}

  show(): void {
    Toast.show({
      text: this.message,
      duration: Toast.durations.SHORT
    });
  }

  dismiss(): void {
    Toast.hide();
  }
}

class AlertNotification implements INotification {
  constructor(public id: string, public message: string) {}

  show(): void {
    Alert.alert('Notification', this.message);
  }

  dismiss(): void {
    // Alerts dismiss automatically
  }
}

class NotificationFactory {
  static create(
    type: 'toast' | 'alert', 
    id: string, 
    message: string
  ): INotification {
    switch (type) {
      case 'toast':
        return new ToastNotification(id, message);
      case 'alert':
        return new AlertNotification(id, message);
      default:
        throw new Error(`Unknown notification type: ${type}`);
    }
  }
}

// Utilizare
const notification = NotificationFactory.create('toast', '1', 'Hello!');
notification.show();
```

---

## **State Management cu Clase**

```typescript
// MobX Store Class
import { makeObservable, observable, action, computed } from 'mobx';

class UserStore {
  user: User | null = null;
  isLoading = false;
  error: string | null = null;

  constructor(private api: ApiService) {
    makeObservable(this, {
      user: observable,
      isLoading: observable,
      error: observable,
      login: action,
      logout: action,
      isAuthenticated: computed
    });
  }

  get isAuthenticated(): boolean {
    return this.user !== null;
  }

  async login(email: string, password: string): Promise<void> {
    this.isLoading = true;
    this.error = null;

    try {
      const response = await this.api.post<{ user: User; token: string }>(
        '/auth/login',
        { email, password }
      );
      
      this.user = response.user;
      this.api.setAuthToken(response.token);
    } catch (error) {
      this.error = error.message;
      throw error;
    } finally {
      this.isLoading = false;
    }
  }

  logout(): void {
    this.user = null;
    this.api.setAuthToken('');
  }
}

// Utilizare cu hooks
const useUserStore = () => {
  const store = useContext(UserStoreContext);
  return useObserver(() => ({
    user: store.user,
    isLoading: store.isLoading,
    isAuthenticated: store.isAuthenticated,
    login: store.login.bind(store),
    logout: store.logout.bind(store)
  }));
};
```

---

## **Native Module Classes**

```java
// Android - Java Class
public class BiometricModule extends ReactContextBaseJavaModule {
    private static final String MODULE_NAME = "BiometricAuth";
    
    public BiometricModule(ReactApplicationContext context) {
        super(context);
    }

    @Override
    public String getName() {
        return MODULE_NAME;
    }

    @ReactMethod
    public void authenticate(String reason, Promise promise) {
        // Implementare autentificare biometrică
        BiometricPrompt.PromptInfo promptInfo = 
            new BiometricPrompt.PromptInfo.Builder()
                .setTitle("Authentication Required")
                .setSubtitle(reason)
                .build();
        
        // Logic pentru autentificare
        promise.resolve(true);
    }
}
```

```typescript
// TypeScript Interface pentru Native Module
interface IBiometricModule {
  authenticate(reason: string): Promise<boolean>;
  isAvailable(): Promise<boolean>;
  getSupportedTypes(): Promise<string[]>;
}

// Wrapper Class
class BiometricService implements IBiometricModule {
  private nativeModule: any;

  constructor() {
    this.nativeModule = NativeModules.BiometricAuth;
    if (!this.nativeModule) {
      throw new Error('BiometricAuth native module not found');
    }
  }

  async authenticate(reason: string): Promise<boolean> {
    try {
      return await this.nativeModule.authenticate(reason);
    } catch (error) {
      console.error('Biometric authentication failed:', error);
      return false;
    }
  }

  async isAvailable(): Promise<boolean> {
    return await this.nativeModule.isAvailable();
  }

  async getSupportedTypes(): Promise<string[]> {
    return await this.nativeModule.getSupportedTypes();
  }
}

export default new BiometricService();
```

---

## **Când să Folosești Clase vs Funcții**

### **Folosește Clase pentru:**

- **Service layers** - API, Database, Authentication
- **Singleton patterns** - Managers, Controllers
- **Complex state management** - MobX stores
- **Native modules** - Bridge către cod nativ
- **Legacy code** - Componente vechi care nu pot fi refactorizate

### **Folosește Funcții pentru:**

- **UI Components** - React Hooks sunt mai simpli
- **Utilities** - Funcții pure fără state
- **Custom Hooks** - Logică reutilizabilă
- **Componente noi** - Standard modern React

---

## **Best Practices**

```typescript
// 1. COMPOSITION peste INHERITANCE
// ✅ Bun - Composition
interface ILogger {
  log(message: string): void;
}

class ConsoleLogger implements ILogger {
  log(message: string): void {
    console.log(message);
  }
}

class ApiClient {
  constructor(private logger: ILogger) {}
  
  async fetch(url: string) {
    this.logger.log(`Fetching ${url}`);
    // ...
  }
}

// 2. DEPENDENCY INJECTION
class App {
  private userService: UserService;
  private authService: AuthService;

  constructor(
    userService: UserService,
    authService: AuthService
  ) {
    this.userService = userService;
    this.authService = authService;
  }
}

// 3. INTERFACE SEGREGATION
// ✅ Bun - Interfețe mici și specifice
interface Readable {
  read(): string;
}

interface Writable {
  write(data: string): void;
}

// ❌ Rău - Interfață prea mare
interface ReadWrite {
  read(): string;
  write(data: string): void;
  delete(): void;
  update(): void;
}
```

Conceptele OOP rămân relevante în React Native, în special pentru arhitectură, type safety și integrare cu cod nativ, chiar dacă componenetele UI moderne folosesc paradigma funcțională.