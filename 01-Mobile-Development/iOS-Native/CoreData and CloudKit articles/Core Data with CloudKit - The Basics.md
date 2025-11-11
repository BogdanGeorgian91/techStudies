Source:
```
https://fatbobman.medium.com/core-data-with-cloudkit-part-1-basics-e3ddd6b47c47
```

# Core Data with CloudKit: The Basics

At WWDC 2019, Apple brought a major update to Core Data by introducing NSPersistentCloudKitContainer. This means that by using Core Data with CloudKit, users can seamlessly access the data in their application on all their Apple devices without having to write a lot of code.

Core Data provides powerful object graph management functionality for developing applications with structured data. CloudKit allows users to access their data on every device where they have logged into their iCloud account, while also providing a constantly available backup service. Core Data with CloudKit combines the advantages of local persistence, cloud backup, and network distribution.

In 2020 and 2021, Apple continued to enhance Core Data with CloudKit, adding functionality for syncing public databases and shared databases on top of the initial support for private database syncing.

## Table of Contents

- Part 1: The Basics
- Part 2: Syncing Local Database to iCloud Private Database
- Part 3: Exploring the CloudKit Dashboard
- Part 4: Troubleshooting
- Part 5: Synchronizing Public Database
- Part 6: Sharing Data in the iCloud

## Limitations of Core Data with CloudKit

### Only runs on Apple's ecosystem

Unlike other cross-platform solutions, Core Data with CloudKit can only run on Apple's ecosystem and can only provide services to users within the Apple ecosystem.

### High testing threshold

An Apple Developer Program account is required to access CloudKit services and the development team's CKContainer during the development process. In addition, the performance on the simulator is far less reliable than on a real device.

## Advantages of Core Data with CloudKit

### Almost free

Developers do not need to pay extra for network services. Private databases are stored in the user's personal iCloud space, and the capacity of public databases automatically increases as the application user base grows, with a maximum storage of 1 PB, 10 TB of database storage, and 200 TB of traffic per day. However, Apple does take a 15–30% cut of app revenue, so it's not completely free.

### Security

On one hand, Apple ensures user data security through sandbox containers, database isolation, encrypted fields, and authentication. On the other hand, given Apple's longstanding image as a privacy advocate among users, using Core Data with CloudKit can increase user trust in your application.

In fact, it was seeing this feature at WWDC 2019 that gave me the original inspiration to develop HealthNotes — to ensure data privacy and to be able to save data for a long time.

### High integration and good user experience

Authentication, distribution, and other processes are seamless, and users do not need to perform any additional logins to enjoy all the features.

## Core Data

Core Data was born in 2005, and its predecessor EOF had gained considerable recognition from users as early as 1994. After years of evolution, Core Data has become quite mature. As an object graph and persistence framework, almost every tutorial will tell you not to treat it as a database, nor as an ORM.

The functions provided by Core Data include, but are not limited to, managing serialized versions, managing object lifecycles, object graph management, SQL isolation, handling changes, persisting data, data memory optimization, and data querying.

Core Data provides a wide range of features, but it is not very friendly to beginners and has a steep learning curve. In recent years, Apple has also noticed this problem and greatly reduced the difficulty of creating Stack by adding PersistentContainer. The emergence of SwiftUI and Core Data templates also allows beginners to use its powerful functions more easily in projects.

## CloudKit

After Apple launched iCloud, developers were unable to integrate their applications with iCloud for several years. This problem was not resolved until Apple introduced the CloudKit framework in 2014.

CloudKit is a collection of services that includes databases, file storage, and user authentication systems, providing a way to move data between applications and iCloud containers. Users can access data saved on iCloud on multiple devices.

CloudKit has significant differences in data types, internal logic, and Core Data, which require some compromise or processing to convert data objects between the two. In fact, when CloudKit was first released, developers strongly hoped for an easy conversion between the two. Prior to the release of Core Data with CloudKit, third-party developers had already provided solutions for synchronizing Core Data or other data objects (such as realm) to CloudKit, and most of these solutions are still supported.

