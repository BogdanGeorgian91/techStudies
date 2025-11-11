Source: 
```
https://fatbobman.medium.com/core-data-with-cloudkit-part-2-syncing-local-database-to-icloud-private-database-fd80581ab360
```

In this article, we will explore the most common scenario in Core Data with CloudKit applications — syncing the local database to the iCloud private database. We will gradually unfold it from several perspectives:

- Directly supporting Core Data with CloudKit in new projects
- Considerations for creating syncable models
- Adding Host in CloudKit support to existing Core Data projects
- Selectively syncing data

## Table of Contents

- Part 1: The Basics
- Part 2: Syncing Local Database to iCloud Private Database
- Part 3: Exploring the CloudKit Dashboard
- Part 4: Troubleshooting
- Part 5: Synchronizing Public Database
- Part 6: Sharing Data in the iCloud

Enabling the Core Data with CloudKit feature in your application only requires a few steps:

- Use NSPersistentCloudKitContainer
- Add CloudKit support to the Signing & Capabilities of the project target
- Create or specify a CloudKit container for the project
- Add background support to the Signing & Capabilities of the project target
- Configure NSPersistentStoreDescription and viewContext
- Check whether the data model meets the synchronization requirements

## Directly Supporting Core Data with CloudKit in New Projects

In recent years, Apple has continuously improved the Core Data template of Xcode, and using the built-in template to create a project that supports Core Data with CloudKit is the most convenient way to get started.

### Creating a New Xcode Project

Create a new project, check **Use Core Data** and **Host in CloudKit** (formerly Use CloudKit) in the project settings, and set the development team (Team).

After setting the save location, Xcode will use a preset template to generate a project document that includes Core Data with CloudKit support.

Xcode may warn you about errors in the new project code. If you find it annoying, you can simply build the project to cancel the error prompt (generate NSManagedObject Subclass).

Next, we will follow the Quick Guide step by step.

### Set up PersistentCloudKitContainer

`Persistence.swift` is the Core Data stack created by the official template. Since Host in CloudKit was already selected when creating the project, the template code has directly used **NSPersistentCloudKitContainer** instead of NSPersistentContainer without modification.

```swift
let container: NSPersistentCloudKitContainer
```

### Enable CloudKit

Click the corresponding Target in the project, select **Signing & Capabilities**, and click **+Capability** to search for and add CloudKit support.

Check CloudKit. Click + and enter the CloudKit container name. Xcode will automatically add `iCloud.` in front of your CloudKit container name. The name of the container usually follows the reverse domain name convention and does not need to be consistent with the project or BundleID. If there is no developer team configured, you will not be able to create a container.

After adding CloudKit support, Xcode will automatically add the **Push Notifications** feature for you, as we discussed in the previous post.

### Enable Background Notifications

Click **+Capability** again, search for background, and add it. Check **Remote notifications**.

This feature allows your application to respond to silent notifications that are pushed when the content of cloud data changes.

### Configure NSPersistentStoreDescription and viewContext

In the current project's `.xcdatamodeld` file, there is only one default configuration, **Default**, in CONFIGURATIONS. When you click on it, you can see that **Used with CloudKit** on the right has already been checked.

If the developer has not customized Configuration in the Data Model Editor, and Used with CloudKit is checked, Core Data will use the selected CloudKit container to set `cloudKitContainerOptions`. Therefore, we do not need to make any additional settings to NSPersistentStoreDescription in the current `Persistence.swift` code (we will introduce how to set NSPersistentStoreDescription in the later section).

Configure the context in `Persistence.swift` as follows:

```swift
container.loadPersistentStores(completionHandler: { (storeDescription, error) in
    if let error = error as NSError? {
        // ... 
        fatalError("Unresolved error \(error), \(error.userInfo)")
    }
})

// Add the following code
container.viewContext.automaticallyMergesChangesFromParent = true
container.viewContext.mergePolicy = NSMergeByPropertyObjectTrumpMergePolicy
do {
    try container.viewContext.setQueryGenerationFrom(.current)
} catch {
    fatalError("Failed to pin viewContext to the current generation:\(error)")
}
```

**Key Configuration Explanations:**

- `container.viewContext.automaticallyMergesChangesFromParent = true` allows the view context to automatically merge data from server-side synchronization (import). Views using `@FetchRequest` or `NSFetchedResultsController` can reflect data changes in the UI in a timely manner.
    
- `container.viewContext.mergePolicy = NSMergeByPropertyObjectTrumpMergePolicy` sets the merge conflict resolution policy. If this property is not set, Core Data will default to using `NSErrorMergePolicy` as the conflict resolution strategy (all conflicts are not handled and an error is reported directly), which will cause iCloud data to not be correctly merged into the local database.
    

### Core Data Merge Policies

Core Data provides four preset merge conflict resolution strategies:

