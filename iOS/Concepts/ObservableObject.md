În dezvoltarea iOS, conceptul de `ObservableObject` este folosit pentru a crea clase care pot fi observate, adică obiecte ce pot notifica automat alte componente (de exemplu, interfețele grafice SwiftUI) atunci când proprietățile lor se schimbă. Acest mecanism permite actualizarea automată a UI-ului când datele din model sau ViewModel se schimbă.

### Ce este ObservableObject?
`ObservableObject` este un protocol care poate fi implementat de o clasă în Swift. Ori de câte ori o proprietate marcată cu `@Published` în această clasă se modifică, această schimbare va fi transmisă către toate componentele care observă obiectul respectiv.

### Cum funcționează?
- Clasa care implementează `ObservableObject` are proprietăți decorate cu wrapper-ul `@Published`.
- Atunci când aceste proprietăți se modifică, obiectul emite automat un eveniment de notificare.
- În SwiftUI, viziunea (View) care observă acest obiect prin `@ObservedObject` sau `@StateObject` va primi notificarea și va reface automat UI-ul pentru a reflecta noile date.

### Ce înseamnă în practică?
De exemplu, dacă ai o clasă ViewModel care gestionează starea unei aplicații, marcarea acesteia cu `ObservableObject` și a proprietăților relevante cu `@Published` face ca schimbările să fie propagate automat către componentele UI care depind de ele, fără să fie nevoie de logică suplimentară pentru actualizări.

### Exemplu simplu:

```swift
class UserSettings: ObservableObject {
    @Published var username: String = "Guest"
    @Published var isLoggedIn: Bool = false
}
```

Apoi, în SwiftUI View:

```swift
struct ContentView: View {
    @ObservedObject var settings = UserSettings()

    var body: some View {
        VStack {
            if settings.isLoggedIn {
                Text("Welcome, \(settings.username)!")
            } else {
                Text("Please log in.")
            }
        }
    }
}
```

În acest exemplu, dacă se modifică `isLoggedIn` sau `username`, UI-ul va fi actualizat automat.

### Evoluție recentă
În iOS 17 a fost introdusă o nouă macrocomandă `@Observable` care simplifică acest proces, eliminând necesitatea de a declara explicit `@Published` și conformitatea cu `ObservableObject`. Aceasta oferă un mod mai concis și mai performant de a crea obiecte observabile, însă `ObservableObject` rămâne relevant pentru compatibilitate cu versiuni mai vechi sau scenarii specifice.

Pe scurt, `ObservableObject` este baza pentru arhitectura reactivă în SwiftUI, permițând sincronizarea automată între modelul aplicației și interfața utilizatorului pe baza schimbărilor de stare [1][4][5][6].

Sources
[1] @Observable in SwiftUI explained – Donny Wals https://www.donnywals.com/observable-in-swiftui-explained/
[2] @Observable Macro performance increase over ... https://www.avanderlee.com/swiftui/observable-macro-performance-increase-observableobject/
[3] SwiftUI's Observable macro is not a drop-in replacement for ... https://www.jessesquires.com/blog/2024/09/09/swift-observable-macro/
[4] Simplifying State Management with @Observable and ... https://www.swiftlyandy.com/blog/observation/02-basics-observable-observedobject
[5] A Deep Dive Into Observation - A New Way to Boost ... https://fatbobman.com/en/posts/mastering-observation/
[6] Migrating from the Observable Object protocol to ... https://developer.apple.com/documentation/swiftui/migrating-from-the-observable-object-protocol-to-the-observable-macro
