source:
```
https://fatbobman.medium.com/core-data-with-cloudkit-part-5-synchronizing-public-database-a7354d716ffc
```

This article will explain how to synchronize the public database to the local device using Core Data with CloudKit, and create a mirror of the Core Data database locally.

[_Part 1:_ The Basics](https://fatbobman.medium.com/core-data-with-cloudkit-part-1-basics-e3ddd6b47c47)
[_Part 2: Syncing Local Database to iCloud Private Database_](https://fatbobman.medium.com/core-data-with-cloudkit-part-2-syncing-local-database-to-icloud-private-database-fd80581ab360)
[_Part3:_ Exploring the CloudKit Dashboard](https://fatbobman.medium.com/core-data-with-cloudkit-part-iii-cloudkit-dashboard-cd6f1c8045c0)
[_Part4:_ Troubleshooting](https://fatbobman.medium.com/core-data-with-cloudkit-part-4-debugging-testing-migration-and-other-issues-8b525fca0e8f)
[_Part5: Synchronizing Public Database_](https://fatbobman.medium.com/core-data-with-cloudkit-part-5-synchronizing-public-database-a7354d716ffc)
[_Part6:_ Sharing Data in the iCloud](https://fatbobman.medium.com/core-data-with-cloudkit-part-6-sharing-data-cc483046de7b)

There are three types of databases in CloudKit:

## Public Database

The public database stores data that developers want anyone to be able to access. Custom zones cannot be added to the public database, and all data is saved in the default zone. Data in the public database can be accessed through the application or the CloudKit web service, regardless of whether the user has an iCloud account. The contents of the public database are visible on the CloudKit dashboard.

The data capacity of the public database counts towards the application’s CloudKit storage quota.

## Private Database

This is where iCloud users store personal data that they do not want the public to see. Users save content they do not want to share publicly through the application here. Only users who are logged in to their iCloud accounts can access the data. By default, only the user can access the contents of their private database (some content can be shared with other iCloud users). The user has full control over the data (creation, viewing, modification, deletion). The contents of the private database are not visible on the CloudKit dashboard and are kept completely confidential from developers.

Developers can create custom zones in the private database to organize and manage data.

==The data capacity of the private database counts towards the user’s iCloud storage quota.==

## Shared Database

The data that iCloud users see in the shared database is a projection of data that other iCloud users have shared with them. This data is still saved in each user’s private database. Users do not own this data and can only view and modify the content if they have the necessary permissions. This database is only available when the user is logged in to their iCloud account.

For example, if you share a record with another user, that record is still saved in your private database. However, the person you shared it with can see the record in their shared database, and can only perform operations based on the permissions you set.

Custom zones cannot be added to the shared database. The data in the shared database is not visible on the CloudKit dashboard.

The data capacity of the shared database counts towards the application’s CloudKit storage quota.

## Same noun, different meanings

In [_Syncing Local Database to iCloud Private Database_](https://fatbobman.com/en/posts/coredatawithcloudkit-2/), we discussed how to synchronize a local database to an `iCloud` private database. In this article, we will talk about how to synchronize a shared database to a local one. Although both articles are about synchronization, the intrinsic meanings and logics of the two types of synchronization are different.

Syncing local data to a private database is essentially a standard `Core Data`project. From model design to code development, developers do not differ from developing projects that only support local persistent databases. `CloudKit` simply acts as a bridge to synchronize data to the user's other devices. In most cases, developers can completely ignore the existence of a private database and `CKRecord` when using managed objects.

Syncing a public database to a local one is completely different. The public database is a concept of a network database. The standard logic is that developers create `Record Type` on the `CloudKit` dashboard, add `CKRecord`records to the public database via the dashboard or client, and clients access the network data records by accessing the server. `Core Data with CloudKit`makes it easy for us to use existing `Core Data` knowledge to complete this process. The data synchronized to the local is a mirror of the server-side public database, and indirect manipulation of the server-side `CKRecord`records is achieved by operating on the data of managed objects locally.

> _Authentication discussed later checks records or databases on the network, even though the object being operated on is a managed object or local persistent storage._

## Public Database vs Private Database

Let’s compare public and private databases from several perspectives.

## Authentication

In the case of not considering data sharing, the data in the private database can only be accessed by the user (who has logged into the `iCloud` account) who created it. The user, as the creator of the data, has all the operation permissions. The authentication rules for private databases are very simple:

![[Screenshot 2025-09-03 at 17.07.00.png]]
In the article [iCloud Dashboard](https://fatbobman.com/en/posts/coredatawithcloudkit-3/), we introduced the concept of security roles. The system creates three preset roles for the public database: `World`, `Authenticated`, and `Creator`. In the public database, authentication needs to consider multiple factors such as whether the user has logged into the `iCloud` account and whether the user is the creator of the data record.

![[Screenshot 2025-09-03 at 17.07.46.png]]

- Each user can read records (regardless of whether they are logged in)
- Each logged-in user can create records
- Logged-in users can only modify or delete records they create

In addition to the relatively long authentication time (because it needs to access the server every time to get the result), judging permissions through the standard `CloudKit API` requires a large amount of code. `Core Data with CloudKit` perfectly solves the authentication efficiency problem by backing up the metadata of `CKRecord` locally and providing convenient `APIs` for developers to call.

We can determine whether a user has permission to modify or delete the current managed object (`ManagedObject`) by using code similar to the following:

let container = PersistenceController.shared.container  
  
if container.canUpdateRecord(forManagedObjectWith:item.objectID) {  
    // update or delete itme  
}

In the past two years, Apple has continuously improved the `NSPersistentCloudKitContainer` and added many important methods to it. These methods can be used not only for public databases or managed objects but also for other types of databases or data (private databases, local databases, shared data, etc.).

### `canUpdateRecord` and `canDeleteRecord`

Get whether the user has permission to modify data. Returns `true` in the following cases:

1. The `objectID` is a temporary object identifier (meaning it has not yet been persisted).
2. The persistence store containing the managed object does not use `CloudKit` (i.e., it is a local database that is not used for synchronization).
3. The persistence store manages a private database (the user has full permissions for the private database).
4. The persistence store manages a public database, and the user is the creator of the record or `Core Data` has not yet updated the managed object to `iCloud`.
5. The persistence store manages a shared database, and the user has permission to modify the data.

_In practice, the results of_ `_canDeleteRecord_` _are not accurate. It is currently recommended that you only use_ `_canUpdateRecord_`_._

`canUpdateRecord` returning `false` does not mean that you cannot delete data from local storage, only that you do not have permission to modify the network record corresponding to the managed object.

### `canModifyMangedObject(in:NSPersistentStore)`

Indicates whether a specific persistence store can be modified.

Use this method to determine if the user can write records to the `CloudKit`database. For example, when the user is not logged into their iCloud account, they cannot write to the persistence store that manages the public database.

Similarly, `canModifyManagedObjects` returning `false` does not mean that you cannot write data to the local `sqlite` file, only that you do not have permission to modify the network storage corresponding to the persistence store.

> _Since local data and persistence stores do not have permission concepts, developers may inadvertently write incorrect code that operates on the data locally without the necessary network permissions. This is particularly dangerous in projects that synchronize public or shared databases. If you modify or delete a data record without network permissions, the network will reject your request, and_ `_Core Data with CloudKit_` _will stop all subsequent synchronization work after receiving the rejection. Therefore, when developing projects that synchronize public or shared databases, you must ensure that you have the corresponding permissions before operating on the data._

## Synchronization Mechanism

From the perspective of `export` (synchronizing local data changes to the server), whether it is synchronizing private databases or public databases, the behavior is the same. `Core Data with CloudKit` immediately synchronizes changes to the server after local data changes. It is an instant one-way action.

From the perspective of `import` (synchronizing network data changes to local), the mechanisms of private databases and public databases are completely different.

In the articles on [The Basics](https://fatbobman.medium.com/core-data-with-cloudkit-part-1-basics-e3ddd6b47c47) and [Exploring the CloudKit Dashboard](https://fatbobman.medium.com/core-data-with-cloudkit-part-iii-cloudkit-dashboard-cd6f1c8045c0), we have introduced the synchronization mechanism of private databases:

- The client subscribes to `CKDatabaseSubscription` on the server.
- After the content of the custom `Zone` in the private database changes, the server sends a silent remote notification to the client.
- After receiving the notification, the client requests change data from the server through `CKFetchRecordZoneChangesOperation`.
- After comparing tokens, the server synchronizes the changed data with the client.

The whole process involves interaction between two parties.

Due to some technical limitations of public databases, the above mechanism cannot be applied to public database synchronization.

- Public databases cannot customize `Zone`.
- Without custom `Zone`, `CKDatabaseSubscription` cannot be subscribed.
- `CKFetchrecordZoneChangesOperation` utilizes proprietary technology of private databases, and public databases can only use `CKQureyOperation`.
- Public databases do not have a tombstone mechanism and cannot record all user operations (deletions).

Due to the above reasons, `Core Data with CloudKit` can only use polling (`poll for changes`) to obtain change data from public databases.

When the application starts or every 30 minutes of operation, `NSPersistentCloudKitContainer` queries the changes in the public database and retrieves data through `CKQurey` operation. The `import` process is initiated by the client and responded by the server.

This synchronization mechanism will limit the applicable scenarios. Only data with low immediacy is suitable for storing in public databases.

## Data Model

When designing a data model for a public database, several factors need to be considered due to different synchronization mechanisms:

### Complexity

The public database uses `CKQueryOperation` to query server-side changes since the last update, which is less efficient than `CKFetchRecordZoneChangesOperation`. If we can control the number of entities and attributes in `ManagedObjectModel`, the fewer requests we need to make, the more efficient the execution. Unless necessary, it is recommended to reduce the complexity of the data model as much as possible.

### Tombstones

When a private database receives a record deletion operation from a client, it immediately deletes the corresponding server-side record and saves the tombstone flag of the deletion operation. When other client devices obtain changes through `CKFetchRecordZoneChangesOperation`, the private database sends the change records (including tombstones) to the client. The client deletes the corresponding local data records based on the tombstone instructions to ensure data consistency.

The public database also immediately deletes the corresponding server-side record after receiving a record deletion operation. However, due to the lack of a tombstone mechanism, when other clients query the database for changes, the public database only informs them of the added or modified records, but cannot notify them of the deletion operation. This means that we cannot pass on the deletion operation from one device to another, and the local mirrors of the public database on the two devices will differ.

To avoid such differences as much as possible, we can add a tombstone-like attribute (such as `isDeleted`) when designing the data model of the public database.

```
// When deleting, set isDelete to true  
if container.canUpdateRecord(forManagedObjectWith:item.objectID){  
    item.isDeleted = true  
    try! viewContext.save()  
}
```

When fetching data, only retrieve records with `isDeleted` set to `false`.

```
@FetchRequest(  
        sortDescriptors: [NSSortDescriptor(keyPath: \Item.timestamp, ascending: true)],  
        predicate: NSPredicate(format: "%K = false", #keyPath(Item.isDelete)),  
        animation: .default  
)  
private var items: FetchedResults<Item>
```

> _The records are not actually deleted but only hidden. The public database can pass on record modification operations between devices, ensuring data consistency and achieving “deletion” of data. The “deleted” data still occupies space on the local and server-side, so it is important to carefully choose when to clear the space._

## Storage Quota

The data of private databases is stored in the user’s personal `iCloud` space, which occupies the capacity quota of their personal space. If the user's `iCloud` space is full, data will no longer be able to sync between devices via the network. Users can solve this problem by cleaning up their personal space or choosing a larger space plan.

The data capacity of public databases occupies the space quota of your application. Apple provides a basic space capacity for each app that supports `CloudKit`, with the following limitations: 10GB of `Asset` storage, 100MB of database, 2GB of data transfer per month, and 40 query requests per second. Space, traffic, and request numbers will increase based on the number of active users of your application (using the app within the past 16 months), and can increase up to 10PB, 10TB, and 200TB per day, respectively.

Although most applications will not exceed these limits, as a developer, it is still advisable to minimize the use of space and improve data response efficiency.

`Core Data with CloudKit` synchronizes the public database by saving a local mirror of the entire public database. Therefore, if data volume cannot be controlled well, the application's occupation of user devices will be terrifying. The "deletion" method mentioned earlier will further encroach on network and device space.

Developers should consider the timing of clearing pseudo-”deleted” data at the beginning of project design.

We cannot guarantee that clearing will occur after all clients have synchronized the “deleted” state. Under the condition that it does not affect the business logic of the application, it is acceptable to allow inconsistent data between devices.

Developers can clear “deleted” data on the client side based on the average usage frequency of the application. Although `Core Data with CloudKit` saves metadata for managed objects corresponding to `CKRecord` locally, it does not provide an API for developers. For easy deletion, we can add a "deletion" time attribute to the model, which works with the query during clearing.

## Suitable Scenarios for Public Databases

The technical characteristics and considerations of calling public databases through `CloudKit` and synchronizing public databases through `Core Data with CloudKit` are different.

Personally, I recommend the following scenarios for synchronizing public databases through `Core Data with CloudKit`:

### Read-only

For example, providing templates, initial data, and news alerts.

The creation, modification, and deletion of public database data are all performed by developers through the dashboard or specific applications. The user’s application only reads the content of the public database, without creating or modifying it.

### Only process one record

The application only creates one data associated with the user or device and only updates the content of that record.

Typically used for recording and device-related status or user (associate-able) status or data. For example, game high score rankings (only saves the user’s highest score).

### Create only, no modification

Log-type scenarios. Users are responsible for creating data and do not particularly rely on the data itself. The application regularly clears expired data locally. Query or backup the public database records through `CloudKit Web` services or other specific applications and regularly clear them.

> _When considering using_ `_Core Data with CloudKit_` _to synchronize public database data, developers must carefully consider the pros and cons of all parties and choose the appropriate application scenario._

## Synchronize Public Database

This section covers a lot of knowledge from [_Syncing Local Database to iCloud Private Database_](https://fatbobman.com/en/posts/coredatawithcloudkit-2/) and [Exploring the CloudKit Dashboard](https://fatbobman.com/en/posts/coredatawithcloudkit-3/). Please read the above two articles before continuing.

## Project Configuration

Configuring the public database in the project is almost identical to configuring the private database.

- Add `iCloud` in `Signing & Capabilities` of the project `Target`.
- Choose `CloudKit` and add `Container`.

_If you only use the public database in the project, you can skip adding the_ `_Remote Notifications_` _feature in_ `_Background Mode_`_._

## Create Local Mirror using NSPersistentCloudKitContainer

- Create a new `Configuration` in `Xcode Data Model Editor` and add the entities you want to expose (`Entity`) to this new configuration.
- Add the following code to your `Core Data Stack` (e.g., `Persistenc.swift` in template project):

```
let publicURL = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first!.appendingPathComponent("public.sqlite")  
let publicDesc = NSPersistentStoreDescription(url: publicURL)  
publicDesc.configuration = "public" //Configuration 名称  
publicDesc.cloudKitContainerOptions = NSPersistentCloudKitContainerOptions(containerIdentifier: "your.public.containerID")  
publicDesc.cloudKitContainerOptions?.databaseScope = .public
```

Familiar with the code? That’s right. In fact, synchronizing the public database only requires one additional line of code compared to synchronizing the private database:

```
publicDesc.cloudKitContainerOptions?.databaseScope = .public
```

> `_databaseScope_` _is a property newly added by Apple to_ `_cloudKitContainerOptions_` _in 2020. The default value is_ `_.private_`_, so it does not need to be set when synchronizing the private database._

That’s it?

Yes, that’s it. Other configurations are the same as synchronizing the private database. Add `Description` to `persistentStoreDescriptions`, configure the context, and configure [Persistent History Tracking](https://www.fatbobman.com/posts/persistentHistoryTracking/) if needed.

## Configure the Dashboard

Because `NSPersistentCloudKitContainer` uses a different way to obtain public data (`CKQuery`) from obtaining private data (`CKFetchRecordZoneChangesOperation`), we need to make certain modifications to the `Schema` in the `CloudKit` dashboard to ensure the normal operation of the program.

In the `CloudKit` dashboard, select `Indexes` and add two indexes for each `Record Type` used for the public database:

![[Screenshot 2025-09-03 at 17.08.35.png]]

_At the time of writing this article, when I built the demo project using_ `_Xcode 13 beta5_`_, I found that I needed to add one more index to synchronize the public database properly. If you are using_ `_Xcode 13_`_, please add an additional index_ `_Sortable_` _to the dashboard._

## Others

## Initializing Schema

Following the steps in the previous section, when you try to add an index in the `CloudKit` dashboard, you may notice that there is no `Record Type`available for you to add the index to. This is because we have not initialized the `Schema` on the server-side yet.

There are two ways to initialize the schema on the server-side:

### Create a managed object data and synchronize it to the server

The server will automatically create a corresponding `Record Type` if it does not exist after receiving the data.

### Use `initializeCloudKitSchema`

`initializeCloudKitSchema` allows us to initialize the `Schema` on the server-side without creating any data. Add the following code to your `Core Data Stack`:

```
try! container.initializeCloudKitSchema(options: .printSchema)
```

After running the project, you should be able to see the corresponding `Record Type` in the dashboard.

This code only needs to be executed once. Remember to remove or comment it out after initialization.

We can also use `initializeCloudKitSchema` in unit tests to verify if the `Model`meets the compatibility requirements of the sync model.

```
let result = try! container.initializeCloudKitSchema(options: .dryRun)
```

`result` will be true if the compatibility requirements are met. `.dryRun`means that the check is only performed locally and the schema is not actually initialized on the server-side.

## Multiple Containers and Configurations

As mentioned in previous articles, you can associate multiple `CloudKit`containers in a project, and one container can correspond to multiple applications.

If your project uses both private and public databases, and the two containers are not the same, you need to set the correct `ContainerID` for `Description` in your code, in addition to associating both containers in your project.

```
let publicDesc = NSPersistentStoreDescription(url: publicURL)  
publicDesc.configuration = "public"  
publicDesc.cloudKitContainerOptions = NSPersistentCloudKitContainerOptions(containerIdentifier: "public.container")  
publicDesc.cloudKitContainerOptions?.databaseScope = .public  
  
let privateDesc = NSPersistentStoreDescription(url: privateURL)  
privateDesc.configuration = "private"  
privateDesc.cloudKitContainerOptions = NSPersistentCloudKitContainerOptions(containerIdentifier: "private.container")
```

The `URL` of the public database's `NSPersistentStoreDescription` must be different from the URL of the private database (i.e., two separate `sqlite` files must be created), as the coordinator cannot load the same URL multiple times.

```
let publicURL = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first!.appendingPathComponent("public.sqlite")  
  
let privateURL = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first!.appendingPathComponent("private.sqlite")
```

## Xcode 13 Beta

It seems that `Xcode 13 Beta` has made some unannounced adjustments to the `CloudKit` module. Using `Core Data with CloudKit` in `Xcode 13 Beta5` may result in many strange warnings. For now, it is recommended to use `Xcode 12` for testing the code in this article.

## Summary

The implementation of syncing local data to a private database and syncing to a public database may seem very similar in code, but as a developer, don’t be deceived by this illusion. It’s important to recognize the essence of the synchronization mechanism in order to better design data models and plan business logic.

Keep in mind that while the implementation may seem similar, there are unique considerations to keep in mind when dealing with shared data. As always, understanding the underlying mechanisms is key to success. Happy syncing!