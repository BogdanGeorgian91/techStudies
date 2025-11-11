# Mirroring a Core Data store with CloudKit

Back user interfaces with a local replica of a CloudKit private database.

## [Overview](/documentation/coredata/mirroring-a-core-data-store-with-cloudkit#overview)

Use Core Data with CloudKit to give users seamless access to the data in your app across all their devices.

Core Data with CloudKit combines the benefits of local persistence with cloud backup and distribution. Core Data provides powerful object graph management features for developing an app with structured data. CloudKit lets users access their data across every device on their iCloud account, while serving as an always-available backup service.

![Flow diagram showing a record syncing between CloudKit and three devices: a laptop, an iPad, and an iPhone.](https://docs-assets.developer.apple.com/published/9dac8da0d3baed269f67ac229a5ca105/media-3226649%402x.png)

### [Determine If Your App Is Eligible for Core Data with CloudKit](/documentation/coredata/mirroring-a-core-data-store-with-cloudkit#Determine-If-Your-App-Is-Eligible-for-Core-Data-with-CloudKit)

Apps adopting Core Data can use Core Data with CloudKit as long as the persistent store is an [`NSSQLiteStoreType`](/documentation/coredata/nssqlitestoretype) store, and the data model is compatible with CloudKit limitations. For example, CloudKit does not support unique constraints, undefined attributes, or required relationships.

Apps using CloudKit cannot use Core Data with CloudKit with existing CloudKit containers. To fully manage all aspects of data mirroring, Core Data owns the CloudKit schema created from the Core Data model. Existing CloudKit containers aren’t compatible with this schema. If your app already uses CloudKit, you can add Core Data with CloudKit when synchronizing a Core Data store with a new container. For more information about working with multiple stores, see [Manage multiple stores](/documentation/coredata/setting-up-core-data-with-cloudkit#Manage-multiple-stores).

For more information about data model requirements, see [Create a Data Model](/documentation/coredata/creating-a-core-data-model-for-cloudkit#Create-a-Data-Model).

### [Set Up Your Development Environment](/documentation/coredata/mirroring-a-core-data-store-with-cloudkit#Set-Up-Your-Development-Environment)

You need an [Apple Developer Program](https://developer.apple.com/programs/) account to access the CloudKit service and your team’s CloudKit containers during development.

You also need an iCloud account to save records to a CloudKit container. Core Data with CloudKit uses a specific record zone in the CloudKit private database, which is accessible only to the current user.

You can run and test Core Data with CloudKit apps using Simulator. You may also test with multiple physical devices logged into the same iCloud account. Connect all devices to a stable wireless internet connection to avoid network problems that could hinder synchronization.

For more information, see [Create an iCloud Account for Development](https://developer.apple.com/library/archive/documentation/DataManagement/Conceptual/CloudKitQuickStart/EnablingiCloudandConfiguringCloudKit/EnablingiCloudandConfiguringCloudKit.html#//apple_ref/doc/uid/TP40014987-CH2-SW7).

### [Configure Core Data with CloudKit](/documentation/coredata/mirroring-a-core-data-store-with-cloudkit#Configure-Core-Data-with-CloudKit)

Add Core Data with CloudKit to your project as follows:

1.  Set up your Xcode project’s capabilities to enable CloudKit, and modify your Core Data stack setup to use [`NSPersistentCloudKitContainer`](/documentation/coredata/nspersistentcloudkitcontainer). See [Setting Up Core Data with CloudKit](/documentation/coredata/setting-up-core-data-with-cloudkit).
    
2.  Design your Core Data model, and use it to initialize the CloudKit schema. See [Creating a Core Data Model for CloudKit](/documentation/coredata/creating-a-core-data-model-for-cloudkit).
    
3.  Modify your views to incorporate remote store changes at appropriate times. See [Syncing a Core Data Store with CloudKit](/documentation/coredata/syncing-a-core-data-store-with-cloudkit).
    

Finally, if you are building custom features or writing a web app, discover how to work with the generated CloudKit schema in [Reading CloudKit Records for Core Data](/documentation/coredata/reading-cloudkit-records-for-core-data).

## [Topics](/documentation/coredata/mirroring-a-core-data-store-with-cloudkit#topics)

### [Configuring CloudKit Mirroring](/documentation/coredata/mirroring-a-core-data-store-with-cloudkit#Configuring-CloudKit-Mirroring)

[

Setting Up Core Data with CloudKit](/documentation/coredata/setting-up-core-data-with-cloudkit)

Set up the classes and capabilities that sync your store to CloudKit.

[

Creating a Core Data Model for CloudKit](/documentation/coredata/creating-a-core-data-model-for-cloudkit)

Design a CloudKit-compatible data model and initialize your CloudKit schema.

[

Syncing a Core Data Store with CloudKit](/documentation/coredata/syncing-a-core-data-store-with-cloudkit)

Synchronize objects between devices, and handle store changes in the user interface.

[

Reading CloudKit Records for Core Data](/documentation/coredata/reading-cloudkit-records-for-core-data)

Access CloudKit records created from Core Data managed objects.

# Setting Up Core Data with CloudKit | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [Mirroring a Core Data store with CloudKit](/documentation/coredata/mirroring-a-core-data-store-with-cloudkit)
-   Setting Up Core Data with CloudKit

Article

# Setting Up Core Data with CloudKit

Set up the classes and capabilities that sync your store to CloudKit.

## [Overview](/documentation/coredata/setting-up-core-data-with-cloudkit#overview)

To sync your Core Data store to CloudKit, you enable the CloudKit capability for your app. You also set up the Core Data stack with a persistent container that is capable of managing one or more local persistent stores that are backed by a CloudKit private database.

### [Configure a new Xcode project](/documentation/coredata/setting-up-core-data-with-cloudkit#Configure-a-new-Xcode-project)

When you create a new project, you specify whether you want to add support for Core Data with CloudKit directly from the project setup interface. The resulting project contains an [`NSPersistentCloudKitContainer`](/documentation/coredata/nspersistentcloudkitcontainer). Once you enable CloudKit in your project, you use this container to manage one or more local stores that are backed with a CloudKit database.

1.  Choose File > New > Project to create a new project.
    
2.  Select a project template to use as the starting point for your project, and click Next.
    
3.  Select the Core Data option for Storage.
    
4.  Select the Host in Cloudkit checkbox.
    
5.  Enter any other project details and click Next.
    
6.  Specify a location for your project and click Create.
    

![Screenshot showing the new project creation dialog with the Core Data Storage option and Host in CloudKit checkbox selected.](https://docs-assets.developer.apple.com/published/f748f77f7568bed1705e93b795580c6d/media-4434183%402x.png)

Not all project templates support Core Data. If the template you want to use doesn’t currently support Core Data, add Core Data to the project as described in [Setting up a Core Data stack](/documentation/coredata/setting-up-a-core-data-stack). Then add Core Data with CloudKit as described in [Update an existing Xcode project](/documentation/coredata/setting-up-core-data-with-cloudkit#Update-an-existing-Xcode-project).

### [Enable iCloud](/documentation/coredata/setting-up-core-data-with-cloudkit#Enable-iCloud)

Core Data with CloudKit requires specific entitlements for your app to communicate with the server. If you have created a new template following the steps above, this will be done for you. For an existing app, begin by adding the iCloud capability to your Xcode project.

1.  In Project Settings, select the Signing & Capabilities tab.
    
2.  Make sure that “Automatically manage signing” is selected.
    
3.  Specify your development team.
    
4.  Click the + Capability button, then do a search for iCloud in the Add Capability editor and select that capability.
    

![Screenshot showing the locations of the Add Capability button, Automatically manage signing checkbox, and Team dropdown in the Signing & Capabilities tab in Project Settings.](https://docs-assets.developer.apple.com/published/52078c8b70403835bbd18631b9101788/media-4436809%402x.png)

An iCloud section appears on your app’s Signing & Capabilities page.

### [Enable CloudKit and push notifications](/documentation/coredata/setting-up-core-data-with-cloudkit#Enable-CloudKit-and-push-notifications)

Core Data with CloudKit uses the CloudKit service to access your team’s containers. To enable CloudKit, configure the iCloud capability.

1.  In Project Settings, select the Signing & Capabilities tab and find the iCloud section.
    
2.  Select the CloudKit checkbox. This selection also adds push notifications that notify your app when remote content changes.
    
3.  Under Containers, add or select a container. For more information about working with CloudKit containers and setting up profiles, see [Enabling CloudKit in Your App](https://developer.apple.com/library/archive/documentation/DataManagement/Conceptual/CloudKitQuickStart/EnablingiCloudandConfiguringCloudKit/EnablingiCloudandConfiguringCloudKit.html#//apple_ref/doc/uid/TP40014987-CH2).
    

![Screenshot showing the iCloud section of the Signing & Capabilities tab with CloudKit enabled.](https://docs-assets.developer.apple.com/published/06abe307359943cc63cd1d8c73640f81/media-4436808%402x.png)

Xcode checks that your development team supports the Push Notification and iCloud capabilities, then registers your app’s bundle identifier and manages provisioning profiles.

### [Enable Remote notifications in the background](/documentation/coredata/setting-up-core-data-with-cloudkit#Enable-Remote-notifications-in-the-background)

For CloudKit to silently notify your app when new content is available, without presenting a user notification such as an alert, sound, or badge, you need to enable the Remote notifications Background Mode.

1.  In Project Settings, select the Signing & Capabilities tab.
    
2.  Click the + Capability button, then do a search for Background Modes in the Add Capability editor and select that capability.
    
3.  Select the “Remote notifications” checkbox.
    

![Screenshot showing the Background Modes section of the Signing & Capabilities tab with the Remote notifications checkbox selected.](https://docs-assets.developer.apple.com/published/ada6c9909dace104f96da43d22dc037d/media-4436810%402x.png)

For more information about silent notifications, see [Pushing background updates to your App](/documentation/UserNotifications/pushing-background-updates-to-your-app).

### [Update an existing Xcode project](/documentation/coredata/setting-up-core-data-with-cloudkit#Update-an-existing-Xcode-project)

If you want to add Core Data with CloudKit to an app that already uses Core Data, you need to modify both your project’s configuration and some of its code.

First, enable iCloud, CloudKit, Push notifications, and Remote notifications in the background as described in the preceding sections. Then, replace your persistent container with an instance of [`NSPersistentCloudKitContainer`](/documentation/coredata/nspersistentcloudkitcontainer).

For example, if you created a project from the Multiplatform App template, with the Use Core Data checkbox selected, the following code appeared in the `PersistenceController`:

```
init(inMemory: Bool = false) {
    container = NSPersistentContainer(name: "Earthquakes")
    if inMemory {
        container.persistentStoreDescriptions.first!.url = URL(fileURLWithPath: "/dev/null")
    }
    container.loadPersistentStores(completionHandler: { (storeDescription, error) in
        if let error = error as NSError? {
            fatalError("Unresolved error \(error), \(error.userInfo)")
        }
    })
    container.viewContext.automaticallyMergesChangesFromParent = true
}
```

[`NSPersistentContainer`](/documentation/coredata/nspersistentcontainer) supports only local persistent stores. To add the ability to sync a local store to a CloudKit database, replace [`NSPersistentContainer`](/documentation/coredata/nspersistentcontainer) with the subclass [`NSPersistentCloudKitContainer`](/documentation/coredata/nspersistentcloudkitcontainer).

```
    let container = NSPersistentCloudKitContainer(name: "Earthquakes")
```

### [Manage multiple stores](/documentation/coredata/setting-up-core-data-with-cloudkit#Manage-multiple-stores)

You might need to mirror a subset of your data using CloudKit, while keeping other data completely local. You can add configurations to your model to organize your data in separate stores, then choose which stores to sync to CloudKit.

1.  Open your project’s `.xcdatamodeld` file.
    
2.  Choose Editor > Add Configuration.
    
3.  Drag each entity into a configuration.
    
4.  Select a configuration that you want to sync to CloudKit, then select the “Used with CloudKit” checkbox in the data model editor. Repeat for each configuration that you want to sync.
    

![Screenshot showing the .xcdatamodeld file with the Configuration list at left containing Default, Cache, Cloud, and Local configurations. The Cloud configuration is selected and its “Used with CloudKit” checkbox is selected in the Data Model inspector.](https://docs-assets.developer.apple.com/published/ecf5d61fd34f3d17ac184ba7bccf9bf1/media-4445965%402x.png)

For an app without configurations, [`NSPersistentCloudKitContainer`](/documentation/coredata/nspersistentcloudkitcontainer) matches the first store description with the first CloudKit container identifier in the entitlements.

For an app with configurations, you need to tell it which CloudKit container, if any, to use with each store. For each of your configurations, make the following changes to your container setup:

1.  Create an [`NSPersistentStoreDescription`](/documentation/coredata/nspersistentstoredescription).
    
2.  Set the store description’s configuration.
    
3.  If this configuration needs to synchronize to CloudKit, set the store description’s [`cloudKitContainerOptions`](/documentation/coredata/nspersistentstoredescription/cloudkitcontaineroptions) to an instance of [`NSPersistentCloudKitContainerOptions`](/documentation/coredata/nspersistentcloudkitcontaineroptions). Provide the container identifier of your CloudKit container in the initializer.
    
4.  Add the store description to the persistent container before loading the store.
    

The following example code creates two store descriptions: one for the “Local” configuration, and one for the “Cloud” configuration. It then sets the cloud store description’s [`cloudKitContainerOptions`](/documentation/coredata/nspersistentstoredescription/cloudkitcontaineroptions) to match the store with its CloudKit container. Finally, it updates the container’s list of persistent store descriptions to include all local and cloud-backed store descriptions, and loads both stores.

```
lazy var persistentContainer: NSPersistentCloudKitContainer = {
    let container = NSPersistentCloudKitContainer(name: "Earthquakes")
    
    // Create a store description for a local store.
    let localStoreLocation = URL(fileURLWithPath: "/path/to/local.store")
    let localStoreDescription =
        NSPersistentStoreDescription(url: localStoreLocation)
    localStoreDescription.configuration = "Local"
    
    // Create a store description for a CloudKit-backed local store.
    let cloudStoreLocation = URL(fileURLWithPath: "/path/to/cloud.store")
    let cloudStoreDescription =
        NSPersistentStoreDescription(url: cloudStoreLocation)
    cloudStoreDescription.configuration = "Cloud"


    // Set the container options on the cloud store.
    cloudStoreDescription.cloudKitContainerOptions = 
        NSPersistentCloudKitContainerOptions(
            containerIdentifier: "com.my.container")
    
    // Update the container's list of store descriptions.
    container.persistentStoreDescriptions = [
        cloudStoreDescription,
        localStoreDescription
    ]
    
    // Load both stores.
    container.loadPersistentStores { storeDescription, error in
        guard error == nil else {
            fatalError("Could not load persistent stores. \(error!)")
        }
    }
    
    return container
}()
```

# Creating a Core Data Model for CloudKit | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [Mirroring a Core Data store with CloudKit](/documentation/coredata/mirroring-a-core-data-store-with-cloudkit)
-   Creating a Core Data Model for CloudKit

Article

# Creating a Core Data Model for CloudKit

Design a CloudKit-compatible data model and initialize your CloudKit schema.

## [Overview](/documentation/coredata/creating-a-core-data-model-for-cloudkit#overview)

To pass records between a Core Data store and a CloudKit database, they both require a shared understanding of the data structure. You define this in the Core Data model and then use that to generate a CloudKit schema.

![Flow diagram showing that your app’s Core Data model generates a CloudKit schema.](https://docs-assets.developer.apple.com/published/81e8f1f9f7ae3eee6d4ead6744ae33d7/media-3230504%402x.png)

### [Create a Data Model](/documentation/coredata/creating-a-core-data-model-for-cloudkit#Create-a-Data-Model)

Use Xcode’s Core Data model editor to define your app’s entities and their attributes, or construct your model in code. For more information, see [Creating a Core Data model](/documentation/coredata/creating-a-core-data-model) and the articles under [Modeling data](/documentation/coredata/modeling-data).

CloudKit doesn’t support all the features of a Core Data model. As you design your model, be aware of the following limitations and make sure you create a compatible data model.

Core Data model element

CloudKit schema limitation

**Entities**

Unique constraints aren’t supported.

**Attributes**

`Undefined` and [`objectID`](/documentation/coredata/nsmanagedobject/objectid) attribute types aren’t supported.

**Relationships**

All relationships must be optional. Due to operation size limitations, CloudKit may not save relationship changes atomically. ![](https://docs-assets.developer.apple.com/published/67dc4b07a8d84366d4cc0e812eb40b4a/spacer.png) All relationships must have an inverse, in case the records synchronize out of order. ![](https://docs-assets.developer.apple.com/published/67dc4b07a8d84366d4cc0e812eb40b4a/spacer.png) CloudKit doesn’t support the Deny deletion rule.

**Configurations**

Entities in a configuration must not have relationships to entities in another configuration.

For more information about how Core Data translates managed objects to CloudKit records, see [Reading CloudKit Records for Core Data](/documentation/coredata/reading-cloudkit-records-for-core-data).

### [Initialize Your CloudKit Schema During Development](/documentation/coredata/creating-a-core-data-model-for-cloudkit#Initialize-Your-CloudKit-Schema-During-Development)

After creating your Core Data model, inform CloudKit about the types of records it contains by initializing a development schema. This is a draft schema that you can rewrite as often as necessary during development. You can’t delete a record type or modify any existing attributes after you promote a development schema to production.

Use the persistent container to initialize the development schema, which you can do during app launch or from within one or more integration tests. To exclude schema initialization from your production builds, use the following:

```
let container = NSPersistentCloudKitContainer(name: "Earthquakes")


// Only initialize the schema when building the app with the 
// Debug build configuration.        
#if DEBUG
do {
    // Use the container to initialize the development schema.
    try container.initializeCloudKitSchema(options: [])
} catch {
    // Handle any errors.
}
#endif
```

After initializing the schema, the console contains an entry similar to the following:

```
<NSCloudKitMirroringDelegate: 0x7f9699d29a90>: Successfully set up CloudKit 
    integration for store
```

While initializing the schema, Core Data creates a temporary instance of each distinct record type in each of the container’s stores that mirror to a CloudKit database and uploads them to the iCloud servers. After completing the upload, the schema is visible in the CloudKit dashboard and Core Data removes the temporary records.

For more information about configuring a CloudKit persistent container, see [Setting Up Core Data with CloudKit](/documentation/coredata/setting-up-core-data-with-cloudkit).

### [Reset the Environment](/documentation/coredata/creating-a-core-data-model-for-cloudkit#Reset-the-Environment)

As you change the model during development, periodically visit the CloudKit dashboard to reset the development environment and delete the existing development schema, before initializing a new one. For more information about resetting the development environment, see [Using CloudKit Dashboard to Manage Databases](https://developer.apple.com/library/archive/documentation/DataManagement/Conceptual/CloudKitQuickStart/EditingSchemesUsingCloudKitDashboard/EditingSchemesUsingCloudKitDashboard.html#//apple_ref/doc/uid/TP40014987-CH5).

### [Promote the Schema to Production](/documentation/coredata/creating-a-core-data-model-for-cloudkit#Promote-the-Schema-to-Production)

When you’re happy with your data model, have a fully tested app, and are ready to submit it to the App Store, it’s time to promote your schema from development to production.

Initialize your schema one last time, then promote it from the CloudKit dashboard. For more information about promoting a schema from development to production, see [Deploying the Schema](https://developer.apple.com/library/archive/documentation/DataManagement/Conceptual/CloudKitQuickStart/DeployingYourCloudKitApp/DeployingYourCloudKitApp.html#//apple_ref/doc/uid/TP40014987-CH10).

### [Update the Production Schema](/documentation/coredata/creating-a-core-data-model-for-cloudkit#Update-the-Production-Schema)

Plan carefully for how your app handles forward compatibility and major changes to the data model, because you can’t rename or delete CloudKit record types and fields in production. Consider these strategies:

-   Migrate users to a completely new store, using [`NSPersistentCloudKitContainerOptions`](/documentation/coredata/nspersistentcloudkitcontaineroptions) to associate the new store with a new container.
    
-   Incrementally add new fields to existing record types. If you adopt this approach, older versions of your app have access to every record a user creates, but not every field.
    
-   Version your entities by including a `version` attribute from the outset, and use a fetch request to select only those records that are compatible with the current version of the app. If you adopt this approach, older versions of your app won’t fetch records that a user creates with a more recent version, effectively hiding them on that device.
    

For example, consider a `Post` entity with a `version` attribute that stores the version of the app that creates the record. You can use a predicate to fetch only the records that are compatible with the current version of the app.

```
// The current version of the app's data model.
let maxCompatibleVersion = 3


let context = NSManagedObjectContext(
    concurrencyType: .privateQueueConcurrencyType
)


context.performAndWait {
    let fetchRequest = NSFetchRequest<NSManagedObject>(entityName: "Post")
    
    // Create a predicate that matches against the version attribute.
    fetchRequest.predicate = NSPredicate(
        format: "version <= %d",
        argumentArray: [maxCompatibleVersion]
    )
    
    // Fetch all posts with a version less than or equal to maxCompatibleVersion.
    let results = context.fetch(fetchRequest)
}
```

Along with these choices, consider your timelines for supporting multiple versions of your app, and for migrating users to newer app versions.

For tips on migrating records to a new schema, see [Designing for CloudKit](https://developer.apple.com/library/archive/documentation/General/Conceptual/iCloudDesignGuide/DesigningforCloudKit/DesigningforCloudKit.html#//apple_ref/doc/uid/TP40012094-CH9). For more information about Core Data model migration, see [Migrating your data model automatically](/documentation/coredata/migrating-your-data-model-automatically). For heavyweight migrations, see the [Core Data Model Versioning and Data Migration](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreDataVersioning/Articles/Introduction.html#//apple_ref/doc/uid/TP40004399-CH1) guide.

# Syncing a Core Data Store with CloudKit | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [Mirroring a Core Data store with CloudKit](/documentation/coredata/mirroring-a-core-data-store-with-cloudkit)
-   Syncing a Core Data Store with CloudKit

Article

# Syncing a Core Data Store with CloudKit

Synchronize objects between devices, and handle store changes in the user interface.

## [Overview](/documentation/coredata/syncing-a-core-data-store-with-cloudkit#overview)

Once you set up your Xcode project (see [Setting Up Core Data with CloudKit](/documentation/coredata/setting-up-core-data-with-cloudkit)) and initialize your development schema (see [Creating a Core Data Model for CloudKit](/documentation/coredata/creating-a-core-data-model-for-cloudkit)), you’re ready to sync a Core Data store to CloudKit.

### [Run Your App and Create Managed Object Instances](/documentation/coredata/syncing-a-core-data-store-with-cloudkit#Run-Your-App-and-Create-Managed-Object-Instances)

Run your app and begin creating [`NSManagedObject`](/documentation/coredata/nsmanagedobject) instances. The objects automatically synchronize with the CloudKit database and propagate to other devices logged into the same iCloud account. The tasks that send changes to the cloud and receive remote changes in the local store happen on the system in the background. You don’t need to add any code to your project to synchronize records across devices.

It can be helpful to think of this process as similar to the water cycle. Water evaporates up and rains down on a natural cadence. Similarly, changes move from Core Data up to CloudKit and across to other devices on a natural rhythm within the system event loop.

Generally, you can expect data to synchronize a local change within about a minute of the change. Core Data also occasionally syncs CloudKit data in scenarios such as when the app hasn’t synced in a long time.

### [Upload Core Data Changes to CloudKit](/documentation/coredata/syncing-a-core-data-store-with-cloudkit#Upload-Core-Data-Changes-to-CloudKit)

When the user makes a change on one device, Core Data uploads the change to CloudKit before sending it to the user’s other devices.

![Flow diagram showing the upload portion of a record’s lifecycle.](https://docs-assets.developer.apple.com/published/56f77daeb6d38916bc9aa82eeb802b46/media-3230553%402x.png)

First, the user creates, updates, or deletes a managed object, such as adding a post or editing a tag. When its managed object context saves changes to the store, Core Data creates a background task for the system to convert the [`NSManagedObject`](/documentation/coredata/nsmanagedobject) to a [`CKRecord`](/documentation/CloudKit/CKRecord). The system executes the task, creating the record and uploading it to CloudKit.

For more information about background tasks, see [`UIApplication`](/documentation/UIKit/UIApplication).

### [Download CloudKit Changes into Core Data](/documentation/coredata/syncing-a-core-data-store-with-cloudkit#Download-CloudKit-Changes-into-Core-Data)

After CloudKit receives a change and saves it to the CloudKit store, it notifies the user’s other devices about the change.

![Flow diagram showing the download portion of a record’s lifecycle.](https://docs-assets.developer.apple.com/published/f1a19f2b401d26c9dffa0a1b72edbf5a/media-3230554%402x.png)

First, CloudKit periodically sends push notifications to other devices on a user’s account. Then, on each device, the system creates a background task to download all of the changed records since the last fetch and converts them into instances of [`NSManagedObject`](/documentation/coredata/nsmanagedobject). Finally, Core Data saves these managed objects into the local store.

For more information about push notifications, see [User Notifications](/documentation/UserNotifications).

### [Isolate the Current View from Store Changes](/documentation/coredata/syncing-a-core-data-store-with-cloudkit#Isolate-the-Current-View-from-Store-Changes)

Consider what happens if a user deletes a record from their phone. This change uploads to CloudKit, and later downloads to a laptop and an iPad. The iPad’s current view may still show the record if the UI hasn’t updated with the changes yet. The user taps on the now-deleted record, which is no longer available in the store. This may lead to inconsistent representation of the record, such as missing data, in your UI.

For this reason, you need to isolate the current view from changes to the store by ensuring that the records the view expects continue to exist. Using query generations, you pin the persistent container’s [`viewContext`](/documentation/coredata/nspersistentcontainer/viewcontext) to a specific generation of store data. This allows the context to fulfill faults — placeholder objects whose values haven’t yet been fetched — that existed at the time the view loaded, even if the store changes underneath.

Pin a context to a query generation before its first read from the store.

```
try? persistentContainer.viewContext.setQueryGenerationFrom(.current)
```

Any time you save, merge, or reset the context, it automatically updates its pin to the current query generation.

For more information about faults, see [Faulting and Uniquing](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/FaultingandUniquing.html#//apple_ref/doc/uid/TP40001075-CH18).

For more information about query generations, see [Accessing data when the store changes](/documentation/coredata/accessing-data-when-the-store-changes).

### [Integrate Store Changes Relevant to the Current View](/documentation/coredata/syncing-a-core-data-store-with-cloudkit#Integrate-Store-Changes-Relevant-to-the-Current-View)

Your app receives remote change notifications when the local store updates from CloudKit. However, it’s unnecessary to update your UI in response to every notification, because some changes may not be relevant to the current view.

Analyze the persistent history to determine whether the changes are relevant to the current view before consuming them in the user interface. Inspect the details of each transaction, such as the entity name, its updated properties, and the type of change, to decide whether to act.

For more information about persistent history tracking, see [Consuming relevant store changes](/documentation/coredata/consuming-relevant-store-changes).

### [Debug Errors in Core Data with CloudKit](/documentation/coredata/syncing-a-core-data-store-with-cloudkit#Debug-Errors-in-Core-Data-with-CloudKit)

Most errors, like those that result from a network failure or a user not being signed in, are transient and don’t require action. You can choose the level of detail that Core Data with CloudKit logs to the system log.

Choose Product > Scheme > Edit Scheme. Select an action such as Run, and select the Arguments tab. Pass the `com.apple.CoreData.CloudKitDebug` user default setting with a debug level value as an argument to the application.

![Screenshot showing passing the -com.apple.CoreData.CloudKitDebug 3 user default setting as a launch argument in the scheme editor.](https://docs-assets.developer.apple.com/published/82f0ac367f1ff31887c307ca788610b9/media-3221950%402x.png)

Higher argument values produce more information; start at `1` and increase if you need more detail. For more information about handling errors, see [Troubleshooting Core Data](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/TroubleshootingCoreData.html#//apple_ref/doc/uid/TP40001075-CH26).

If you observe persistent errors that don’t automatically recover, file a bug. For more information about bug reporting, see [Submitting Bugs and Feedback](https://developer.apple.com/bug-reporting/).

### [Inspect Logs to See What Happened](/documentation/coredata/syncing-a-core-data-store-with-cloudkit#Inspect-Logs-to-See-What-Happened)

If Core Data with CloudKit doesn’t appear to be syncing, confirm that you’re testing on two unlocked devices logged into the same iCloud account, with good wireless internet connections.

Push notifications may get dropped or deferred, so don’t rely on them for testing. Watch system logs to observe the status and result of expected activity. Run the `log stream` command from the terminal, filtering by process and container ID. If the logs don’t include the operation, your push may have been dropped. Check the originating device for export activity.

Filter CloudKit logs to see operations on the `cloudd` process for your container.

```
$ log stream --info --debug --predicate 'process = "cloudd" and message
contains[cd] "containerID=com.mycontainer"'
```

Filter Core Data logs to see information about the mirroring delegate’s setup, exports, and imports for your process.

```
$ log stream --info --debug --predicate 'process = "myprocess" and 
(subsystem = "com.apple.coredata" or subsystem = "com.apple.cloudkit")'
```

Or monitor both CloudKit and Core Data logs at the same time.

```
$ log stream --info --debug --predicate '(process = "myprocess" and
(subsystem = "com.apple.coredata" or subsystem = "com.apple.cloudkit")) or
(process = "cloudd" and message contains[cd] "container=com.mycontainer")'
```

For more information about logging, see [Viewing Log Messages](/documentation/os/viewing-log-messages).

# Reading CloudKit Records for Core Data | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [Mirroring a Core Data store with CloudKit](/documentation/coredata/mirroring-a-core-data-store-with-cloudkit)
-   Reading CloudKit Records for Core Data

Article

# Reading CloudKit Records for Core Data

Access CloudKit records created from Core Data managed objects.

## [Overview](/documentation/coredata/reading-cloudkit-records-for-core-data#overview)

Although your Core Data app interacts primarily with managed objects, you can access a managed object’s [`CKRecord`](/documentation/CloudKit/CKRecord) directly. This is useful if you’re leveraging CloudKit to add features like sharing. You can also use [CloudKit JS](/documentation/CloudKitJS) to access CloudKit records from your web app.

To prevent collision with existing CloudKit record types and reserved names, CloudKit prefixes the [`CKRecord`](/documentation/CloudKit/CKRecord) types and fields it creates for your Core Data entities with `CD_`.

To work with records directly, you need to understand the mappings between entities and record types, attributes and fields, and the ways a record stores relationships.

### [Read Entities from Record Types](/documentation/coredata/reading-cloudkit-records-for-core-data#Read-Entities-from-Record-Types)

CloudKit doesn’t typically support inheritance, so it provides only a single system field, [`recordType`](/documentation/CloudKit/CKRecord/recordType-6v7au), to hold type information. Core Data stores the name of the _root entity_ from the inheritance hierarchy in [`recordType`](/documentation/CloudKit/CKRecord/recordType-6v7au).

When you initialize a schema, Core Data adds a custom field to the record type, `CD_entityName`, to store the name of the _current entity_.

![Layout diagram showing a Post entity with content and title attributes.](https://docs-assets.developer.apple.com/published/95e740430e55a1bded89539a2904d7de/media-3227969%402x.png)

For example, an entity named `Post` generates the following structure (before adding its attributes), with its `CD_entityName` set to `Post`, and its `recordType` set to `CD_Post`.

```
<CKRecord: 0x7fbae9e19510; recordID=CD_Post_UUID:
(com.apple.coredata.cloudkit.zone:__defaultOwner__), values={
    "CD_entityName" = Post;
}, recordType=CD_Post>
```

Consider a second entity, `ImagePost`, that inherits from Post.

![Layout diagram showing an ImagePost entity that inherits from Post to add an imageData attribute, and a VideoPost entity that inherits from Post to add a VideoPost attribute.](https://docs-assets.developer.apple.com/published/55a504b60e495cc790ed2d27402feaf6/media-3227968.png)

`ImagePost` generates the following structure (before adding its attributes), with its `CD_entityName` set to `ImagePost`, and its `recordType` set to `CD_Post`.

```
<CKRecord: 0x7f9c9fe17780; recordID=CD_ImagePost_UUID:
(com.apple.coredata.cloudkit.zone:__defaultOwner__), values={
    "CD_entityName" = ImagePost;
}, recordType=CD_Post>
```

Query against [`recordType`](/documentation/cloudkit/ckrecord/1462206-recordtype) when searching against an entity’s inheritance hierarchy. Query against `CD_entityName` when searching for instances of a specific type.

For more information about CloudKit queries, see [`CKQuery`](/documentation/CloudKit/CKQuery).

### [Read Attributes from Fields](/documentation/coredata/reading-cloudkit-records-for-core-data#Read-Attributes-from-Fields)

When you initialize a schema, Core Data creates fields for each of an entity’s attributes, mapping the attribute name to a field with a key in the form `CD_[attribute.name]`. The field’s type may vary between Core Data and CloudKit.

Core Data attribute type

`NSManagedObject` attribute type

`CKRecord` attribute type

`Integer 16`

[`NSNumber`](/documentation/Foundation/NSNumber)

[`NSNumber`](/documentation/Foundation/NSNumber)

`Integer 32`

[`NSNumber`](/documentation/Foundation/NSNumber)

[`NSNumber`](/documentation/Foundation/NSNumber)

`Integer 64`

[`NSNumber`](/documentation/Foundation/NSNumber)

[`NSNumber`](/documentation/Foundation/NSNumber)

`Double`

[`NSNumber`](/documentation/Foundation/NSNumber)

[`NSNumber`](/documentation/Foundation/NSNumber)

`Float`

[`NSNumber`](/documentation/Foundation/NSNumber)

[`NSNumber`](/documentation/Foundation/NSNumber)

`Boolean`

[`NSNumber`](/documentation/Foundation/NSNumber)

[`NSNumber`](/documentation/Foundation/NSNumber)

`Date`

[`NSDate`](/documentation/Foundation/NSDate)

[`NSDate`](/documentation/Foundation/NSDate)

`Decimal`

[`NSDecimalNumber`](/documentation/Foundation/NSDecimalNumber)

[`NSNumber`](/documentation/Foundation/NSNumber)

`UUID`

[`NSUUID`](/documentation/Foundation/NSUUID)

[`NSString`](/documentation/Foundation/NSString)

`URI`

[`NSURL`](/documentation/Foundation/NSURL)

[`NSString`](/documentation/Foundation/NSString)

`String`

[`NSString`](/documentation/Foundation/NSString)

[`NSString`](/documentation/Foundation/NSString) or [`CKAsset`](/documentation/CloudKit/CKAsset)

`Binary Data`

[`NSData`](/documentation/Foundation/NSData)

[`NSData`](/documentation/Foundation/NSData) or [`CKAsset`](/documentation/CloudKit/CKAsset)

`Transformable`

[`NSData`](/documentation/Foundation/NSData)

[`NSData`](/documentation/Foundation/NSData) or [`CKAsset`](/documentation/CloudKit/CKAsset)

`Undefined`

—

not supported

`Object ID`

—

not supported

All variable length attribute types—String, Binary Data, and Transformable—generate an additional field with a key in the form `CD_[attribute.name]_ckAsset`. If a field’s value grows too large to store within the record size limit of 1MB, Core Data automatically converts the value to an external asset. Core Data transitions between the original field and its asset counterpart transparently during serialization. When inspecting a CloudKit record directly, check the length of the original field’s value; if it is zero, look in the asset field.

![Layout diagram showing a Post entity with content and title attributes.](https://docs-assets.developer.apple.com/published/e80e3ec91c444bd2ac6d87ed1874177d/media-3222599%402x.png)

For example, an entity named `Post` with String `content` and `title` attributes would generate the following fully materialized record, with pairs of fields for `CD_content and` `CD_content_ckAsset`, and for `CD_title` and `CD_title_ckAsset`.

```
<CKRecord: 0x7f9c9fd0f870; recordID=CD_Post_UUID:
(com.apple.coredata.cloudkit.zone:__defaultOwner__), values={
    "CD_content" = "An example core data string";
    "CD_content_ckAsset" = "<CKAsset: 0x7f9c9fe1db50; 
path=/var/folders/*/C9EDC901-385B-4778-9D78-03E9C740AD89.fxd, 
UUID=C37985B7-F959-4174-AA93-C404F9DCC6A5>";
    "CD_entityName" = Post;
    "CD_title" = "An example core data string";
    "CD_title_ckAsset" = "<CKAsset: 0x7f9c9fd10140; 
path=/var/folders/*/C7977A3A-623E-441E-9086-66F2F5B7B746.fxd, 
UUID=81800071-ECBD-46E1-B4F9-2F7168269497>";
}, recordType=CD_Post
```

### [Read One-to-One Relationships from Fields](/documentation/coredata/reading-cloudkit-records-for-core-data#Read-One-to-One-Relationships-from-Fields)

One-to-one relationships store foreign keys in both related records, mapping the relationship name to a field with a key in the form `CD_[relationship.name]`. This field stores the foreign key of the related object in the form `CKRecord.recordID.recordName`.

For example, consider a one-to-one relationship between an `ImageData` entity and an `Attachment` entity. In Core Data, the `Attachment` has an `imageData` relationship, and the `ImageData` has an `attachment` relationship.

![Flow diagram showing a one-to-one relationship between an ImageData entity with a data attribute and attachment relationship, to an Attachment entity with thumbnail and uuid attributes and an imageData relationship.](https://docs-assets.developer.apple.com/published/91cb641cd69243569d115c68e0d72084/media-3225833%402x.png)

This one-to-one relationship between `ImageData` and `Attachment` would generate the following CloudKit records.

```
<CKRecord: 0x7f9ca1300f50; recordID=CD_Attachment_UUID:
(com.apple.coredata.cloudkit.zone:__defaultOwner__), values={
    "CD_entityName" = Attachment;
    "CD_imageData" = "CD_ImageData_UUID";
}, recordType=CD_Attachment>
<CKRecord: 0x7f9c9fc18610; recordID=CD_ImageData_UUID:
(com.apple.coredata.cloudkit.zone:__defaultOwner__), values={
    "CD_attachment" = "CD_Attachment_UUID";
    "CD_entityName" = ImageData;
}, recordType=CD_ImageData>
```

The `CD_imageData` field on the `CD_Attachment` contains the foreign key of the image, and the `CD_attachment` field on the `CD_ImageData` contains the foreign key of the attachment.

### [Read One-to-Many Relationships from Fields](/documentation/coredata/reading-cloudkit-records-for-core-data#Read-One-to-Many-Relationships-from-Fields)

One-to-many relationships store a foreign key on each record on the many side of the relationship, mapping the relationship name to a field with a key in the form `CD_[relationship name]`. This field stores the foreign key of the related object in the form `CKRecord.recordID.recordName`.

![Flow diagram showing a one-to-many relationship between an Post entity with content and title attributes and an attachments relationship; to an Attachment entity with thumbnail and uuid attributes and a post relationship.](https://docs-assets.developer.apple.com/published/7f5d837dab544513c6f43bdeee3c7149/media-3225834%402x.png)

For example, a one-to-many relationship between a single `Post` and multiple `Attachment` instances would generate multiple `CD_Attachment` records. Each record contains the foreign key of the `Post` it belongs to in their `CD_post` field.

```
<CKRecord: 0x7f9ca1300f50; recordID=CD_Attachment_UUID:
(com.apple.coredata.cloudkit.zone:__defaultOwner__), values={
    "CD_entityName" = Attachment;
    "CD_post" = "CD_VideoPost_UUID";
}, recordType=CD_Attachment>
```

Generated `Post` records don’t contain a reference to their attachments.

### [Read Many-to-Many Relationships from CDMR Records](/documentation/coredata/reading-cloudkit-records-for-core-data#Read-Many-to-Many-Relationships-from-CDMR-Records)

Many-to-many relationships model the join table using a custom Core Data Mirrored Relationship (CDMR) record type.

CloudKit doesn’t support the notion of a join, and it’s inefficient to encode arrays on both records and keep them in sync. Instead, Core Data constructs a CDMR record to accurately and succintly capture all of the criteria of the join.

CDMR records have the following fields.

CDMR Field

Description

`CD_entityNames`

An alphabetically sorted, semicolon-separated list of the entities in the relationship, for example, `“Post:Tag”`. The ordering of this field determines the ordering for all other fields.

`CD_recordNames`

The record names of the two related objects, for example, `“CD_Post_F587C290-BC2F-441B-98FC-1357BA89C411:CD_Tag_215FA1E0-6A16-4A2B-BFA2-C13202BE6D50”`, sorted according to the CD\_entityNames order.

`CD_relationships`

The relationship names, for example, `“tags:posts”`, sorted according to the CD\_entityNames order.

For example, consider a many-to-many relationship between `Tag` and `Post` entities.

![Flow diagram showing a many-to-many relationship between a Post entity with content and title attributes and a tags relationship; to a Tag entity with color, name, and uuid attributes and a posts relationship.](https://docs-assets.developer.apple.com/published/9ccc4233c22521bdb7178bdb3b4ec10c/media-3225832%402x.png)

The individual `Tag` and `Post` records don’t contain fields for the relationship.

```
<CKRecord: 0x7f9c9fd0f870; recordID=CD_Post_UUID:
(com.apple.coredata.cloudkit.zone:__defaultOwner__), values={
    "CD_content" = "An example core data string";
    "CD_content_ckAsset" = "<CKAsset: 0x7f9c9fe1db50; 
path=/var/folders/*/C9EDC901-385B-4778-9D78-03E9C740AD89.fxd, 
UUID=C37985B7-F959-4174-AA93-C404F9DCC6A5>";
    "CD_entityName" = Post;
    "CD_title" = "An example core data string";
    "CD_title_ckAsset" = "<CKAsset: 0x7f9c9fd10140; 
path=/var/folders/*/C7977A3A-623E-441E-9086-66F2F5B7B746.fxd, 
UUID=81800071-ECBD-46E1-B4F9-2F7168269497>";
}, recordType=CD_Post>
<CKRecord: 0x7f9ca10188d0; recordID=CD_Tag_UUID:
(com.apple.coredata.cloudkit.zone:__defaultOwner__), values={
    "CD_color" = {length = 17, bytes = 0x536f6d65206578616d706c652064617461};
    "CD_color_ckAsset" = "<CKAsset: 0x7f9c9fd07790; 
path=/var/folders/*/5D5DF5B2-DB27-4F01-B311-52A274374F59.fxd, 
UUID=787F4868-1F4D-4BF7-86D4-3867BEA65172>";
    "CD_entityName" = Tag;
    "CD_name" = "An example core data string";
    "CD_name_ckAsset" = "<CKAsset: 0x7f9ca1300af0; 
path=/var/folders/*/C40A1E1F-C2F5-4BA1-A6ED-F5977301A1F7.fxd, 
UUID=A4C4B698-55FC-4C87-BA8A-D6DD0011DD90>";
    "CD_uuid" = "51BAFD98-D1F7-472F-95D6-BBF40D7CBD75";
}, recordType=CD_Tag>
```

The relationship between any two `Tag` and `Post` records exists in a third CDMR record. The CDMR record describes the entity type, record name, and Core Data relationship between the `Tag` and `Post`.

```
<CKRecord: 0x7f9ca1301780; recordID=EE64F478-A761-4049-B559-853457ABA997:
(com.apple.coredata.cloudkit.zone:__defaultOwner__), values={
    "CD_entityNames" = "Post:Tag";
    "CD_recordNames" = "CD_Post_UUID:CD_Tag_UUID";
    "CD_relationships" = "tags:posts";
}, recordType=CDMR>
```

The structure of a CDMR record is carefully designed to occupy the minimum necessary footprint, and to require the least effort to decode and work with, making it usable outside the Core Data framework.

### [Access CloudKit Objects](/documentation/coredata/reading-cloudkit-records-for-core-data#Access-CloudKit-Objects)

You can access a managed object’s [`CKRecord`](/documentation/CloudKit/CKRecord) directly through its associated context using `record(for:)` for a single record, or `records(for:)` for multiple records. To retrieve the record ID only, use `recordID(for:)`, or `recordIDs(for:)`.

Alternatively, use the class functions [`recordForManagedObjectID:`](/documentation/coredata/nspersistentcloudkitcontainer/recordformanagedobjectid:), [`recordsForManagedObjectIDs:`](/documentation/coredata/nspersistentcloudkitcontainer/recordsformanagedobjectids:), [`recordIDForManagedObjectID:`](/documentation/coredata/nspersistentcloudkitcontainer/recordidformanagedobjectid:), and `recordIDs(for:)` on [`NSPersistentCloudKitContainer`](/documentation/coredata/nspersistentcloudkitcontainer).

# Synchronizing a local store to the cloud | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   Synchronizing a local store to the cloud

Sample Code

# Synchronizing a local store to the cloud

Share data between a user’s devices and other iCloud users.

[Download](https://docs-assets.developer.apple.com/published/dbcc19631790/SynchronizingALocalStoreToTheCloud.zip)

## [Overview](/documentation/coredata/synchronizing-a-local-store-to-the-cloud#Overview)

### [Configure the sample code project](/documentation/coredata/synchronizing-a-local-store-to-the-cloud#Configure-the-sample-code-project)

Before you run the sample code project in Xcode, do the following:

1.  In Xcode’s Project navigator, select the project, and click the Signing & Capabilities tab.
    
2.  From the Team pop-up menu, choose your developer team.
    
3.  In the Bundle Identifier field, enter a new bundle ID for the CoreDataCloudKitDemo target. The bundle identifier for the project has an associated App ID, so you need a unique identifier to create your own App ID. Use a reverse-DNS format for your identifier, as [Preparing Your App for Distribution](/documentation/Xcode/preparing-your-app-for-distribution) describes.
    
4.  In the iCloud Capability Section, click the + button to create a new iCloud Container, or click a the checkbox next to an existing container you would like to use.
    
5.  Select the CoreDataCloudKitDemoUnitTests target and use the Team pop-up menu to choose your developer team.
    

Xcode automatically generates provisioning profiles as needed. You can now build and run the CoreDataCloudKitDemo app or tests.

### [Run the CoreDataCloudKitDemo app](/documentation/coredata/synchronizing-a-local-store-to-the-cloud#Run-the-CoreDataCloudKitDemo-app)

1.  Select the CoreDataCloudKitDemo scheme.
    
2.  Choose a destination. The CoreDataCloudKitDemo app supports the following destinations:
    
    -   Any iOS simulator
        
    -   Any iOS device
        
    -   My Mac (Mac Catalyst)
        

### [Configuration options](/documentation/coredata/synchronizing-a-local-store-to-the-cloud#Configuration-options)

To facilitate testing, the app supports the following configuration options the `AppDelegate` class parses into properties:

-   `-CDCKDTesting`
    
    -   Set to `1` to store files in the special directory `TestStores`, so that tests don’t overwrite user data.
        
-   `-CDCKDAllowCloudKitSync`
    
    -   Set to `0` to disable CloudKit sync during testing.
        
-   `com.apple.CoreData.ConcurrencyDebug`
    -   Enable Core Data multithreading assertions to verify all Core Data operations use the correct queue.
# NSPersistentCloudKitContainer

A container that encapsulates the Core Data stack in your app, and mirrors select persistent stores to a CloudKit private database.

```
class NSPersistentCloudKitContainer
```


## [Overview](/documentation/coredata/nspersistentcloudkitcontainer#overview)

[`NSPersistentCloudKitContainer`](/documentation/coredata/nspersistentcloudkitcontainer) is a subclass of [`NSPersistentContainer`](/documentation/coredata/nspersistentcontainer) capable of managing both CloudKit-backed and noncloud stores.

By default, [`NSPersistentCloudKitContainer`](/documentation/coredata/nspersistentcloudkitcontainer) contains a single store description, which Core Data assigns to the first CloudKit container identifier in an app’s entitlements. Use [`NSPersistentCloudKitContainerOptions`](/documentation/coredata/nspersistentcloudkitcontaineroptions) to customize this behavior or create additional store descriptions with backing by different containers.

For more information about setting up multiple stores, see [Setting Up Core Data with CloudKit](/documentation/coredata/setting-up-core-data-with-cloudkit).

## [Topics](/documentation/coredata/nspersistentcloudkitcontainer#topics)

### [Checking Permissions](/documentation/coredata/nspersistentcloudkitcontainer#Checking-Permissions)

[`func canUpdateRecord(forManagedObjectWith: NSManagedObjectID) -> Bool`](/documentation/coredata/nspersistentcloudkitcontainer/canupdaterecord\(formanagedobjectwith:\))

Returns a Boolean value that indicates whether the user can modify the managed object’s underlying CloudKit record.

[`func canDeleteRecord(forManagedObjectWith: NSManagedObjectID) -> Bool`](/documentation/coredata/nspersistentcloudkitcontainer/candeleterecord\(formanagedobjectwith:\))

Returns a Boolean value that indicates whether the user can delete the managed object’s underlying CloudKit record.

[`func canModifyManagedObjects(in: NSPersistentStore) -> Bool`](/documentation/coredata/nspersistentcloudkitcontainer/canmodifymanagedobjects\(in:\))

Returns a Boolean value that indicates whether the user can modify the specified persistent store.

### [Sharing Objects](/documentation/coredata/nspersistentcloudkitcontainer#Sharing-Objects)

[

Accepting Share Invitations in a SwiftUI App](/documentation/coredata/accepting-share-invitations-in-a-swiftui-app)

Adapt your app to use UIKit’s application and scene delegates so it can process CloudKit share invitations.

### [Promoting Your Schema](/documentation/coredata/nspersistentcloudkitcontainer#Promoting-Your-Schema)

[`func initializeCloudKitSchema(options: NSPersistentCloudKitContainerSchemaInitializationOptions) throws`](/documentation/coredata/nspersistentcloudkitcontainer/initializecloudkitschema\(options:\))

Creates the CloudKit schema for all stores in the container that manage a CloudKit database.

[`struct NSPersistentCloudKitContainerSchemaInitializationOptions`](/documentation/coredata/nspersistentcloudkitcontainerschemainitializationoptions)

Options that control the behavior when promoting the container’s schema to CloudKit.

### [Monitoring Container Events](/documentation/coredata/nspersistentcloudkitcontainer#Monitoring-Container-Events)

[`class Event`](/documentation/coredata/nspersistentcloudkitcontainer/event)

An object that represents activity in a persistent CloudKit container.

[`enum EventType`](/documentation/coredata/nspersistentcloudkitcontainer/eventtype)

The type of event in a persistent CloudKit container, either setup, import, or export.

[`class NSPersistentCloudKitContainerEventRequest`](/documentation/coredata/nspersistentcloudkitcontainereventrequest)

A request to fetch setup, import, or export events in a persistent CloudKit container.

[`class NSPersistentCloudKitContainerEventResult`](/documentation/coredata/nspersistentcloudkitcontainereventresult)

The result of a request to fetch persistent CloudKit container events.

[`class let eventChangedNotification: NSNotification.Name`](/documentation/coredata/nspersistentcloudkitcontainer/eventchangednotification)

A notification that contains details about an event in a persistent CloudKit container.

[`class let eventNotificationUserInfoKey: String`](/documentation/coredata/nspersistentcloudkitcontainer/eventnotificationuserinfokey)

The user info dictionary key for the persistent CloudKit container event.

## [Relationships](/documentation/coredata/nspersistentcloudkitcontainer#relationships)

### [Inherits From](/documentation/coredata/nspersistentcloudkitcontainer#inherits-from)

-   [`NSPersistentContainer`](/documentation/coredata/nspersistentcontainer)

### [Conforms To](/documentation/coredata/nspersistentcloudkitcontainer#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

# canUpdateRecord(forManagedObjectWith:) | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [NSPersistentCloudKitContainer](/documentation/coredata/nspersistentcloudkitcontainer)
-   canUpdateRecord(forManagedObjectWith:)

Instance Method

# canUpdateRecord(forManagedObjectWith:)

Returns a Boolean value that indicates whether the user can modify the managed object’s underlying CloudKit record.

```
func canUpdateRecord(forManagedObjectWith objectID: NSManagedObjectID) -> Bool
```

## [Parameters](/documentation/coredata/nspersistentcloudkitcontainer/canupdaterecord\(formanagedobjectwith:\)#parameters)

`objectID`

The ID of the managed object.

## [Return Value](/documentation/coredata/nspersistentcloudkitcontainer/canupdaterecord\(formanagedobjectwith:\)#return-value)

[`true`](/documentation/swift/true) if the user can modify the CloudKit record; otherwise, [`false`](/documentation/swift/false).

## [Discussion](/documentation/coredata/nspersistentcloudkitcontainer/canupdaterecord\(formanagedobjectwith:\)#Discussion)

This method returns [`true`](/documentation/swift/true) if [`canModifyManagedObjects(in:)`](/documentation/coredata/nspersistentcloudkitcontainer/canmodifymanagedobjects\(in:\)) returns [`true`](/documentation/swift/true) and any of the following conditions are true:

-   `objectID` is a temporary object identifier.
    
-   The persistent store that contains the managed object isn’t using CloudKit.
    
-   The persistent store manages the user’s private database.
    
-   The persistent store manages the public database, and the user owns the underlying record or Core Data has yet to save the managed object to iCloud.
    
-   The persistent store manages the shared database, and the user has the necessary permissions to update the managed object’s underlying record. For more information, see [`CKShare.ParticipantPermission`](/documentation/CloudKit/CKShare/ParticipantPermission).
# canDeleteRecord(forManagedObjectWith:) | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [NSPersistentCloudKitContainer](/documentation/coredata/nspersistentcloudkitcontainer)
-   canDeleteRecord(forManagedObjectWith:)

Instance Method

# canDeleteRecord(forManagedObjectWith:)

Returns a Boolean value that indicates whether the user can delete the managed object’s underlying CloudKit record.

```
func canDeleteRecord(forManagedObjectWith objectID: NSManagedObjectID) -> Bool
```

## [Parameters](/documentation/coredata/nspersistentcloudkitcontainer/candeleterecord\(formanagedobjectwith:\)#parameters)

`objectID`

The ID of the managed object.

## [Return Value](/documentation/coredata/nspersistentcloudkitcontainer/candeleterecord\(formanagedobjectwith:\)#return-value)

[`true`](/documentation/swift/true) if the user can delete the CloudKit record; otherwise, [`false`](/documentation/swift/false).

## [Discussion](/documentation/coredata/nspersistentcloudkitcontainer/candeleterecord\(formanagedobjectwith:\)#Discussion)

This method returns [`true`](/documentation/swift/true) if [`canModifyManagedObjects(in:)`](/documentation/coredata/nspersistentcloudkitcontainer/canmodifymanagedobjects\(in:\)) returns [`true`](/documentation/swift/true) and any of the following conditions are true:

-   `objectID` is a temporary object identifier.
    
-   The persistent store that contains the managed object isn’t using CloudKit.
    
-   The persistent store manages the user’s private database.
    
-   The persistent store manages the public database, and the user owns the underlying record or Core Data has yet to save the managed object to iCloud.
    
-   The persistent store manages the shared database, and the user has the necessary permissions to delete the managed object’s underlying record. For more information, see [`CKShare.ParticipantPermission`](/documentation/CloudKit/CKShare/ParticipantPermission).

# canModifyManagedObjects(in:) | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [NSPersistentCloudKitContainer](/documentation/coredata/nspersistentcloudkitcontainer)
-   canModifyManagedObjects(in:)

Instance Method

# canModifyManagedObjects(in:)

Returns a Boolean value that indicates whether the user can modify the specified persistent store.

```
func canModifyManagedObjects(in store: NSPersistentStore) -> Bool
```

## [Parameters](/documentation/coredata/nspersistentcloudkitcontainer/canmodifymanagedobjects\(in:\)#parameters)

`store`

The persistent store.

## [Return Value](/documentation/coredata/nspersistentcloudkitcontainer/canmodifymanagedobjects\(in:\)#return-value)

[`true`](/documentation/swift/true) if the user can modify records in the persistent store’s CloudKit database; otherwise, [`false`](/documentation/swift/false).

## [Discussion](/documentation/coredata/nspersistentcloudkitcontainer/canmodifymanagedobjects\(in:\)#Discussion)

Use this method to determine whether the user is able to write any records to the CloudKit database. To find out if the user can modify a specific object, use the [`canUpdateRecord(forManagedObjectWith:)`](/documentation/coredata/nspersistentcloudkitcontainer/canupdaterecord\(formanagedobjectwith:\)) and [`canDeleteRecord(forManagedObjectWith:)`](/documentation/coredata/nspersistentcloudkitcontainer/candeleterecord\(formanagedobjectwith:\)) methods instead.

This method always returns [`true`](/documentation/swift/true) for persistent stores that manage the user’s private CloudKit database.

# Accepting Share Invitations in a SwiftUI App | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [NSPersistentCloudKitContainer](/documentation/coredata/nspersistentcloudkitcontainer)
-   Accepting Share Invitations in a SwiftUI App

Article

# Accepting Share Invitations in a SwiftUI App

Adapt your app to use UIKit’s application and scene delegates so it can process CloudKit share invitations.

## [Overview](/documentation/coredata/accepting-share-invitations-in-a-swiftui-app#overview)

When the user accepts an invitation to participate in a CloudKit share, the system passes that share’s metadata to your app’s scene delegate for processing. To receive this metadata in a SwiftUI app:

1.  Add a scene delegate — an object that conforms to [`UIWindowSceneDelegate`](/documentation/UIKit/UIWindowSceneDelegate) — that’s responsible for passing the accepted share metadata to your app’s persistent container.
    
2.  Add an application delegate — an object that conforms to [`UIApplicationDelegate`](/documentation/UIKit/UIApplicationDelegate) — that configures new scenes to use your custom scene delegate class.
    
3.  Modify your app’s [`App`](/documentation/SwiftUI/App) structure to use [`UIApplicationDelegateAdaptor`](/documentation/SwiftUI/UIApplicationDelegateAdaptor) to initialize and manage an application delegate at runtime.
    

### [Add a Scene Delegate to Process Share Invitations](/documentation/coredata/accepting-share-invitations-in-a-swiftui-app#Add-a-Scene-Delegate-to-Process-Share-Invitations)

In response to the user accepting a CloudKit share invitation, the system routes that action, for processing, to the delegate of the app’s active scene. SwiftUI apps don’t contain a scene delegate by default, but you can add one, and use it to implement the [`windowScene(_:userDidAcceptCloudKitShareWith:)`](/documentation/UIKit/UIWindowSceneDelegate/windowScene\(_:userDidAcceptCloudKitShareWith:\)) method. Your implementation is responsible for passing the provided share metadata to your app’s persistent container for processing.

To create the scene delegate, right-click your project in Xcode’s Project navigator and select New File. Choose the Swift File template and name the file `SceneDelegate.swift`. Open the new file in Xcode’s source editor and define the `SceneDelegate` class as a subclass of [`UIResponder`](/documentation/UIKit/UIResponder) that adopts the [`UIWindowSceneDelegate`](/documentation/UIKit/UIWindowSceneDelegate) protocol. Within this class, add your implementation of [`windowScene(_:userDidAcceptCloudKitShareWith:)`](/documentation/UIKit/UIWindowSceneDelegate/windowScene\(_:userDidAcceptCloudKitShareWith:\)), as shown in the following example.

```
class SceneDelegate: UIResponder, UIWindowSceneDelegate {
    func windowScene(_ windowScene: UIWindowScene,
        userDidAcceptCloudKitShareWith cloudKitShareMetadata: CKShare.Metadata) {
        
        let stack = CoreDataStack.shared


        // Get references to the app's persistent container 
        // and shared persistent store.
        let container = stack.persistentContainer
        let store = stack.sharedPersistentStore


        // Tell the container to accept the specified share, adding
        // the shared objects to the shared persistent store.
       container.acceptShareInvitations(from: [cloudKitShareMetadata],
                                        into: store,
                                        completion: nil)
    }
}
```

The example above uses a `CoreDataStack` object that manages the initialization of the persistent container, and the private and shared persistent stores. For a reference implementation, see the sample code project [Synchronizing a local store to the cloud](/documentation/coredata/synchronizing-a-local-store-to-the-cloud).

### [Add an Application Delegate to Configure New Scenes](/documentation/coredata/accepting-share-invitations-in-a-swiftui-app#Add-an-Application-Delegate-to-Configure-New-Scenes)

Before an app connects a new scene, the system asks the application delegate to provide the configuration for the new scene, including the class to use as its own delegate. By providing this configuration, your app can use your custom delegate implementation and correctly process accepted CloudKit share invitations. Because SwiftUI apps don’t include an application delegate, you need to add one to your app’s target.

Right-click your project in Xcode’s Project navigator and select New File. Choose the Swift File template and name the file `AppDelegate.swift`. Open the new file in Xcode’s source editor and add the following code, which configures each new scene to use the custom `SceneDelegate` class from the previous section.

```
class AppDelegate: UIResponder, UIApplicationDelegate {
    func application(_ application: UIApplication, configurationForConnecting
        connectingSceneSession: UISceneSession,
        options: UIScene.ConnectionOptions) -> UISceneConfiguration {


        // Create a scene configuration object for the
        // specified session role.
        let config = UISceneConfiguration(name: nil,
            sessionRole: connectingSceneSession.role)


        // Set the configuration's delegate class to the
        // scene delegate that implements the share
        // acceptance method.
        config.delegateClass = SceneDelegate.self


        return config
    }
}
```

For more information on scene configuration, see [`application(_:configurationForConnecting:options:)`](/documentation/UIKit/UIApplicationDelegate/application\(_:configurationForConnecting:options:\)).

### [Modify Your App to Utilize the Application Delegate](/documentation/coredata/accepting-share-invitations-in-a-swiftui-app#Modify-Your-App-to-Utilize-the-Application-Delegate)

After you add the scene and application delegates to your app’s target, use the [`UIApplicationDelegateAdaptor`](/documentation/SwiftUI/UIApplicationDelegateAdaptor) property wrapper to instruct your app’s top-level object — the structure that adopts SwiftUI’s [`App`](/documentation/SwiftUI/App) protocol — to initialize and manage an instance of the application delegate at runtime, as shown in the following example.

```
@main
struct SharingExample: App {
    // Instruct SwiftUI to use the custom AppDelegate class 
    // as the app's application delegate.
    @UIApplicationDelegateAdaptor var appDelegate: AppDelegate


    var body: some Scene {
        WindowGroup {
            ContentView()
                .environment(\.managedObjectContext,
                    CoreDataStack.shared.persistentContainer.viewContext)
        }
    }
}
```

With this change in place, whenever the user accepts a CloudKit share invitation, SwiftUI notifies the active scene’s delegate where you can process the accepted invitation accordingly.

# initializeCloudKitSchema(options:) | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [NSPersistentCloudKitContainer](/documentation/coredata/nspersistentcloudkitcontainer)
-   initializeCloudKitSchema(options:)

Instance Method

# initializeCloudKitSchema(options:)

Creates the CloudKit schema for all stores in the container that manage a CloudKit database.

```
func initializeCloudKitSchema(options: NSPersistentCloudKitContainerSchemaInitializationOptions = []) throws
```

## [Parameters](/documentation/coredata/nspersistentcloudkitcontainer/initializecloudkitschema\(options:\)#parameters)

`options`

The options to use when creating the CloudKit schema.

## [Discussion](/documentation/coredata/nspersistentcloudkitcontainer/initializecloudkitschema\(options:\)#Discussion)

To create the schema, this method creates a set of representative [`CKRecord`](/documentation/CloudKit/CKRecord) instances for all stores in the container that use Core Data with CloudKit, and uploads them to CloudKit. These records have a representative value for every field Core Data might serialize for the specified managed object model. After successfully uploading the records, the schema is visible in the CloudKit Dashboard and the container deletes the representative records.
# NSPersistentCloudKitContainerSchemaInitializationOptions | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   NSPersistentCloudKitContainerSchemaInitializationOptions

Structure

# NSPersistentCloudKitContainerSchemaInitializationOptions

Options that control the behavior when promoting the container’s schema to CloudKit.

```
struct NSPersistentCloudKitContainerSchemaInitializationOptions
```

## [Topics](/documentation/coredata/nspersistentcloudkitcontainerschemainitializationoptions#topics)

### [Constants](/documentation/coredata/nspersistentcloudkitcontainerschemainitializationoptions#Constants)

[`static var dryRun: NSPersistentCloudKitContainerSchemaInitializationOptions`](/documentation/coredata/nspersistentcloudkitcontainerschemainitializationoptions/dryrun)

A flag that indicates the container validates the model and generates the records, but doesn’t upload them to CloudKit.

[`static var printSchema: NSPersistentCloudKitContainerSchemaInitializationOptions`](/documentation/coredata/nspersistentcloudkitcontainerschemainitializationoptions/printschema)

Prints the generated records to the console.

### [Initializers](/documentation/coredata/nspersistentcloudkitcontainerschemainitializationoptions#Initializers)

[`init(rawValue: UInt)`](/documentation/coredata/nspersistentcloudkitcontainerschemainitializationoptions/init\(rawvalue:\))

Creates the schema initialization options using the specified raw value.

## [Relationships](/documentation/coredata/nspersistentcloudkitcontainerschemainitializationoptions#relationships)

### [Conforms To](/documentation/coredata/nspersistentcloudkitcontainerschemainitializationoptions#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`ExpressibleByArrayLiteral`](/documentation/Swift/ExpressibleByArrayLiteral)
-   [`OptionSet`](/documentation/Swift/OptionSet)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)
-   [`SetAlgebra`](/documentation/Swift/SetAlgebra)
# dryRun | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [NSPersistentCloudKitContainerSchemaInitializationOptions](/documentation/coredata/nspersistentcloudkitcontainerschemainitializationoptions)
-   dryRun

Type Property

# dryRun

A flag that indicates the container validates the model and generates the records, but doesn’t upload them to CloudKit.

```
static var dryRun: NSPersistentCloudKitContainerSchemaInitializationOptions { get }
```

## [Discussion](/documentation/coredata/nspersistentcloudkitcontainerschemainitializationoptions/dryrun#Discussion)

This option is useful for unit testing to ensure your managed object model is valid for use with CloudKit.

# printSchema | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [NSPersistentCloudKitContainerSchemaInitializationOptions](/documentation/coredata/nspersistentcloudkitcontainerschemainitializationoptions)
-   printSchema

Type Property

# printSchema

Prints the generated records to the console.

```
static var printSchema: NSPersistentCloudKitContainerSchemaInitializationOptions { get }
```


# NSPersistentCloudKitContainer.Event | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [NSPersistentCloudKitContainer](/documentation/coredata/nspersistentcloudkitcontainer)
-   NSPersistentCloudKitContainer.Event

Class

# NSPersistentCloudKitContainer.Event

An object that represents activity in a persistent CloudKit container.

```
class Event
```

## [Topics](/documentation/coredata/nspersistentcloudkitcontainer/event#topics)

### [Inspecting Event Properties](/documentation/coredata/nspersistentcloudkitcontainer/event#Inspecting-Event-Properties)

[`var type: NSPersistentCloudKitContainer.EventType`](/documentation/coredata/nspersistentcloudkitcontainer/event/type)

The type of event, either setup, import, or export.

[`var identifier: UUID`](/documentation/coredata/nspersistentcloudkitcontainer/event/identifier)

A unique identifier for the event in a container.

[`var storeIdentifier: String`](/documentation/coredata/nspersistentcloudkitcontainer/event/storeidentifier)

The associated store identifier in the container for the event.

[`var succeeded: Bool`](/documentation/coredata/nspersistentcloudkitcontainer/event/succeeded)

A Boolean value that indicates whether the operation the event represents is successful.

[`var startDate: Date`](/documentation/coredata/nspersistentcloudkitcontainer/event/startdate)

The start date of the operation that the event represents.

[`var endDate: Date?`](/documentation/coredata/nspersistentcloudkitcontainer/event/enddate)

The end date of the operation that the event represents.

[`var error: (any Error)?`](/documentation/coredata/nspersistentcloudkitcontainer/event/error)

An error that indicates why an operation fails.

## [Relationships](/documentation/coredata/nspersistentcloudkitcontainer/event#relationships)

### [Inherits From](/documentation/coredata/nspersistentcloudkitcontainer/event#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/coredata/nspersistentcloudkitcontainer/event#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSCopying`](/documentation/Foundation/NSCopying)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)

# NSPersistentCloudKitContainer.EventType | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [NSPersistentCloudKitContainer](/documentation/coredata/nspersistentcloudkitcontainer)
-   NSPersistentCloudKitContainer.EventType

Enumeration

# NSPersistentCloudKitContainer.EventType

The type of event in a persistent CloudKit container, either setup, import, or export.

```
enum EventType
```

## [Topics](/documentation/coredata/nspersistentcloudkitcontainer/eventtype#topics)

### [Event Types](/documentation/coredata/nspersistentcloudkitcontainer/eventtype#Event-Types)

[`case setup`](/documentation/coredata/nspersistentcloudkitcontainer/eventtype/setup)

An event the persistent CloudKit container generates when setting up a store.

[`` case `import` ``](/documentation/coredata/nspersistentcloudkitcontainer/eventtype/import)

An event the persistent CloudKit container generates when importing records into a store.

[`case export`](/documentation/coredata/nspersistentcloudkitcontainer/eventtype/export)

An event the persistent CloudKit container generates when exporting managed objects from a store.

### [Initializers](/documentation/coredata/nspersistentcloudkitcontainer/eventtype#Initializers)

[`init?(rawValue: Int)`](/documentation/coredata/nspersistentcloudkitcontainer/eventtype/init\(rawvalue:\))

## [Relationships](/documentation/coredata/nspersistentcloudkitcontainer/eventtype#relationships)

### [Conforms To](/documentation/coredata/nspersistentcloudkitcontainer/eventtype#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

# NSPersistentCloudKitContainerEventRequest | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   NSPersistentCloudKitContainerEventRequest

Class

# NSPersistentCloudKitContainerEventRequest

A request to fetch setup, import, or export events in a persistent CloudKit container.

```
class NSPersistentCloudKitContainerEventRequest
```

## [Topics](/documentation/coredata/nspersistentcloudkitcontainereventrequest#topics)

### [Fetching Events](/documentation/coredata/nspersistentcloudkitcontainereventrequest#Fetching-Events)

[`class func fetchEvents(after: Date) -> Self`](/documentation/coredata/nspersistentcloudkitcontainereventrequest/fetchevents\(after:\)-5izg7)

Creates a fetch request for events after a specified date from a persistent CloudKit container.

[`class func fetchEvents(after: NSPersistentCloudKitContainer.Event?) -> Self`](/documentation/coredata/nspersistentcloudkitcontainereventrequest/fetchevents\(after:\)-3yfp)

Creates a fetch request for events that occur after a specified event from a persistent CloudKit container.

[`class func fetchEvents(matchingFetch: NSFetchRequest<any NSFetchRequestResult>) -> Self`](/documentation/coredata/nspersistentcloudkitcontainereventrequest/fetchevents\(matchingfetch:\))

Creates a fetch request for events that match a specified fetch request from a persistent CloudKit container.

[`class func fetchForEvents() -> NSFetchRequest<any NSFetchRequestResult>`](/documentation/coredata/nspersistentcloudkitcontainereventrequest/fetchforevents\(\))

Creates a fetch request for all events in a persistent CloudKit container.

[`var resultType: NSPersistentCloudKitContainerEventResult.ResultType`](/documentation/coredata/nspersistentcloudkitcontainereventrequest/resulttype)

The type of result that the request returns.

## [Relationships](/documentation/coredata/nspersistentcloudkitcontainereventrequest#relationships)

### [Inherits From](/documentation/coredata/nspersistentcloudkitcontainereventrequest#inherits-from)

-   [`NSPersistentStoreRequest`](/documentation/coredata/nspersistentstorerequest)

### [Conforms To](/documentation/coredata/nspersistentcloudkitcontainereventrequest#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSCopying`](/documentation/Foundation/NSCopying)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)

# NSPersistentCloudKitContainerEventResult | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   NSPersistentCloudKitContainerEventResult

Class

# NSPersistentCloudKitContainerEventResult

The result of a request to fetch persistent CloudKit container events.

```
class NSPersistentCloudKitContainerEventResult
```

## [Topics](/documentation/coredata/nspersistentcloudkitcontainereventresult#topics)

### [Handling Event Results](/documentation/coredata/nspersistentcloudkitcontainereventresult#Handling-Event-Results)

[`var result: Any?`](/documentation/coredata/nspersistentcloudkitcontainereventresult/result)

The result of the persistent CloudKit container event request, which the result type determines.

[`var resultType: NSPersistentCloudKitContainerEventResult.ResultType`](/documentation/coredata/nspersistentcloudkitcontainereventresult/resulttype-swift.property)

The type of result that the CloudKit container event fetch request returns.

[`enum ResultType`](/documentation/coredata/nspersistentcloudkitcontainereventresult/resulttype-swift.enum)

The types of results from a persistent CloudKit container event fetch request.

## [Relationships](/documentation/coredata/nspersistentcloudkitcontainereventresult#relationships)

### [Inherits From](/documentation/coredata/nspersistentcloudkitcontainereventresult#inherits-from)

-   [`NSPersistentStoreResult`](/documentation/coredata/nspersistentstoreresult)

### [Conforms To](/documentation/coredata/nspersistentcloudkitcontainereventresult#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)

# eventChangedNotification | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [NSPersistentCloudKitContainer](/documentation/coredata/nspersistentcloudkitcontainer)
-   eventChangedNotification

Type Property

# eventChangedNotification

A notification that contains details about an event in a persistent CloudKit container.

```
class let eventChangedNotification: NSNotification.Name
```

# eventNotificationUserInfoKey | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   [NSPersistentCloudKitContainer](/documentation/coredata/nspersistentcloudkitcontainer)
-   eventNotificationUserInfoKey

Type Property

# eventNotificationUserInfoKey

The user info dictionary key for the persistent CloudKit container event.

```
class let eventNotificationUserInfoKey: String
```

# NSPersistentCloudKitContainerOptions | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   NSPersistentCloudKitContainerOptions

Class

# NSPersistentCloudKitContainerOptions

An object that customizes how a store description aligns with a CloudKit database.

```
class NSPersistentCloudKitContainerOptions
```

## [Overview](/documentation/coredata/nspersistentcloudkitcontaineroptions#overview)

Use [`NSPersistentCloudKitContainerOptions`](/documentation/coredata/nspersistentcloudkitcontaineroptions) to customize the behavior of an [`NSPersistentCloudKitContainer`](/documentation/coredata/nspersistentcloudkitcontainer) or to create additional store descriptions that sync to other containers.

For more information about setting up multiple stores, see [Setting Up Core Data with CloudKit](/documentation/coredata/setting-up-core-data-with-cloudkit).

## [Topics](/documentation/coredata/nspersistentcloudkitcontaineroptions#topics)

### [Creating Container Options](/documentation/coredata/nspersistentcloudkitcontaineroptions#Creating-Container-Options)

[`init(containerIdentifier: String)`](/documentation/coredata/nspersistentcloudkitcontaineroptions/init\(containeridentifier:\))

Initializes container options using the given CloudKit container identifier.

[`var containerIdentifier: String`](/documentation/coredata/nspersistentcloudkitcontaineroptions/containeridentifier)

The identifier of the CloudKit container associated with a given store description.

[`var databaseScope: CKDatabase.Scope`](/documentation/coredata/nspersistentcloudkitcontaineroptions/databasescope-4c72t)

The database scope — public, private, or shared — to use for a specified store in a persistent CloudKit container.

## [Relationships](/documentation/coredata/nspersistentcloudkitcontaineroptions#relationships)

### [Inherits From](/documentation/coredata/nspersistentcloudkitcontaineroptions#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/coredata/nspersistentcloudkitcontaineroptions#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)

# Sharing Core Data objects between iCloud users | Apple Developer Documentation

-   [Core Data](/documentation/coredata)
-   Sharing Core Data objects between iCloud users

Sample Code

# Sharing Core Data objects between iCloud users

Use Core Data and CloudKit to synchronize data between devices of an iCloud user and share data between different iCloud users.

[Download](https://docs-assets.developer.apple.com/published/d3e48d795efa/SharingCoreDataObjectsBetweenICloudUsers.zip)

## [Overview](/documentation/coredata/sharing-core-data-objects-between-icloud-users#Overview)

More and more people own multiple devices and use them for digital asset sharing and collaboration. They expect seamless data synchronization and sharing experiences with robust privacy and security features. Apps can support such use cases by implementing a data-sharing flow using Core Data and CloudKit.

This sample code project demonstrates how to use Core Data and CloudKit to share photos between iCloud users. People who share photos, called _owners_, can create a share, send an invitation, manage the permissions, and stop the sharing. People who accept the share, called _participants_, can view and edit the photos, or stop participating in the share.

### [Configure the sample code project](/documentation/coredata/sharing-core-data-objects-between-icloud-users#Configure-the-sample-code-project)

Open the sample code project in Xcode. Before building it, perform the following steps:

1.  In the Signing & Capabilities pane of the `CoreDataCloudKitShare` target, set the developer team to let Xcode automatically manage the provisioning profile. See \[Assign a project to a team\](doc://com.apple.documentation/xcode/preparing-your-app-for-distribution#Assign-the-project-to-a-team> for details.
    
2.  In the iCloud section, select an empty iCloud container from the Containers list. If there isn’t an empty container, click the Add button (+), enter a container name, and click OK to let Xcode create the container and associate it with the app. An iCloud container identifier is case-sensitive, and must be unique and begin with “`iCloud.`”.
    
3.  Specify the same team and iCloud container for all other targets.
    
4.  Specify the same iCloud container for the `gCloudKitContainerIdentifier` variable in `PersistenceController.swift`.
    
5.  In the Info pane of the `CoreDataCloudKitShareOnWatch` target, change the value of the `WKCompanionAppBundleIdentifier` key to `<The iOS app bundle ID>`.
    

To run the sample app on a device, configure the device as follows:

1.  Log in with an Apple ID. For an Apple Watch, open the Apple Watch app on the paired iPhone, log in at General > Apple ID, and confirm the Apple ID showing up in the Settings app on the watch.
    
2.  For an iOS device, confirm that iCloud is on for the app at Settings > Apple ID > iCloud > Apps Using iCloud.
    
3.  After running the app on the device, confirm that Allow Notifications is on for the app at Settings > Notifications. For an Apple Watch, use the Apple Watch app on the paired iPhone to confirm the settings.
    

For more information about the project configuration, see [Setting Up Core Data with CloudKit](/documentation/CoreData/setting-up-core-data-with-cloudkit).

### [Create the CloudKit schema](/documentation/coredata/sharing-core-data-objects-between-icloud-users#Create-the-CloudKit-schema)

CloudKit apps require a schema to declare the data types they use. When apps create a record in the CloudKit development environment, CloudKit automatically creates the record type if it doesn’t exist. In the production environment, CloudKit doesn’t have that capability, nor does it allow removing an existing record type or field, so after finalizing the schema, developers need to deploy it to the production environment. Without this step, apps that work in the production environment, like the ones people download from the App Store or TestFlight, can’t communicate with the CloudKit server. For more information, see [Deploying an iCloud Container’s Schema](/documentation/CloudKit/deploying-an-icloud-container-s-schema).

Apps that use [`NSPersistentCloudKitContainer`](/documentation/CoreData/NSPersistentCloudKitContainer) can call [`initializeCloudKitSchema(options:)`](/documentation/CoreData/NSPersistentCloudKitContainer/initializeCloudKitSchema\(options:\)) to create the CloudKit schema that matches their Core Data model, or keep it up to date every time their model changes. The method works by creating fake data for the record types and then deleting it, which can take some time and blocks the other CloudKit operations. Apps must not call it in the production environment or in the normal development process that doesn’t include model changes.

To create the CloudKit schema for this sample app, select `InitializeCloudKitSchema` from Xcode’s target menu and run the target. Having a target dedicated on CloudKit schema creation separates the `initializeCloudKitSchema(options:)` call from the normal flow. After running the target, use [CloudKit Console](http://icloud.developer.apple.com/dashboard/) to ensure each Core Data entity and attribute has a CloudKit counterpart. See [Reading CloudKit Records for Core Data](/documentation/CoreData/reading-cloudkit-records-for-core-data) for the mapping rules.

For apps that use the CloudKit public database, use CloudKit Console to manually add the `Queryable` index for the `recordName` field, and the `Queryable` and `Sortable` indexes for the `modifiedAt` field for all record types, including the `CDMR` type that Core Data generates to manage many-to-many relationships.

For more information, see [Creating a Core Data Model for CloudKit](/documentation/CoreData/creating-a-core-data-model-for-cloudkit).

### [Try out the sharing flow](/documentation/coredata/sharing-core-data-objects-between-icloud-users#Try-out-the-sharing-flow)

To create and share a photo using the sample app, follow these steps:

1.  Prepare two iOS devices, A and B, and log in to each device with a different Apple ID.
    
2.  Use Xcode to build and run the sample app on the devices.
    
3.  On device A, tap the Add button (+) to add a photo to the Core Data store.
    
4.  Touch and hold the photo to display the context menu, and then tap New Share to present the sharing UI.
    
5.  Follow the UI to send a link to the Apple ID on device B. Use iMessage if you can because it’s easier to set up.
    
6.  After receiving the link on device B, tap it to accept and open the share, which launches the sample app and shows the photo.
    

To discover more features of the sample app:

-   On device A, add another photo, touch and hold it, tap Add to Share, and then tap the trailing icon of the share. The photo soon appears on device B.
    
-   On device B, touch and hold the photo, tap Participants, and then tap the Remove Me icon to stop the participation. The photo disappears.
    
-   Tap the Manage Shares icon, and then tap a trailing icon of the share to present and use the sharing management UI.
    

### [Set up the Core Data stack](/documentation/coredata/sharing-core-data-objects-between-icloud-users#Set-up-the-Core-Data-stack)

Every CloudKit container has a [private database](/documentation/CloudKit/CKContainer/privateCloudDatabase) and a [shared database](/documentation/CloudKit/CKContainer/sharedCloudDatabase). To mirror both of them, the sample app sets up a Core Data stack with two stores, sets one store’s [database scope](/documentation/CoreData/NSPersistentCloudKitContainerOptions/databaseScope-4c72t) to `.private` and the other to `.shared`, and then uses [`affectedStores`](/documentation/CoreData/NSFetchRequest/affectedStores) or [`assign(_:to:)`](/documentation/CoreData/NSManagedObjectContext/assign\(_:to:\)) to specify the target store for data fetching or saving.

When setting up the store description, the sample app enables [persistent history](/documentation/CoreData/persistent-history) tracking and turns on remote change notifications by setting the `NSPersistentHistoryTrackingKey` and `NSPersistentStoreRemoteChangeNotificationPostOptionKey` options to `true`. Core Data relies on the persistent history to track the store changes, and the sample app updates its UI when remote changes occur.

```
privateStoreDescription.setOption(true as NSNumber, forKey: NSPersistentHistoryTrackingKey)
privateStoreDescription.setOption(true as NSNumber, forKey: NSPersistentStoreRemoteChangeNotificationPostOptionKey)
```

To synchronize data through CloudKit, apps need to use the same CloudKit container. The sample app explicitly specifies the same container for its iOS, macOS, and watchOS apps when setting up the CloudKit container options.

```
let cloudKitContainerOptions = NSPersistentCloudKitContainerOptions(containerIdentifier: gCloudKitContainerIdentifier)
```

### [Share a Core Data object](/documentation/coredata/sharing-core-data-objects-between-icloud-users#Share-a-Core-Data-object)

Sharing a Core Data object between iCloud users includes creating a share ([`CKShare`](/documentation/CloudKit/CKShare)) from the owner side, accepting the sharing invitation from the participant side, and managing the share from both sides. Owners can stop their sharing or change the share permission. Participants can stop their participation. The sample app uses the following system sharing UI to implement the flow:

-   It uses [`ShareLink`](/documentation/SwiftUI/ShareLink) to start a new sharing and send the invitation. (Non-SwiftUI apps use [`UIActivityViewController`](/documentation/UIKit/UIActivityViewController) or [`NSSharingServicePicker`](/documentation/AppKit/NSSharingServicePicker).)
    
-   It uses [`UICloudSharingController`](/documentation/UIKit/UICloudSharingController) or [`NSSharingService`](/documentation/AppKit/NSSharingService) to manage a share. (Apps that use the Shared with You framework can use [`SWCollaborationView`](/documentation/SharedWithYou/SWCollaborationView) if the UI is appropriate. For more information, see [Adding shared content collaboration to your app](/documentation/SharedWithYou/adding-shared-content-collaboration-to-your-app).)
    

`ShareLink` requires the sharing object be [`Transferable`](/documentation/CoreTransferable/Transferable). The `Photo` class in this sample conforms to the protocol by implementing [`transferRepresentation`](/documentation/CoreTransferable/Transferable/transferRepresentation) to provide a [`CKShareTransferRepresentation`](/documentation/CloudKit/CKShareTransferRepresentation) instance, which is based on a new share it creates by calling [`share(_:to:completion:)`](/documentation/coredata/nspersistentcloudkitcontainer/3746834-share).

`NSPersistentCloudKitContainer` uses CloudKit zone sharing to share objects. Each share has its own record zone on the CloudKit server. CloudKit has a limit on how many record zones a database can have. To avoid reaching the limit over time, the sample app provides an option for users to share an object by adding it to an existing share, as the following example shows:

```
func shareObject(_ unsharedObject: NSManagedObject, to existingShare: CKShare?,
                 completionHandler: ((_ share: CKShare?, _ error: Error?) -> Void)? = nil)
```

The system sharing UI may change the share and save it directly to the CloudKit server. Since iOS 16.4, iPadOS 16.4, macOS 13.3, and watchOS 9.4, `NSPersistentCloudKitContainer` automatically observes the changes and updates the share it maintains. To support earlier systems that can’t upgrade to the latest versions, implement the relevant methods of [`UICloudSharingControllerDelegate`](/documentation/UIKit/UICloudSharingControllerDelegate) or [`NSCloudSharingServiceDelegate`](/documentation/AppKit/NSCloudSharingServiceDelegate) to update the share `NSPersistentCloudKitContainer` maintains.

The sample app doesn’t interact with the system sharing UI for other purposes. Apps that need to do so can create a [`CKSystemSharingUIObserver`](/documentation/CloudKit/CKSystemSharingUIObserver) object and provide a closure for [`systemSharingUIDidSaveShareBlock`](/documentation/cloudkit/cksystemsharinguiobserver/4027493-systemsharinguididsaveshareblock) and [`systemSharingUIDidStopSharingBlock`](/documentation/cloudkit/cksystemsharinguiobserver/4027494-systemsharinguididstopsharingblo) to detect and react to the changes. For systems where `CKSystemSharingUIObserver` is unavailable, use `UICloudSharingControllerDelegate` or `NSCloudSharingServiceDelegate`.

`NSPersistentCloudKitContainer` doesn’t support cross-share relationships. That is, it doesn’t allow relating objects associated with different shares. When sharing an object, `NSPersistentCloudKitContainer` moves the entire object graph, which includes the object and all its relationships, to the share’s record zone. When a participant stops participating in a share, `NSPersistentCloudKitContainer` deletes the object graph from the shared persistent store.

### [Detect relevant changes by consuming store persistent history](/documentation/coredata/sharing-core-data-objects-between-icloud-users#Detect-relevant-changes-by-consuming-store-persistent-history)

When importing data from CloudKit, `NSPersistentCloudKitContainer` records the changes on Core Data objects in the store’s persistent history, and triggers remote change notifications (`.NSPersistentStoreRemoteChange`) so apps can keep their state up-to-date. The sample app observes the notification and performs the following actions in the notification handler:

-   Gathers the relevant history transactions ([`NSPersistentHistoryTransaction`](/documentation/CoreData/NSPersistentHistoryTransaction)), and notifies the views when remote changes happen. The changes on shares don’t generate any transactions.
    
-   Merges the transactions to the `viewContext` of the persistent container, which triggers a SwiftUI update for the views that present photos. Views relevant to shares fetch the shares from the stores, and update the UI.
    
-   Detects the new tags, and handles duplicates, if necessary.
    

To process the persistent history more effectively, the sample app:

-   Maintains the token of the last transaction it consumes for each store, and uses it as the starting point of the next run.
    
-   Maintains a transaction author, and uses it to filter the transactions irrelevant to `NSPersistentCloudKitContainer`.
    
-   Only fetches and consumes the history of the relevant persistent store.
    

The following code sets up the history fetch request (`NSPersistentHistoryChangeRequest`):

```
let lastHistoryToken = historyToken(with: storeUUID)
let request = NSPersistentHistoryChangeRequest.fetchHistory(after: lastHistoryToken)
let historyFetchRequest = NSPersistentHistoryTransaction.fetchRequest!
historyFetchRequest.predicate = NSPredicate(format: "author != %@", TransactionAuthor.app)
request.fetchRequest = historyFetchRequest


if privatePersistentStore.identifier == storeUUID {
    request.affectedStores = [privatePersistentStore]
} else if sharedPersistentStore.identifier == storeUUID {
    request.affectedStores = [sharedPersistentStore]
}
```

The persistent history stays on the device as a part of the Core Data store, and accumulates over time. Apps that have a large data set can purge it. To do so, observe [`eventChangedNotification`](/documentation/CoreData/NSPersistentCloudKitContainer/eventChangedNotification) to determine the start date of the last successful `.export` event, and then purge the history that occurs sometime before that date. The _sometime_ needs to be long enough for the history to become irrelevant, which can be several months for apps that people use on a regular basis. Apps generally only need to purge the history several times a year.

For more information about persistent history processing, see [Consuming Relevant Store Changes](/documentation/CoreData/consuming-relevant-store-changes).

### [Remove duplicate data](/documentation/coredata/sharing-core-data-objects-between-icloud-users#Remove-duplicate-data)

In the CloudKit environment, duplicate data is sometimes inevitable because:

-   Different peers can create the same data. In the sample app, owners can share a photo with a permission that allows participants to tag it. When owners and participants simultaneously create the same tag, a duplicate occurs.
    
-   Apps rely on some initial data and there’s no way to allow only one peer to preload it. Duplicates occur when multiple peers preload the data at the same time.
    

To remove duplicate data (or _deduplicate_), apps need to implement a way that allows all peers to eventually reserve the same object and remove others. The sample app does so in the following way:

1.  It gives each tag a universally unique identifier (UUID). Tags that have the same name (but different UUIDs) and are associated with the same share (and are, therefore, in the same CloudKit record zone) are duplicates, so only one of them can exist.
    
2.  It detects new tags from CloudKit by looking into the persistent history each time a remote change notification occurs. Deduplication only applies to the private persistent store because the user may not have permission to change the shared persistent store.
    
3.  For each new tag, it fetches the duplicates from the same persistent store, and sorts them by their UUID so the tag with the lowest UUID goes first.
    
4.  It picks the first tag as the one to reserve and marks the others as `deduplicated`. Because each UUID is globally unique and each peer picks the first tag, all peers eventually reserve the same tag, which is the one that has the globally lowest UUID.
    
5.  It removes the deduplicated tags sometime later.
    

When detecting duplicate tags, the sample app doesn’t delete them immediately. It waits until the next `eventChangedNotification` occurs, and only removes the tags with a `deduplicatedDate` that’s sometime before the last successful export and import event. This allows enough time for `NSPersistentCloudKitContainer` to synchronize the relationships of the deduplicated tags, and the app to establish the relationships for the tag it reserves.

The following code implements the deduplication process:

```
func deduplicateAndWait(tagObjectIDs: [NSManagedObjectID])
```

The following code shows how the app determines the deduplicated tags it can safely remove:

```
@objc
func containerEventChanged(_ notification: Notification)
```

### [Implement a custom sharing flow](/documentation/coredata/sharing-core-data-objects-between-icloud-users#Implement-a-custom-sharing-flow)

Apps can implement a custom sharing flow when the system sharing UI is unavailable or doesn’t fit. The sample app performs the following tasks so users can share photos from watchOS:

1.  It creates a share using `share(_:to:completion:)` when an owner shares a photo.
    
2.  It configures the share with appropriate permissions, and adds participants for a share. A share is private if its [`publicPermission`](/documentation/CloudKit/CKShare/publicPermission) is [`.none`](/documentation/CloudKit/CKShare/ParticipantPermission/none). For shares that have a public permission more permissive than `.none` (called _public shares_), users can participate by tapping the share link, so there’s no need to add participants beforehand. The sample app looks up participants using [`fetchParticipants(matching:into:completion:)`](/documentation/coredata/nspersistentcloudkitcontainer/3746829-fetchparticipants), configures the participant permission using [`CKShare.Participant.permission`](/documentation/CloudKit/CKShare/Participant/permission-swift.property), and adds it to the share using [`addParticipant(_:)`](/documentation/CloudKit/CKShare/addParticipant\(_:\)).
    
3.  It allows users to deliver the share URL ([`CKShare.url`](/documentation/CloudKit/CKShare/url)) to a participant using `ShareLink`.
    
4.  It implements [`userDidAcceptCloudKitShare(with:)`](/documentation/WatchKit/WKApplicationDelegate/userDidAcceptCloudKitShare\(with:\)) to accept the share using [`acceptShareInvitations(from:into:completion:)`](/documentation/coredata/nspersistentcloudkitcontainer/3746828-acceptshareinvitations). After the acceptance synchronizes, the photo and its relationships are available in the participant’s shared persistent store.
    
5.  It manages the participants using `addParticipant(_:)` and [`removeParticipant(_:)`](/documentation/CloudKit/CKShare/removeParticipant\(_:\)) from the owner side, or stops the sharing or participation by calling `purgeObjectsAndRecordsInZone(with:in:completion:)`. (The purge API deletes the zone from CloudKit, and also the object graph from the Core Data store. Apps that need to keep the object graph can make a deep copy, ensure the new graph doesn’t connect to any share, and save it to the store.)
    

In this process, the sample app calls [`persistUpdatedShare(_:in:completion:)`](/documentation/coredata/nspersistentcloudkitcontainer/3746832-persistupdatedshare) when it changes the share using CloudKit APIs for `NSPersistentCloudKitContainer` to update the store. The following code shows how the app adds a participant:

```
participant.permission = permission
participant.role = .privateUser
share.addParticipant(participant)


self.persistentContainer.persistUpdatedShare(share, in: persistentStore) { (share, error) in
    if let error = error {
        print("\(#function): Failed to persist updated share: \(error)")
    }
    completionHandler?(share, error)
}
```
