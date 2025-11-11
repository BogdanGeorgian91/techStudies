**Source**: https://medium.com/kostiantyn-kolosov/ios-in-the-cloud-how-to-make-friendship-between-cloudkit-coredata-swiftdata-and-widgetkit-f22431d4eaf6

**Project State:**

- **Architecture**: None
- **Modularised**: Yes
- **Frontend**: UIKit + SwiftUI
- **Sync**: CloudKit, CoreData, SwiftData, WidgetKit
- **Level**: Medium

## Preamble (Stream of Thoughts, you can skip to Step 1)

When iOS 17 was released, I wasn't too excited about the new features. Most of my reservations stemmed from my conservative views. Yes, I'm one of those who prefer old, tried-and-true tools and don't trust anything that hasn't stood the test of time and a heavy production load. However, to understand if SwiftUI and SwiftData would suit me, I decided to use them for my latest pet project.

## What's the Point with SwiftData?

- There's a lack of clear answers to pressing questions. In fact, since the framework is based on CoreData, which in turn is based on SQLite, most questions are resolved by the underlying base. But on the other hand, do we really need yet another method of interacting with the database?
- **Performance?** It's very close to CoreData since they are based on the same core.
- **Security?** Possibly. **Thread safety?** Here it is! The practice of multithreading is increasingly infiltrating our lives, bringing more or less justified complexities. I believe you are already familiar with the classic problems of multithreading, but if not, there are many excellent articles where authors explain very clearly and in detail what it is and how to work with it.
- **Convenient syntax.** I'm not striving for perfect code, but this time it's one of those cases where I've been convinced otherwise. Naturally, it's something to get used to, because when you're used to working with the convenient (albeit limited) CoreData editor, adaptation takes time.

### Practical Application

I'm a pragmatist, and the purpose of this article is not to introduce you to the basics, but to show one method of implementing this into an already existing application.

## Step 1: Create a New Project

▶︎ **UIKit** for demonstrate UIViewController and SwiftUI View implementation + navigation using UIViewController

▶︎ We will use our own Persistence Controller, so let's set storage as **None**.
![[xcode)_1.png]]

And create our xcdatamodel. I named it `CoreDataModel.xcdatamodeld`
![[xcode_2.png]]

Of course we will format our fresh project. You can use your own formatting macro, I use something like that.

![[xcode_3.png]]

**Hint**: Don't forget to update info.plist path on the Build Settings
![[xcode_4.png]]

## Step 2: Connect CloudKit

Tap on **"+ Capability"** and Choose **"iCloud"**
![[xcode_5.png]]


Then check **CloudKit** and add a container by tapping **"+"**

![[xcode_6.png]]

![[Screenshot 2025-09-03 at 16.53.57.png]]
## Step 3: Create Data Models

Let's create base for our storing objects

