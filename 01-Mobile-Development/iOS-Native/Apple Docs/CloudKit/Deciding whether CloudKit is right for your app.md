# Deciding whether CloudKit is right for your app

Explore the various options you have for using iCloud to store and sync your app’s data.

## [Overview](/documentation/cloudkit/deciding-whether-cloudkit-is-right-for-your-app#overview)

Use iCloud to store your app’s data and make that data available on the web and on each device a person owns. iCloud provides a number of options for storing app data, where each option provides a different set of features and a varying amount of control over that data. Some options are easier to implement than others, so it’s important that you carefully consider each one and its suitability for your app before choosing CloudKit.

### [Store documents, images, and other file types in iCloud](/documentation/cloudkit/deciding-whether-cloudkit-is-right-for-your-app#Store-documents-images-and-other-file-types-in-iCloud)

If your app creates files such as documents and images, and you want those files to sync across a person’s devices, use iCloud Documents. You decide which files to sync by storing them in your app’s ubiquity container, and iCloud manages the persistence and synchronization of those files for you. For more information, see [`url(forUbiquityContainerIdentifier:)`](/documentation/Foundation/FileManager/url\(forUbiquityContainerIdentifier:\)) and [Configuring iCloud services](/documentation/Xcode/configuring-icloud-services).

In addition, the device’s owner can use iCloud Backup to store a snapshot of their device, including files that your app creates. That person can then restore an existing or new device from that snapshot. Even though your app isn’t explicitly participating in the system backup process, any files your app creates contribute to the backup’s overall size, and large backups may lead to longer restore times. If your app has files that the system doesn’t need to back up, such as temporary or cached files, use the available APIs to indicate to the system that it can ignore them. For more information, see [Optimizing Your App’s Data for iCloud Backup](/documentation/Foundation/optimizing-your-app-s-data-for-icloud-backup).

### [Sync preferences and other key-value pairs using iCloud](/documentation/cloudkit/deciding-whether-cloudkit-is-right-for-your-app#Sync-preferences-and-other-key-value-pairs-using-iCloud)

If your app maintains a set of feature flags or other lightweight configuration, or enables someone to customize the app’s look and feel, you may want to synchronize that state across a person’s devices to create a rich and consistent experience. Using iCloud key-value storage, your app can store a maximum of 1,024 string keys, with associated data, and iCloud automatically keeps those key-value pairs in sync. Key-value storage supports only numeric types, as well as [`Bool`](/documentation/Swift/Bool), [`String`](/documentation/Swift/String), [`Date`](/documentation/Foundation/Date), [`Data`](/documentation/Foundation/Data), [`Array`](/documentation/Swift/Array), and [`Dictionary`](/documentation/Swift/Dictionary). For more information, see [`NSUbiquitousKeyValueStore`](/documentation/Foundation/NSUbiquitousKeyValueStore).

### [Store model objects in CloudKit](/documentation/cloudkit/deciding-whether-cloudkit-is-right-for-your-app#Store-model-objects-in-CloudKit)

If your app manages a complex graph of model objects, which may include relationships between those objects, use one of the options in the following table:

[`NSPersistentCloudKitContainer`](/documentation/CoreData/NSPersistentCloudKitContainer)

If you don’t require granular control over how and when your data syncs, or your app already uses [`NSPersistentContainer`](/documentation/CoreData/NSPersistentContainer) to persist data to disk, prefer this approach. [`NSPersistentCloudKitContainer`](/documentation/CoreData/NSPersistentCloudKitContainer) provides a fully managed schema, maintains a local replica of your data, and supports the public, private, and shared databases. For more information, see [Setting Up Core Data with CloudKit](/documentation/CoreData/setting-up-core-data-with-cloudkit).

[`CKSyncEngine`](/documentation/cloudkit/cksyncengine-5sie5)

With a sync engine, you retain control of your data but the engine automatically schedules sync operations to fetch and send changes to that data. Your app participates in those operations by handling sync events and providing changed records when the engine requests them. Use [`CKSyncEngine`](/documentation/cloudkit/cksyncengine-5sie5) with private and shared databases.

[`CKDatabase`](/documentation/cloudkit/ckdatabase), [`CKOperation`](/documentation/cloudkit/ckoperation), and related types

CloudKit’s base types provide full control over the data you store, and the design and management of your container’s schema. This approach, however, is the most intricate and requires you to manually fetch and send records, resolve any conflicts, schedule operations, handle iCloud account changes, process change notifications, persist server change tokens, and so on. For more information, see [Enabling CloudKit in Your App](/documentation/cloudkit/enabling-cloudkit-in-your-app).

Current page is Deciding whether CloudKit is right for your app