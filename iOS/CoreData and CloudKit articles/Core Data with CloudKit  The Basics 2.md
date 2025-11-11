
Published on Aug 5, 2021

At WWDC 2019, Apple introduced a significant update to `Core Data` by introducing the `NSPersistentCloudKitContainer`. This means that with `Core Data with CloudKit`, users can seamlessly access data in their applications across all their Apple devices without writing extensive code.

`Core Data` provides powerful object graph management capabilities for developing applications with structured data. CloudKit allows users to access their data on each device where they are logged into their iCloud account, also offering a consistently available backup service. `Core Data with CloudKit` combines the advantages of local persistence with cloud backup and network distribution.

In 2020 and 2021, Apple continued to enhance `Core Data with CloudKit`, adding public and shared database synchronization to its initial support for private database synchronization.

I will introduce the usage, debugging techniques, console settings of `Core Data with CloudKit`, and try to delve deeper into its synchronization mechanisms through several blog posts.

## Limitations of Core Data with CloudKit

-   **Apple Ecosystem Only**
    
    Unlike other cross-platform solutions, `Core Data with CloudKit` can only operate within the Apple ecosystem and is only available to users in this ecosystem.
    
-   **High Testing Threshold**
    
    Access to the `CloudKit` service and the development team’s `CKContainer` requires an [Apple Developer Program](https://developer.apple.com/programs/?utm_source=Fatbobman%20Blog&utm_medium=web) account during development. Additionally, the performance on simulators is not as reliable as on actual devices.
    

## Advantages of Core Data with CloudKit

-   **Almost Free**
    
    Developers generally do not have to pay extra for network services. Private databases are stored in the user’s personal iCloud space, and the capacity of public databases automatically increases with the number of app users, up to 1 PB of storage, 10 TB of database storage, and 200 TB of daily traffic. It’s almost free, considering Apple takes a 15-30% cut of app revenues.
    
-   **Secure**
    
    Apple ensures data security through various technical means such as sandbox containers, database segregation, encrypted fields, and authentication. Additionally, given Apple’s long-standing image as a defender of user privacy, using `Core Data with CloudKit` can increase users’ trust in your application.
    
    In fact, it was after seeing this feature at WWDC 2019 that I was inspired to develop [Health Notes](/healthnotes/) — ensuring data privacy while keeping data for a long time.
    
-   **High Integration, Good User Perception**
    
    Authentication and distribution are seamless. Users can enjoy all the features without any extra login.
    

## Core Data

`Core Data` was introduced in 2005, with its predecessor `EOF` gaining considerable user recognition as early as 1994. After years of evolution, `Core Data` has become quite mature. As an object graph and persistence framework, almost every tutorial will tell you not to treat it as a database or an `ORM`.

`Core Data`’s features include, but are not limited to, managing serialized versions, object life cycle management, object graph management, SQL isolation, change handling, data persistence, memory optimization, and data querying.

`Core Data` offers many features, but it is not very beginner-friendly and has a steep learning curve. In recent years, Apple has addressed this issue by adding `PersistentContainer` to greatly reduce the difficulty of creating `Stacks`; the emergence of `SwiftUI` and `Core Data Template` has made it easier for beginners to use its powerful features in projects.

## CloudKit

For several years after Apple launched `iCloud`, developers were unable to integrate their applications with `iCloud`. This problem was resolved in 2014 with the introduction of the `CloudKit` framework.

`CloudKit` is a collective service of databases, file storage, and user authentication systems, providing a mobile data interface between applications and `iCloud containers`. Users can access data saved in `iCloud` on multiple devices.

The data types and internal logic of `CloudKit` differ greatly from `Core Data`, and compromises or processing are required to convert data objects between the two. In fact, as soon as `CloudKit` was released, developers strongly hoped for a convenient conversion between the two. Before the introduction of `Core Data with CloudKit`, third-party developers had already provided solutions to synchronize `Core Data` or other data objects (like `realm`) to `CloudKit`, most of which are still supported today.

Relying on the previously introduced [Persistent History Tracking](/en/posts/persistenthistorytracking/) feature, Apple finally provided its own solution, `Core Data with CloudKit`, in 2019.

## Core Data Objects vs. CloudKit Objects

Both frameworks have their own fundamental object types that are not directly interchangeable. Here’s a brief introduction and comparison of some basic object types mentioned in this article:

-   **NSPersistentContainer vs. CKContainer**
    
    `NSPersistentContainer` handles the managed object model (`NSManagedObjectModel`), creating and managing the persistence coordinator (`NSPersistentStoreCoordinator`) and managed object context (`NSManagedObjectContext`). Developers create its instance through code.
    
    `CKContainer` is similar to an app’s sandbox logic, where you can store various resources like structured data and files. Each app using `CloudKit` should have its own `CKContainer` (though one app can correspond to multiple `CKContainers` and vice versa). Developers usually don’t create new `CKContainers` directly in the code but rather through the `iCloud console` or in the `Xcode Target`’s `Signing & Capabilities`.
    
-   **NSPersistentStore vs. CKDatabase/CKRecordZone**
    
    `NSPersistentStore` is the abstract base class for all `Core Data` persistence storage, supporting four types of persistence (SQLite, Binary, XML, and In-Memory). Multiple `NSPersistentStore` instances (possibly of different types) can be held in an `NSPersistentContainer` by declaring multiple `NSPersistentStoreDescriptions`. `NSPersistentStore` doesn’t have a user authentication concept but can be set to read-only or read-write modes. Due to the [Persistent History Tracking](/en/posts/persistenthistorytracking/) requirement of `Core Data with CloudKit`, only `SQLite` type `NSPersistentStores` can be synchronized, where each instance on the device points to a SQLite database file.
    
    In `CloudKit`, there is only one type of structured data storage, but it’s distinguished in **two dimensions**.
    
    From a user authentication perspective, `CKDatabase` provides three forms of databases: private, public, and shared. Only the app’s user (who has logged into their iCloud account) can access their private database, which is stored in the user’s personal iCloud space, and no one else can operate on its data. Data stored in the public database can be accessed by any authorized app, even if the app’s user hasn’t logged into an iCloud account. App users can share some data with other users of the same app, which is placed in the shared database, where the sharer can set read-write permissions for other users.
    
    Data in `CKDatabase` is also not scattered; it’s placed in designated `RecordZones`. We can create any number of `Zones` in the private database (public and shared databases only support the default `Zone`). When a `CKContainer` is created, each type of database will automatically generate a `CKRecordZone` named `_defaultZone`.
    
    Therefore, when we save data to a CloudKit database, we need to specify not only the database type (private, public, shared) but also the specific `zoneID` (which is unnecessary when saving to `_defaultZone`).
    
-   **NSManagedObjectModel vs. Schema**
    
    `NSManagedObjectModel` is the managed object model, representing the `Core Data` corresponding data entities. Most developers define it using `Xcode`’s `Data Model Editor`, which is saved in the `xcdatamodeled` file, containing entity properties, relationships, indexes, constraints, validations, configurations, etc.
    
    When `CloudKit` is enabled in an app, a `Schema` is created in the `CKContainer`. The `Schema` includes record types (`Record Type`), possible relationships between record types, indexes, and user permissions.
    
    Besides directly creating `Schema` content in the `iCloud` console, you can also create `CKRecord` in the code, letting `CloudKit` automatically create or update corresponding content in the `Schema`.
    
    `Schema` has permission settings (`Security Roles`), which can set different read-write permissions for `world`, `icloud`, and `creator`.
    
-   **Entities vs. Record Types**
    
    Although we often emphasize that `Core Data` is not a database, entities (`Entities`) are very similar to tables in a database. We describe objects in entities, including their names, properties, and relationships, eventually forming them into `NSEntityDescription` and summarizing them in `NSManagedObjectModel`.
    
    In `CloudKit`, data objects’ names and properties are described using `Record Types`.
    
    `Entity` has a lot of configurable information, but `Record Types` can only correspond to describing part of it. Since the two cannot correspond one-to-one, there are specific regulations to follow when designing `Core Data with CloudKit` data objects (which we will explore in the next article).
    
-   **Managed Object vs. CKRecord**
    
    Managed Objects (`Managed Object`) are model objects representing persistent storage records. They are instances of `NSManagedObject` or its subclasses and are registered in the
    
    managed object context (`NSManagedObjectContext`). In any given context, there is at most one managed object instance corresponding to a given record in persistent storage.
    
    In `CloudKit`, each record is called a `CKRecord`.
    
    We don’t need to worry about the creation process of `Managed Object` IDs (`NSManagedObjectID`), as `Core Data` handles everything, but for `CKRecord`, we usually need to explicitly set a `CKRecordIdentifier` for each record in the code. As the unique identifier of `CKRecord`, `CKRecordIdentifier` is used to determine the unique location of that `CKRecord` in the database. If the data is stored in a custom `CKRecordZone`, we also need to indicate it in `CKRecord.ID`.
    
-   **CKSubscription**
    
    `CloudKit` is a cloud service that must respond to data changes in different devices of the same `iCloud` account (private database) or devices using different `iCloud` accounts (public database).
    
    Developers create `CKSubscription` on `iCloud` through `CloudKit`. When data in `CKContainer` changes, the cloud server checks whether the change meets any `CKSubscription`’s trigger condition. If so, it sends a remote notification (`Remote Notification`) to the subscribed devices. This is why `Xcode` automatically adds `Remote Notification` when `CloudKit` functionality is added in the `Xcode Target`’s `Signing & Capabilities`.
    
    In practice, three subclasses of `CKSubscription` are used to perform different subscription tasks:
    
    `CKQuerySubscription`, which pushes a `Notification` when a `CKRecord` meets the set `NSPredicate`.
    
    `CKDatabaseSubscription`, which subscribes to and tracks the creation, modification, and deletion of records in the database (`CKDatabase`). This subscription can only be used in custom `CKRecordZones` in private and shared databases and will only notify the `subscription creator`. Future articles will show how `Core Data with CloudKit` uses this subscription in private databases.
    
    `CKRecordZoneNotification`, which executes when a user, or in some cases, `CloudKit`, modifies records in that zone (`CKRecordZone`), for example, when a field’s value in a record changes.
    
    For remote notifications pushed by the `iCloud` server, applications need to respond in the `Application Delegate`. In most cases, `remote notifications` can be in the form of `silent notifications`, and for this, developers need to enable `Backgroud Modes`’ `Remote notifications` in their application.
    

## Implementation Speculation of Core Data with CloudKit

Let’s speculate on the implementation process of `Core Data with CloudKit`, combining the basic knowledge introduced above.

Taking private database synchronization as an example:

-   Initialization:
    
    1.  Create `CKContainer`
    2.  Configure `Schema` based on `NSManagedObjectModel`
    3.  Create a `CKRecordZone` with ID `com.apple.coredata.cloudkit.zone` in the private database
    4.  Create `CKDatabaseSubscription` on the private database
-   Data Export (exporting local `Core Data` data to the cloud):
    
    1.  `NSPersistentCloudKitContainer` creates background tasks to respond to `Persistent History Tracking`’s `NSPersistentStoreRemoteChange` notifications
    2.  Based on the `transaction` from `NSPersistentStoreRemoteChange`, convert `Core Data` operations into `CloudKit` operations, such as converting `NSManagedObject` instances into `CKRecord` instances for new data.
    3.  Pass the converted `CKRecord` or other `CloudKit operations` to the `iCloud` server through `CloudKit`
-   Server-side:
    
    1.  Process `CloudKit operation data` submitted from remote devices in order
    2.  Check with the `CKDatabaseSubscription` created during initialization if the operation causes changes in data in the `com.apple.coredata.cloudkit.zone` of the private database
    3.  Distribute remote notifications to all devices (same `iCloud` account) that created `CKDatabaseSubscription`
-   Data Import (synchronizing remote data locally):
    
    1.  `NSPersistentCloudKitContainer`’s background tasks respond to cloud `silent push`
    2.  Send a refresh operation request to the cloud with the last operation `token`
    3.  The cloud returns changes in the database since the last refresh for each device based on their `token`
    4.  Convert remote data into local data (delete, update, add, etc.)
    5.  Since the `view context`’s `automaticallyMergesChangesFromParent` property is true, local data changes will automatically be reflected in the `view context`

The above steps omit all technical difficulties and details, only describing the general process.

## Conclusion

This article briefly introduced some basic knowledge about `Core Data`, `CloudKit`, and `Core Data with CloudKit`. In the next article, we will explore how to use `Core Data with CloudKit` to implement synchronization between local and private databases.

PS: There are many articles on how to use NSPersistentContainer, but like other Core Data features, it’s not easy to use effectively. I’ve encountered numerous issues over more than two years of use. Recently, I have systematically relearned `Core Data with CloudKit` and organized its key points. I hope this series of articles will help more developers understand and use the `Core Data with CloudKit` feature.