With the previous release of the persistent history tracking feature, Apple finally provided its own solution, Core Data with CloudKit, in 2019.

## Core Data objects vs CloudKit objects

Both frameworks have their own basic object types, which cannot be directly mapped to each other. Here, we will briefly introduce and compare some of the basic object types involved in this article:

### NSPersistentContainer vs CKContainer

**NSPersistentContainer** creates and manages the persistent store coordinator (NSPersistentStoreCoordinator) and managed object context (NSManagedObjectContext) through handling the managed object model (NSManagedObjectModel). Developers create its instance through code.

**CKContainer** is similar to the application's sandbox logic, where structured data, files, and other resources can be saved. Each application using CloudKit should have its own CKContainer (through configuration, an application can correspond to multiple CKContainers, and a CKContainer can also serve multiple applications). Developers usually do not create new CKContainer instances directly in their code, but rather through the iCloud console or by creating one in the Signing&Capabilities of the Xcode Target.

### NSPersistentStore vs CKDatabase/CkRecordZone

**NSPersistentStore** is the abstract base class for all Core Data persistent stores, supporting four types of persistence (SQLite, Binary, XML, and In-Memory). In an NSPersistentContainer, multiple NSPersistentStore instances (which can be of different types) can be held through declaring multiple NSPersistentStoreDescriptions. NSPersistentStore has no concept of user authentication, but can be set to read-only or read-write modes. As Core Data with CloudKit requires support for persistent history tracking, only NSPersistentStore instances with SQLite as the storage type can be synchronized, and on the device, the instance of this NSPersistentStore will point to a SQLite database file.

On CloudKit, structured data storage has only one type, but it is distinguished in two dimensions.

From the perspective of user authentication, **CKDatabase** provides three forms of databases: private, public, and shared. Application users (who have logged in to their iCloud account) can only access their own private database, which is saved in their personal iCloud space and cannot be operated on by others. Data saved in the public database can be called by any authorized application, even if the app user has not logged in to their iCloud account, and the application can still read its contents. Application users can share some data with other users of the same app, and the shared data will be placed in the shared database, and the sharer can set other users' read and write permissions for the data.

Data in CKDatabase is not scattered together, but is placed in specified RecordZones. We can create any number of Zones in the private database (public and shared databases only support default Zone). When a CKContainer is created, a CKRecordZone named `_defaultZone` is generated by default in each database.

Therefore, when we save data to a CloudKit database, we not only need to specify the database (private, public, shared) type, but also need to indicate the specific zoneID (no need to mark when saving to `_defaultZone`).

### NSManagedObjectModel vs Schema

**NSManagedObjectModel** is a model for managed objects, which represents the data entities in Core Data. In most cases, developers use Xcode's Data Model Editor to define the model, which is saved in a `.xcdatamodeld` file that contains information such as entity properties, relationships, indexes, constraints, validation, and configurations.

When CloudKit is enabled in an application, a **Schema** is created in the CKContainer. The Schema includes record types, relationships between record types, indexes, and user permissions.

In addition to creating the Schema content directly in the iCloud console, we can also create CKRecord in the code, which will automatically create or update the corresponding content in the Schema.

The Schema has security role settings that can be set for world, icloud, and creator to different read and write permissions.

### Entities vs Record Types

Although we usually emphasize that Core Data is not a database, entities are very similar to tables in a database. We describe objects in entities, including their names, properties, and relationships. Eventually, we describe them as NSEntityDescription and compile them into NSManagedObjectModel.

In CloudKit, we describe the name and properties of the data object using **Record Types**.

There is a lot of information that can be configured in Entity, but Record Types can only describe part of it. Since the two cannot be one-to-one correspondence, when designing data objects for Core Data with CloudKit, we need to follow the relevant rules (specific rules will be discussed in the next article).