- **NSMergeByPropertyStoreTrumpMergePolicy**: Compares each attribute and if both the persistent and memory data have changed and conflict, the persistent data wins.
    
- **NSMergeByPropertyObjectTrumpMergePolicy**: Compares each attribute and if both the persistent and memory data have changed and conflict, the memory data wins.
    
- **NSOverwriteMergePolicy**: The memory data always wins.
    
- **NSRollbackMergePolicy**: The persistent data always wins.
    

For the use case of Core Data with CloudKit, `NSMergeByPropertyObjectTrumpMergePolicy` is usually selected.

`setQueryGenerationFrom(.current)` is a new addition to Apple's documentation and examples. Its purpose is to avoid unstable situations where data changes generated by the application during data import are inconsistent with imported data. Although I have rarely encountered this situation in my more than two years of use, I still recommend adding a context snapshot lock in the code to improve stability.

Until Xcode 13 beta4, Apple still has not added context settings to the built-in Core Data with CloudKit template, which causes unexpected behavior when importing data using the original template and is not very friendly to beginners.

### Check if Data Model meets synchronization requirements

The Data Model of the template project is very simple, with only one Entity and one Attribute, and no adjustments are required at present. The synchronization rules for the Data Model will be detailed in the next chapter.

### Modify ContentView.swift

**Reminder**: The generated `ContentView.swift` from the template is incomplete and needs to be modified before it can be displayed correctly.

```swift
var body: some View {
    NavigationView { // add NavigationView
        List {
            ForEach(items) { item in
                Text("Item at \(item.timestamp!, formatter: itemFormatter)")
            }
            .onDelete(perform: deleteItems)
        }
        .toolbar {
            HStack { // add HStack
                EditButton()
                Button(action: addItem) {
                    Label("Add Item", systemImage: "plus")
                }
            }
        }
    }
}
```

After modification, the toolbar buttons can be displayed normally.

We have now completed a project that supports Core Data with CloudKit.

### Run

Set up and log in to the same iCloud account on the simulator or device. Only the same account can access the same iCloud private database.

The following animation shows the running effect on a device (Airplay mirroring) and a simulator.

Operations (addition, deletion) performed on the simulator will usually be reflected on the device in about 15–20 seconds. However, operations performed on the device need to switch the simulator to the background and then return to the foreground to be reflected in the simulator (because the simulator does not support silent notification response). If testing is conducted between two simulators, similar operations need to be performed on both ends.

Apple documentation describes the synchronization and distribution time as not exceeding 1 minute, usually around 10–30 seconds in actual use. It supports batch data updates, so there is no need to worry about the efficiency of updating a large amount of data.

When data changes occur, there will be a lot of debugging information generated in the console, and there will be more articles related to debugging in the future.

## Notes on Creating Synchronizable Models

To transfer records seamlessly between Core Data and CloudKit databases, it is best to have some understanding of the data structure types of both sides. For more details, please refer to Part 1: The Basics.

The CloudKit Schema does not support all features and configurations of the Core Data Model. Therefore, when designing a synchronizable Core Data project, please note the following limitations and ensure that you create a compatible data model.

### Entities

**Unique constraints** in Core Data are not supported by CloudKit Schema.

Unique constraints in Core Data rely on SQLite support, but CloudKit is not a relational database and does not support them as expected.

```sql
CREATE UNIQUE INDEX Z_Movie_UNIQUE_color_colors ON ZMOVIE (ZCOLOR COLLATE BINARY ASC, ZCOLORS COLLATE BINARY ASC)
```

### Attributes

**Properties that are neither optional nor have default values are not allowed.**

Allowable formats:

- Optional
- With default values
- Optional + with default values

Properties that are non-optional and do not have default values in the above example are incompatible, and Xcode will report an error.

**Undefined type** is not supported.

### Relationships

All relationships must meet these requirements:

- **All relationships must be set to optional**
- **All relationships must have an inverse relationship**
- **Deny deletion rules are not supported**

CloudKit also has a relationship type of object similar to Core Data - **CKReference**. However, this object can only support up to 750 records, which cannot meet the needs of most Core Data application scenarios. CloudKit converts the relationships in Core Data into Record Name (UUID string) one by one, which may cause CloudKit not to save the relationship changes atomically. Therefore, there are stricter restrictions on relationship definitions.

In most Core Data scenarios, the majority of relationship definitions can still meet the above requirements.

### Configurations

**Entities cannot establish relationships with entities in other configurations.**

I'm a bit confused about this limitation in the official documentation, because even if network synchronization is not used, developers usually do not establish relationships between entities in two Configurations. If a relationship is needed, Fetched Properties are typically used.

If the Model does not meet the synchronization compatibility conditions after enabling CloudKit synchronization, Xcode will report an error to remind developers. When changing an existing project to support Core Data with CloudKit, some modifications may be necessary to the code.

## Adding Host in CloudKit Support to Existing Core Data Projects