**CoreData**: (check resources if you don't know how to add the Entity)

![[xcode10.png]]
Set module and Codegen as mentioned on the second picture to make CloudKit work with it
### CoreDataNote.swift

```swift
import CoreData  
  
class CoreDataNote: NSManagedObject { }
```

**SwiftData:**

> **Hint**: Make sure that your properties have a default value or you will get fatal error from the CloudKit

### SwiftDataNote.swift

```swift
import Foundation  
import SwiftData  

@Model  
class SwiftDataNote: Identifiable {  
    var id: UUID = UUID.init()  
    var title: String = ""  
    var colorIndex: Int = 0  
  
    init(title: String, colorIndex: Int) {  
        self.title = title  
        self.colorIndex = colorIndex  
    }  
}
```

## Step 4: App Group

To pass data between widget and main app we should create bridge between it.

Tap **"+ Capability"** and select **"App Groups"**
![[xdo12.png]]

![[Screenshot 2025-09-03 at 16.57.10.png]]
## Step 5: Let's Match This!

Okay, we created a new project. What's next? Of course we should create a bridge between our app and its internal data storage.

Create a new file named `Persistence.swift` and pass it in your project

I based this code on the default implementation from Xcode, just extended its functionality

Let's start, here is the full code of this Manager:

### Persistence.swift

```swift
import Foundation  
import CoreData  
import SwiftData  
  
protocol Persistence {  
    // CoreData ManagedContext  
    var cdContext: NSManagedObjectContext { get }  
  
    // SwiftData Managed Container  
    var sdContainer: ModelContainer { get }  
}  
  
struct PersistenceController: Persistence {  
      
    static var shared: Persistence = PersistenceController()  
  
    // Persistence Protocol  
    var sdContainer: ModelContainer  
      
    var cdContext: NSManagedObjectContext { container.viewContext }  
  
    // Internal  
    let container: NSPersistentCloudKitContainer  
  
    // Private  
    private init(inMemory: Bool = false) {  
        let coreDataDatabaseName = "CoreDataModel"  
        container = NSPersistentCloudKitContainer(name: coreDataDatabaseName)  
        container.configure(inMemory: inMemory, databaseName: coreDataDatabaseName)  
  
        sdContainer = ModelContainer.getConfiguredSwiftDataContainer(inMemory: inMemory)  
    }  
}  
  
// MARK: - Private extensions  
private extension NSPersistentCloudKitContainer {  
    func configure(inMemory: Bool, databaseName: String) {  
        self.viewContext.automaticallyMergesChangesFromParent = true  
        if inMemory {  
            self.persistentStoreDescriptions.first!.url = URL(fileURLWithPath: "/dev/null")  
        } else {  
            let url = URL.storeURL(for: "group.com.KKolosov.CloudKitIsAMagic", databaseName: databaseName)  
            self.persistentStoreDescriptions.first!.url = url  
        }  
        self.loadPersistentStores(completionHandler: { (storeDescription, error) in  
            if let error = error as NSError? {  
                fatalError("Unresolved error \(error), \(error.userInfo)")  
            }  
        })  
    }  
}  
  
private extension ModelContainer {  
    static func getConfiguredSwiftDataContainer(inMemory: Bool) -> ModelContainer {  
        let config = ModelConfiguration(isStoredInMemoryOnly: inMemory)  
        let swiftDataContainer = try! ModelContainer(  
            for: SwiftDataNote.self,  
            configurations: config)  
        return swiftDataContainer  
    }  
}  
  
private extension URL {  
    /// Returns a URL for the given app group and database pointing to the sqlite database.  
    static func storeURL(for appGroup: String, databaseName: String) -> URL {  
        var containerUrl: URL  
  
        guard let initialContainer = FileManager.default.containerURL(forSecurityApplicationGroupIdentifier: appGroup) else {  
            fatalError("Shared file container could not be created.")  
        }  
        #if os(tvOS)  
        containerUrl = initialContainer.appendingPathComponent("Library/Caches")  
        #else  
        containerUrl = initialContainer  
        #endif  
  
        return containerUrl.appendingPathComponent("\(databaseName).sqlite")  
    }  
}  
  
// MARK: - Syntax Sugar  
extension ModelContainer {  
    static var current: ModelContainer { PersistenceController.shared.sdContainer }  
}  
  
extension NSManagedObjectContext {  
    static var current: NSManagedObjectContext { PersistenceController.shared.cdContext }  
}
```

## Step 6: Editor

For our example we will not create a totally accurate and high functional note editor, we will create note with color and title.

### NotificationCenterNames.swift

```swift
extension Notification.Name {  
    static var dataFlowUpdated: Notification.Name = .init("Data Flow Updated")  
}
```

### NoteEditorView.swift

```swift
import SwiftUI  
import SwiftData  
  
struct NoteEditorView: View {  
    @Environment(\.dismiss) var dismiss  
    @Environment(\.managedObjectContext) var coreDataContext  
    @Environment(\.modelContext) var swiftDataContext  
  
    @State var title: String = ""  
    @State var selectedColorIndex: Int = 0  
  
    @State var error: VerifyError?  
    @State var errorAlertIsPresented: Bool = false  
  
    var body: some View {  
        NavigationStack {  
            VStack {  
                editorView  
                Spacer()  
                addButtons  
            }  
            .navigationTitle("Create a new Note")  
            .alert(isPresented: $errorAlertIsPresented, error: error) {  
                Button("Ok") { }  
            }  
        }  
    }  
  
    @ViewBuilder  
    private var editorView: some View {  
        VStack {  
            TextField("Start typing here", text: $title)  
                .textFieldStyle(.roundedBorder)  
                .padding(.vertical)  
            colorsView  
        }  
        .padding(.horizontal)  
    }  
  
    @ViewBuilder  
    private var colorsView: some View {  
        ScrollView {  
            HStack(spacing: 0) {  
                ForEach(NoteColor.allCases, id: \.id) { color in  
                    Circle()  
                        .frame(width: 24, height: 24)  
                        .foregroundStyle(Color(color.colorRepresentation))  
                        .overlay {  
                            if color.rawValue == selectedColorIndex {  
                                Circle()  
                                    .stroke(Color(color.colorRepresentation), lineWidth: 1.5)  
                                    .frame(width: 28, height: 28)  
                            }  
                        }  
                        .onTapGesture {  
                            withAnimation { self.selectedColorIndex = color.rawValue }  
                        }  
                        .padding(.horizontal, 8)  
                }  
                Spacer()  
            }  
            .frame(height: 30)  
        }  
    }  
  
    @ViewBuilder  
    private var addButtons: some View {  
        VStack {  
            Divider()  
            HStack {  
                Button("Add CD Note", systemImage: "tray.and.arrow.down", action: addCoreDataNote)  
                Button("Add SD Note", systemImage: "swiftdata", action: addSwiftDataNote)  
            }  
            .buttonStyle(.borderedProminent)  
        }  
        .padding(.bottom)  
    }  
  
    private func verify() throws {  
        guard !title.isEmpty else { throw VerifyError.titleIsMissing }  
    }  
  
    private func addCoreDataNote() {  
        do {  
            try verify()  
            let newCoreDataNote = CoreDataNote(context: coreDataContext)  
            newCoreDataNote.title = title  
            newCoreDataNote.colorIndex = Int16(selectedColorIndex)  
            newCoreDataNote.id = .init()  
            try? coreDataContext.save()  
            NotificationCenter.default.post(name: .dataFlowUpdated, object: nil)  
            dismiss()  
        } catch {  
            self.error = error as? VerifyError  
        }  
    }  
  
    private func addSwiftDataNote() {  
        do {  
            try verify()  
            let newSwiftDataNote = SwiftDataNote(title: title, colorIndex: selectedColorIndex)  
            swiftDataContext.insert(newSwiftDataNote)  
            try? swiftDataContext.save()  
            NotificationCenter.default.post(name: .dataFlowUpdated, object: nil)  
            dismiss()  
        } catch {  
            self.error = error as? VerifyError  
        }  
    }  
}  
  
enum VerifyError: LocalizedError {  
    case titleIsMissing  
  
    var errorDescription: String? {  
        "Enter title, please"  
    }  
}
```

And enum with colors

### NoteColor.swift

```swift
import UIKit  
  
protocol ColorRepresentable: Identifiable {  
    var colorRepresentation: UIColor { get }  
}  
  
enum NoteColor: Int, CaseIterable, ColorRepresentable {  
    var id: Self { self }  
      
    case red, blue, green, purple, orange  
  
    var colorRepresentation: UIColor {  
        switch self {  
        case .red: .red  
        case .blue: .blue  
        case .green: .green  
        case .purple: .purple  
        case .orange: .orange  
        }  
    }  
}
```

## Step 7: Preparing Our Main Screen

Open `main.storyboard` and Embed it in Navigation Controller

![[Screenshot 2025-09-03 at 16.57.46.png]]

![[Screenshot 2025-09-03 at 16.58.27.png]]

## Step 8: SwiftUI Dashboard

It is an implementation of the first variant of the Dashboard. Will be presented inside UIHostingController on our UITabBarController

### DashboardView.swift

```swift
import SwiftUI  
import SwiftData  
  
struct DashboardView: View {  
    @Query(animation: .default) var swiftDataNotes: [SwiftDataNote]  
    @FetchRequest(sortDescriptors: [], animation: .default) var coreDataNotes: FetchedResults<CoreDataNote>  
  
    @Environment(\.managedObjectContext) var coreDataContext  
    @Environment(\.modelContext) var swiftDataContext  
  
    var body: some View {  
        List {  
            swiftDataSection  
            coreDataSection  
        }  
    }  
  
    @ViewBuilder  
    private var swiftDataSection: some View {  
        Section {  
            ForEach(swiftDataNotes) { note in  
                HStack {  
                    Image(systemName: "circle.fill")  
                        .imageScale(.small)  
                        .foregroundStyle(Color(NoteColor(rawValue: note.colorIndex)!.colorRepresentation))  
                    Text(note.title)  
                }  
                .swipeActions(edge: .trailing, allowsFullSwipe: true) {  
                    Button("Delete") { deleteSwiftDataNote(note) }  
                        .tint(.red)  
                }  
            }  
        } header: {  
            Text("SwiftData Notes")  
        }  
    }  
  
    @ViewBuilder  
    private var coreDataSection: some View {  
        Section {  
            ForEach(coreDataNotes) { note in  
                HStack {  
                    Image(systemName: "circle.fill")  
                        .imageScale(.small)  
                        .foregroundStyle(Color(NoteColor(rawValue: Int(note.colorIndex))!.colorRepresentation))  
                    Text(note.title ?? "")  
                }  
                .swipeActions(edge: .trailing, allowsFullSwipe: true) {  
                    Button("Delete") { deleteCoreDataNote(note) }  
                        .tint(.red)  
                }  
            }  
        } header: {  
            Text("CoreData Notes")  
        }  
    }  
  
    private func deleteSwiftDataNote(_ note: SwiftDataNote) {  
        swiftDataContext.delete(note)  
        try? swiftDataContext.save()  
    }  
  
    private func deleteCoreDataNote(_ note: CoreDataNote) {  
        coreDataContext.delete(note)  
        try? coreDataContext.save()  
    }  
}
```

And let's implement UIKit variant of the Dashboard

### DashboardViewController.swift

```swift
import SwiftUI  
import SwiftData  
  
class DashboardViewController: UITableViewController {  
    var swiftDataNotes: [SwiftDataNote] = []  
    var coreDataNotes: [CoreDataNote] = []  
  
    override init(style: UITableView.Style) {  
        super.init(style: style)  
    }  
      
    required init?(coder: NSCoder) {  
        fatalError("init(coder:) has not been implemented")  
    }  
      
    override func viewDidLoad() {  
        super.viewDidLoad()  
  
        self.fetchData()  
        self.initDataFlow()  
    }  
  
    private func initDataFlow() {  
        NotificationCenter.default.addObserver(forName: .dataFlowUpdated, object: nil, queue: .main) { [weak self] _ in  
            guard let self else { return }  
            fetchData()  
        }  
    }  
  
    private func fetchData() {  
        Task {  
            do {  
                let swiftDataContext = PersistenceController.shared.sdContainer.mainContext  
                self.swiftDataNotes = try swiftDataContext.fetch(FetchDescriptor<SwiftDataNote>())  
                DispatchQueue.main.async { self.tableView.reloadData() }  
            } catch {  
                fatalError(error.localizedDescription)  
            }  
        }  
  
        Task {  
            do {  
                let coreDataContext = PersistenceController.shared.cdContext  
                self.coreDataNotes = try coreDataContext.fetch(CoreDataNote.fetchRequest())  
                DispatchQueue.main.async { self.tableView.reloadData() }  
            } catch {  
                fatalError(error.localizedDescription)  
            }  
        }  
    }  
}
```

### DashboardViewController+TableView.swift

```swift
import UIKit  
  
extension DashboardViewController {  
    override func numberOfSections(in tableView: UITableView) -> Int { 
        DashboardSections.allCases.count 
    }  
  
    override func tableView(_ tableView: UITableView, titleForHeaderInSection section: Int) -> String? {  
        guard let section = DashboardSections(rawValue: section) else { fatalError() }  
        switch section {  
        case .swiftData: return "SwiftData"  
        case .coreData: return "CoreData"  
        }  
    }  
  
    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {  
        guard let section = DashboardSections(rawValue: section) else { fatalError() }  
        switch section {  
        case .swiftData: return swiftDataNotes.count  
        case .coreData: return coreDataNotes.count  
        }  
    }  
  
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {  
        let cell = UITableViewCell(style: .default, reuseIdentifier: nil)  
        guard let section = DashboardSections(rawValue: indexPath.section) else { fatalError() }  
        
        switch section {  
        case .swiftData:  
            cell.textLabel?.text = swiftDataNotes[indexPath.row].title  
            cell.imageView?.image = .init(systemName: "circle.fill", 
                                        withConfiguration: UIImage.SymbolConfiguration(scale: .small))  
            cell.imageView?.tintColor = NoteColor(rawValue: swiftDataNotes[indexPath.row].colorIndex)!.colorRepresentation  
        case .coreData:  
            cell.textLabel?.text = coreDataNotes[indexPath.row].title  
            cell.imageView?.image = .init(systemName: "circle.fill", 
                                        withConfiguration: UIImage.SymbolConfiguration(scale: .small))  
            cell.imageView?.tintColor = NoteColor(rawValue: Int(coreDataNotes[indexPath.row].colorIndex))!.colorRepresentation  
        }  
        return cell  
    }  
  
    override func tableView(_ tableView: UITableView, trailingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {  
        let deleteAction = UIContextualAction(style: .destructive, title: "Delete") { [weak self] _, _, _ in  
            guard let self else { return }  
            guard let section = DashboardSections(rawValue: indexPath.section) else { fatalError() }  
            let offset = indexPath.row  
              
            switch section {  
            case .swiftData:  
                Task {  
                    do {  
                        let swiftDataContext = PersistenceController.shared.sdContainer.mainContext  
                        swiftDataContext.delete(self.swiftDataNotes[offset])  
                        try swiftDataContext.save()  
                        self.swiftDataNotes.remove(at: offset)  
                        DispatchQueue.main.async {  
                            tableView.deleteRows(at: [indexPath], with: .automatic)  
                        }  
                    } catch {  
                        fatalError(error.localizedDescription)  
                    }  
                }  
            case .coreData:  
                do {  
                    let coreDataContext = PersistenceController.shared.cdContext  
                    coreDataContext.delete(self.coreDataNotes[offset])  
                    try coreDataContext.save()  
                    self.coreDataNotes.remove(at: offset)  
                    DispatchQueue.main.async {  
                        tableView.deleteRows(at: [indexPath], with: .automatic)  
                    }  
                } catch {  
                    fatalError(error.localizedDescription)  
                }  
            }  
        }  
  
        return .init(actions: [deleteAction])  
    }  
}  
  
private enum DashboardSections: Int, CaseIterable {  
    case swiftData, coreData  
}
```

### CloudKitManager.swift

```swift
import Foundation  
import CloudKit  
import Combine  
import CoreData  
import WidgetKit  
  
final class CloudKitManager {  
    public static let shared = CloudKitManager()  
    fileprivate var disposables = Set<AnyCancellable>()  
  
    private let cloudKitDebugIsEnabled = false  
  
    private init() {  
        NotificationCenter.default.publisher(for: NSPersistentCloudKitContainer.eventChangedNotification)  
            .sink(receiveValue: { notification in  
                if let cloudEvent = notification.userInfo?[NSPersistentCloudKitContainer.eventNotificationUserInfoKey]  
                    as? NSPersistentCloudKitContainer.Event {  
                    if cloudEvent.endDate == nil {  
                        debugPrint("Starting an event...")  
                    } else {  
                        switch cloudEvent.type {  
                        case .import:  
                            debugPrint("An import finished!")  
                            DispatchQueue.main.async {  
                                NotificationCenter.default  
                                    .post(name: .dataFlowUpdated, object: nil)  
                                WidgetCenter.shared.reloadAllTimelines()  
                            }  
                        default: break  
                        }  
                    }  
                }  
            })  
            .store(in: &disposables)  
    }  
}
```

And trigger this manager initialization

Open `AppDelegate.swift`

And pass `_ = CloudKitManager.shared` into `application didFinishLaunchingWithOptions`

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {  
    // Override point for customization after application launch.  
    _ = CloudKitManager.shared  
    return true  
}
```

And finally! Let's make friendship between those dashboards!

Open `ViewController.swift` and fill it like that:

```swift
import UIKit  
import SwiftUI  
  
class ViewController: UITabBarController {  
  
    override func viewDidLoad() {  
        super.viewDidLoad()  
  
        self.configureNavigationController()  
        self.configureScreen()  
    }  
  
    // MARK: - Private  
    private func configureNavigationController() {  
        self.navigationItem.rightBarButtonItem = UIBarButtonItem(
            image: .init(systemName: "plus"), 
            style: .plain,  
            target: self, 
            action: #selector(routeToAddNote))  
    }  
  
    private func configureScreen() {  
        let swiftUIView = UIHostingController(rootView: DashboardView()  
            .environment(\.managedObjectContext, .current)  
            .modelContainer(.current))  
        swiftUIView.tabBarItem.image = .init(systemName: "swift")  
        swiftUIView.tabBarItem.title = "SwiftUI"  
  
        let uiKitController = DashboardViewController(style: .insetGrouped)  
        uiKitController.tabBarItem.image = .init(systemName: "mosaic.fill")  
        uiKitController.tabBarItem.title = "UIKit"  
  
        self.viewControllers = [ swiftUIView, uiKitController ]  
    }  
  
    // MARK: - Actions  
    @objc private func routeToAddNote() {  
        let view = NoteEditorView()  
            .environment(\.managedObjectContext, .current)  
            .modelContainer(.current)  
        present(UIHostingController(rootView: view), animated: true)  
    }  
}
```

So, what do we have now?

**We have sync between devices: CloudKit**  
**Application: Dashboard with SwiftData and CoreData, Create a new note screen and seamless update**  
**SQLite interaction: CoreData and SwiftData inside one project working stick together**

## Step 9: Widget

**Disclaimer!** This will be just an example widget, we will not make business with the timelines, interaction, configuration, live activity, etc. This guide is just about passing data between app and widget.

**File** → **New** → **Target**  
**Widget Extension**

![[Screenshot 2025-09-03 at 16.58.59.png]]

![[Screenshot 2025-09-03 at 16.59.18.png]]
Set `Persistence.swift`, `SwiftDataNote.swift`, `CoreDataModel.xcdatamodeld`, `CoreDataNote.swift` membership on the Widget Target

![[Screenshot 2025-09-03 at 16.59.33.png]]

Add AppContainer like we do on the main project

![[Screenshot 2025-09-03 at 16.59.48.png]]

And edit `NotesWidget.swift`

```swift
import WidgetKit  
import SwiftUI  
import CoreData  
import SwiftData  
  
struct Provider: TimelineProvider {  
    func placeholder(in context: Context) -> SimpleEntry {  
        SimpleEntry(date: Date())  
    }  
  
    func getSnapshot(in context: Context, completion: @escaping (SimpleEntry) -> ()) {  
        let entry = SimpleEntry(date: Date())  
        completion(entry)  
    }  
  
    func getTimeline(in context: Context, completion: @escaping (Timeline<Entry>) -> ()) {  
        var entries: [SimpleEntry] = []  
  
        // Generate a timeline consisting of five entries an hour apart, starting from the current date.  
        let currentDate = Date()  
        for hourOffset in 0 ..< 5 {  
            let entryDate = Calendar.current.date(byAdding: .hour, value: hourOffset, to: currentDate)!  
            let entry = SimpleEntry(date: entryDate)  
            entries.append(entry)  
        }  
  
        let timeline = Timeline(entries: entries, policy: .atEnd)  
        completion(timeline)  
    }  
}  
  
struct SimpleEntry: TimelineEntry {  
    let date: Date  
}  
  
struct NotesWidgetEntryView: View {  
    @Query(animation: .default) var swiftDataNotes: [SwiftDataNote]  
    @FetchRequest(sortDescriptors: [], animation: .default) var coreDataNotes: FetchedResults<CoreDataNote>  
  
    var entry: Provider.Entry  
  
    var body: some View {  
        VStack(alignment: .leading) {  
            Label("SwiftData", systemImage: "swiftdata")  
            Text("Notes: \(swiftDataNotes.count)")  
            if let swiftDataNote = swiftDataNotes.first {  
                Text("\(swiftDataNote.title)")  
            }  
  
            Divider()  
  
            Label("CoreData", systemImage: "tray.full.fill")  
            Text("Notes: \(coreDataNotes.count)")  
            if let coreDataNote = coreDataNotes.first {  
                Text("\(coreDataNote.title ?? "")")  
            }  
        }  
        .font(.callout)  
    }  
}  
  
struct NotesWidget: Widget {  
    let kind: String = "NotesWidget"  
  
    var body: some WidgetConfiguration {  
        StaticConfiguration(kind: kind, provider: Provider()) { entry in  
            NotesWidgetEntryView(entry: entry)  
                .containerBackground(.fill.tertiary, for: .widget)  
                .environment(\.managedObjectContext, .current)  
                .modelContainer(.current)  
        }  
        .configurationDisplayName("My Widget")  
        .description("This is an example widget.")  
    }  
}
```

And enjoy the result!
![[Screenshot 2025-09-03 at 17.00.01.png]]
## Known Problems

- **Widget does not reload after notes change from another device.** This article contains overmuch content and this is not the main target of it, so, if you want updates for the widget in this case, this is not so hard and resources like StackOverflow contain very good answers.
- **Non-Architecture.** Of course.

## Resources

### Source Code

[https://github.com/Rayllienstery/CloudKitIsAMagic](https://github.com/Rayllienstery/CloudKitIsAMagic)

### Documentation References

**CoreData Entity**  
[https://developer.apple.com/documentation/coredata/modeling_data/configuring_entities](https://developer.apple.com/documentation/coredata/modeling_data/configuring_entities?changes=_3#)

**AppGroups**  
[https://developer.apple.com/documentation/xcode/configuring-app-groups](https://developer.apple.com/documentation/xcode/configuring-app-groups)

**CloudKitManager based on:** https://github.com/ggruen/CloudKitSyncMonitor/blob/main/Sources/CloudKitSyncMonitor/SyncMonitor.sw