### Managed Object vs CKRecord

A **managed object** is a model object that represents a persistent storage record. The managed object is an instance of NSManagedObject or its subclass, and is registered in the managed object context (NSManagedObjectContext). In any given context, the managed object corresponds to at most one instance of a given record in persistent storage.

In CloudKit, each record is called a **CKRecord**.

We do not need to worry about the creation process of the ID (NSMangedObjectID) of the managed object, as Core Data handles everything for us. However, for CKRecord, in most cases, we need to explicitly set the CKRecordIdentifier for each record in the code. As the unique identifier of the CKRecord, CKRecordIdentifier is used to determine the unique location of the CKRecord in the database. If the data is saved in a custom CKRecordZone, we also need to specify it in CKRecord.ID.

## CKSubscription

CloudKit is a cloud service that responds to data changes in private databases on different devices using the same iCloud account, or in public databases on devices using different iCloud accounts.

Developers create CKSubscriptions on iCloud using CloudKit. When there is a change in data in CKContainer, the server checks whether the change satisfies the trigger condition of a CKSubscription. When the condition is met, a remote notification is sent to the subscribed devices. This is why when we add the CloudKit feature in the Signing & Capabilities of the Xcode Target, Xcode automatically adds Remote Notification.

In practical use, three subclasses of CKSubscription are used for different subscription tasks:

- **CKQuerySubscription** pushes a notification when a CKRecord meets a set NSPredicate.
- **CKDatabaseSubscription** subscribes and tracks the creation, modification, and deletion of records in a database (CKDatabase).

This subscription can only be used for private databases and shared databases' custom CKRecordZones and only notifies the subscription creator. In future articles, we will see how Core Data with CloudKit uses this subscription in a private library.

**CKRecordZoneNotification** executes when the user or, in some cases, CloudKit modifies the record in the record zone (CKRecordZone). For example, when the value of a field in the record changes.

For the remote notifications pushed by the iCloud server, the application needs to respond in the Application Delegate. In most cases, the remote notification can be in the form of a silent notification. Therefore, developers need to enable Background Modes' Remote notifications in their applications.

## Guess on the Implementation of Core Data with CloudKit

Combining the basic knowledge introduced above, let us try to speculate on the implementation process of Core Data with CloudKit using private database synchronization as an example:

### Initialization:

1. Create CKContainer.
2. Configure Schema based on NSManagedObjectModel.
3. Create a CKRecordZone in the private database with the ID `com.apple.coredata.cloudkit.zone`.
4. Create a CKDatabaseSubscription on the private database.

### Data export (export local Core Data data to the cloud):

1. NSPersistentCloudKitContainer creates a background task that responds to the NSPersistentStoreRemoteChange notification of persistence history tracking.
2. Based on the transaction of NSPersistentStoreRemoteChange, convert Core Data operations into CloudKit operations. For example, for newly added data, convert NSManagedObject instances to CKRecord instances.
3. Pass the converted CKRecord or other CloudKit operations to the iCloud server through CloudKit.

### Server-side:

1. Process CloudKit operation data submitted by remote devices in order.
2. Check whether the operation causes data changes in the `com.apple.coredata.cloudkit.zone` of the private database according to the CKDatabaseSubscription created during initialization.
3. Distribute remote notifications to all devices (same iCloud account) that subscribed to CKDatabaseSubscription.

### Data Import (Syncing Remote Data to Local)

1. Use NSPersistentCloudKitContainer to create a background task that responds to the Cloud's silent push notifications.
2. Send a refresh request to the Cloud with the token of the last operation.
3. Based on the token of each device, the Cloud returns the changes made to the database since the last refresh.
4. Convert the remote data into local data by deleting, updating, and adding items, etc.
5. Since the automaticallyMergesChangesFromParent property of the view context is set to true, the changes in local data will automatically reflect in the view context.

_The above steps omit all technical difficulties and details, and only describe the general process._