With the foundation of a template project, upgrading a Core Data project to support Core Data with CloudKit is very easy:

- Replace NSPersistentContainer with NSPersistentCloudKitContainer
- Add CloudKit and background functionality, and add CloudKit container
- Configure the context

### Two Important Notes

#### CloudKit container cannot be authenticated

When adding a CloudKit container, sometimes authentication may fail, especially when adding a container that has already been created. This situation is almost inevitable.

```
CoreData: error: CoreData+CloudKit: -[NSCloudKitMirroringDelegate recoverFromPartialError:forStore:inMonitor:]block_invoke(1943): <NSCloudKitMirroringDelegate: 0x282430000>: Found unknown error as part of a partial failure: <CKError 0x28112d500: "Permission Failure" (10/2007); server message = "Invalid bundle ID for container"; uuid = ; container ID = "iCloud.Appname">
```

The solution is to log in to the developer account, go to **Certificates, Identifiers & Profiles**, select the corresponding BundleID, configure iCloud, click Edit, and reconfigure the container.

#### Using Custom NSPersistentStoreDescription

Some developers like to customize NSPersistentDescription (even if there is only one Configuration). In this case, `cloudKitContainerOptions` must be explicitly set for NSPersistentDescription, as follows:

```swift
let cloudStoreDescription = NSPersistentStoreDescription(url: cloudStoreLocation)
cloudStoreDescription.configuration = "Cloud"

cloudStoreDescription.cloudKitContainerOptions = NSPersistentCloudKitContainerOptions(containerIdentifier: "your.containerID")
```

Even if Configuration in Model Editor is not set to Used with CloudKit, network synchronization functions still work. The biggest advantage of checking Used with CloudKit is that Xcode will help you check if the Model is compatible with CloudKit.

## Selective Data Synchronization

In some scenarios, we may want to selectively synchronize data. By defining multiple configurations in the Data Model Editor, we can control the data synchronization.

Configuring a Configuration is straightforward - simply drag the desired Entity into it.

### Placing Different Entities in Different Configurations

Suppose we have an Entity called **Cache**, which is used as a local data cache and does not need to be synchronized to iCloud.

Apple's official documentation, as well as other resources discussing Configurations, are mainly aimed at scenarios like this.

We can create two Configurations:

- **local** - for Cache
- **cloud** - for other Entities that need to be synchronized

Using code similar to the following:

```swift
let cloudURL = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first!
    .appendingPathComponent("cloud.sqlite")
let localURL = FileManager.default.urls(for:.documentDirectory, in:.userDomainMask).first!
    .appendingPathComponent("local.sqlite")
```

```swift
let cloudDesc = NSPersistentStoreDescription(url: cloudURL)
cloudDesc.cloudKitContainerOptions = NSPersistentCloudKitContainerOptions(containerIdentifier: "your.cloudKit.container")
cloudDesc.configuration = "cloud"

let localDesc = NSPersistentStoreDescription(url: localURL)
localDesc.configuration = "local"

container.persistentStoreDescriptions = [cloudDesc,localDesc]
```

Only data from Entities in the cloud configuration will be synchronized to iCloud.

We cannot create relationship between Entity across configurations. If necessary, use Fetched Properties to achieve restricted similar effects.

### Placing the Same Entity in Different Configurations

If we want to control the synchronization of data from the same Entity (partial synchronization), we can use the following approach.

Suppose we have an Entity called **Movie**, and for some reason, we only want to synchronize some of its data.

#### Setup Steps:

1. Add a new Attribute to Movie called `local: Bool` (set to true for local data, false for synchronized data)
2. Create two Configurations: **cloud** and **local**, and add Movie to both configurations
3. Use the same code as above to add two Descriptions to the NSPersistentCloudKitContainer

When fetching Movie, NSPersistentCoordinator will automatically merge the Movie records in the two Stores. However, when writing Movie instances, the coordinator will only write the instance to the first Description containing Movie, so **the order of adding them is important**.

For example, if we have `container.persistentStoreDescriptions = [cloudDesc,localDesc]`, any new Movie created in `container.viewContext` will be written to `cloud.sqlite`.

#### Alternative Multi-Container Solution:

- Create a NSPersistentContainer named `localContainer`, which only contains `localDesc` (multi-container solution)
- Enable Persistent History Tracking on `localDesc`
- Use `localContainer` to create a context to write Movie instances (which will only be saved locally, not synchronized over the network)
- Handle `NSPersistentStoreRemoteChange` notifications to merge the data written from `localContainer` into container's `viewContext`.

The above approach requires the use of Persistent History Tracking. For more information, please refer to my other article "Using Persistent History Tracking in CoreData".

## Summary

In this article, we have explored how to synchronize a local database with an iCloud private database. We covered the essential setup steps, model design considerations, and advanced techniques for selective synchronization. Understanding these concepts is crucial for building robust Core Data with CloudKit applications that provide seamless data synchronization across devices.