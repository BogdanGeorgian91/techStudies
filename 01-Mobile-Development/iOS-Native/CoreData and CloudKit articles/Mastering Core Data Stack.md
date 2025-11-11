
Perhaps you find it a bit tedious, or perhaps you feel that the templates provided by Xcode already meet your needs. Many Core Data users are not willing to spend too much time understanding and mastering Core Data Stack. This not only limits their ability to fully utilize the rich features provided by Core Data, but also leaves developers at a loss when facing abnormal errors. This article will explain the functionality, composition, and configuration of Core Data Stack, and discuss how to design a Core Data Stack that meets current needs based on personal experience. This article will not show complete creation code, but rather an explanation of principles, ideas, and experience.

## What is Core Data Stack

## Functionality

In an application that uses Core Data, the model layer that manages and persists the application is considered as Core Data Stack. In Core Data Stack, a set of coordinated classes provided by Core Data are created and configured to provide object graph management, data persistence and other services for the application.

There are no specific requirements for the naming or type of Core Data Stack instances. You can create and organize your own code using structures and classes according to your own habits and needs.

## Components

A basic Core Data Stack must consist of instances of the following four classes:

-   NSManagedObjectModel (Managed Object Model)
-   NSManagedObjectContext (Managed Object Context)
-   NSPersistentStoreCoordinator (Persistent Store Coordinator)
-   NSPersistentStore (Persistent Store)

The following diagram illustrates their relationships:

![](https://miro.medium.com/v2/resize:fit:700/1*-QvyBlbqG7-smx6gJwkagg.png)

## NSManagedObjectModel

Every Core Data Stack must have an instance of NSManagedObjectModel, which can be seen as a programmatic representation of the actual data model.

Usually, we use Xcode’s data model editor to create a blueprint of the data and define the entities, attributes, relationships, configurations, and others that the application uses.

The data model editor saves the defined results as an XML file. During project compilation, Xcode compiles the file into a binary file with the momd extension and places it in the Bundle. When creating an instance of NSManagedObjectModel, the file is actually used.

## NSManagedObjectContext

The NSManagedObjectContext can be thought of as a scratchpad for drawing, where we can draw freely and erase at any time.

The main responsibility of the managed object context is to manage the collection of NSManagedObject instances, which includes retrieving, creating, deleting, and modifying managed objects. The managed object context has an embedded undo manager that provides undo/redo functionality.

The managed object context ensures that there are no multiple managed object instances corresponding to the same persistent store record in a context, and provides other features such as caching, change tracking, lazy loading, data validation, and change notification.

It sits at the top of the Core Data Stack and is responsible for most of the interaction between the application and the Core Data Stack.

Usually, an application needs to create at least one managed object context instance running on the main thread. In practice, it is not uncommon to create multiple managed object contexts.

## NSPersistentStore

NSPersistentStore is the abstract base class for all Core Data persistent stores, which creates different instances by specifying the storage type (SQLite, Binary, XML, and Memory). The persistent store provides a standard API that converts the internal data objects, logic, and operations of Core Data into corresponding instructions or records for the specified storage type.

If the four preset storage types of Core Data cannot meet your needs, developers can also customize the required persistent storage for their own data sources.

A few years ago, most applications only needed to create one persistent store. With the increasing popularity of Core Data with CloudKit, there are more and more applications with multiple persistent stores.

## NSPersistentStoreCoordinator

NSPersistentStoreCoordinator acts as the glue in the Core Data Stack. As a coordinator, it creates a communication bridge between other components. Whether it’s the managed object model, managed object context, or persistent store, they all collaborate through the persistent store coordinator.

For efficiency reasons, developers usually need to deal directly with the persistent store coordinator in scenarios such as batch data processing, CoreData with CoreSpotlight, and database migration.

As shown in the figure above, a persistent store coordinator corresponds to only one managed object model, but can be used in conjunction with multiple managed object contexts and multiple persistent stores.

> _At this point, many readers may feel that there is a huge omission in this article — NSPersistentContainer. As the most commonly used means of creating Core Data Stack in recent years, it has not been introduced yet. The original intention of NSPersistentContainer is to simplify the configuration complexity of the above modules, with the four components still being the main focus internally. In the following sections, we will introduce the process of creating Core Data Stack before and after the emergence of NSPersistentContainer, allowing readers to have a better understanding of the development process and implementation principles of Core Data Stack._

## The Era without NSPersistentContainer

Before the birth of NSPersistentContainer (prior to Xcode 8), we used to create Core Data Stack using the following process with the four major components mentioned above:

-   Instantiate a managed object model

To create an instance of NSManagedObjectModel, we need to load the data model file from the application bundle. The code would look something like this:

```
// Exemplu încărcare model
guard let url = Bundle.main.url(forResource: "Model", withExtension: "momd"),
      let model = NSManagedObjectModel(contentsOf: url) else {
    fatalError("Nu pot încărca modelul")
}
```

-   Instantiate Persistence Storage Coordinator

To create a persistence storage coordinator, a managed object model instance is needed. Only when the coordinator has mastered the application’s data model can it add persistence storage.

let coordinator \= NSPersistentStoreCoordinator(managedObjectModel: model)

-   Creating Persistent Storage

When creating persistent storage, you need to specify the storage type, configuration name (set in the data model editor), path, and other information. For existing database files, the persistent storage coordinator will check if it is completely consistent with the definition of the managed object model.

```
guard let store \= try? coordinator.addPersistentStore(type: .sqlite,  
                                                              configuration: "Local",  
                                                              at: localURL,  
                                                              options: nil)  
        else {  
            fatalError()  
        }
```

-   Create a managed object context and retain a reference to the managed object

Create a managed object context, set its type (main thread or background thread), and retain a reference to the persistent store coordinator.

```
let viewContext \= NSManagedObjectContext(.mainQueue)  
        viewContext.persistentStoreCoordinator \= coordinator
```

> _Looking solely at the amount of code, even without using NSPersistentContainer, creating a Core Data Stack with basic functionality can be done in just a few lines. However, this approach requires developers to have a thorough understanding and mastery of Core Data’s major components to complete. NSPersistentContainer was created to lower the barrier of entry for developers in creating a Core Data Stack._

## The New Era of NSPersistentContainer

## Xcode 8.x — Xcode 10.x

Since Xcode 8.0, Apple has introduced NSPersistentContainer for Core Data.

NSPersistentContainer encapsulates the managed object model, persistent store coordinator, managed object context, and persistent store, simplifying the creation and management of the Core Data Stack.

An instance of NSPersistentContainer can be viewed as a simplified version of the Core Data Stack. The templates provided by Xcode can handle most scenarios for Core Data Stack requirements.

Below is some sample code from the Core Data template provided in Xcode 13.

```
struct PersistenceController {  
    static let shared \= PersistenceController()  
  
    let container: NSPersistentContainer  
    init(inMemory: Bool \= false) {  
        container \= NSPersistentContainer(name: "Model")  
        if inMemory {  
            container.persistentStoreDescriptions.first!.url \= URL(fileURLWithPath: "/dev/null")  
        }  
        container.loadPersistentStores(completionHandler: { \_, error in  
            if let error \= error as NSError? {  
                fatalError("Unresolved error \\\\(error), \\\\(error.userInfo)")  
            }  
        })  
    }  
}
```

Developers can create a Core Data Stack without any knowledge of managed object models, persistence coordinators, or persistent storage.

NSPersistentContainer greatly reduces the barrier to entry for using Core Data, but it also results in many new Core Data users not understanding the internal workings of Core Data and being unable to use it deeply.

Until the release of NSPersistentCloudContainer in Xcode 11, the role of NSPersistentContainer was limited to simplifying Core Data Stack creation and did not provide any new features.

## Xcode 11.x — present

Starting from Xcode 11, Apple introduced NSPersistentCloudContainer, which breaks down the barrier between Core Data and CloudKit. With this, NSPersistentContainer gradually gains its unique features and becomes increasingly important.

NSPersistentCloudContainer is a subclass of NSPersistentContainer. It simplifies the creation of traditional Core Data Stack and provides support for CloudKit network database. Currently, most methods and properties related to network database can only be operated in NSPersistentCloudContainer. Since Apple has not publicly disclosed the internal details of NSPersistentCloudContainer, third-party stack encapsulation libraries for Core Data can only support local storage (unable to use Core Data with CloudKit functionality).

## What’s included in the current Core Data Stack

In recent years, as Core Data’s capabilities have continued to grow, the content included in the Core Data Stack has also grown increasingly complex. Even when using NSPersistentContainer, the code cannot avoid becoming more complex.

## Core Data with CloudKit

As a central feature of the Apple ecosystem, more and more applications provide network synchronization based on Core Data with CloudKit. Therefore, more settings and extensions need to be made in the Core Data Stack for network synchronization.

> _For more information about NSPersistentCloudContainer, please refer to my series of articles on_ [_Core Data with CloudKit_](https://fatbobman.com/en/posts/coredatawithcloudkit-1/)_._

In addition to using the network synchronization methods and properties provided by the Core Data framework in the Core Data Stack, many developers will create methods suitable for their project applications at the Core Data Stack level. For example, Apple has created many convenient methods on the Core Data Stack for sharing participants, creating CKShare, obtaining CKShare, and data permission verification in the example of [data sharing](https://developer.apple.com/documentation/coredata/synchronizing_a_local_store_to_the_cloud).

## Persistent History Tracking

In recent years, with the strong promotion by Apple, more and more applications provide widgets or share the same data content through the App Group.

For applications using Core Data, enabling the Persistent History Tracking feature for Core Data can provide users with a better experience. In addition, some of Apple’s new APIs require that Persistent History Tracking must be enabled before use.

Therefore, the Core Data Stack has added settings and transaction processing functions for Persistent History Tracking.

## CoreData with CoreSpotlight

At WWDC 2021, Apple introduced a new version of the NSCoreDataCoreSpotlightDelegate API. This API greatly reduces the difficulty of maintaining Core Data data in an application on the system Spotlight. Since creating NSCoreDataCoreSpotlightDelegate requires the use of NSPersistentStoreDescription and NSPersistentStoreCoordinator, the same work needs to be completed in the Core Data Stack. The content and functionality of the Core Data Stack will also be further increased.

> _For more information on NSCoreDataCoreSpotlightDelegate, please refer to my article_ [_Displaying Core Data Data in Spotlight_](https://fatbobman.com/en/posts/spotlight/)_._

## Exposing Context or Container

A few years ago, the Core Data Stack usually only needed to provide an instance of NSManagedObjectContext to the outside world. Through this instance, we could obtain the persistent store coordinator, and through the coordinator, we could obtain the managed object model and persistent storage.

However, after using NSPersistentContainer (especially NSPersistentCloudContainer), developers cannot obtain the corresponding persistent container through the managed object context, and therefore cannot call the persistent container’s specific properties and methods.

Therefore, in the current Core Data Stack, it is best to expose the persistent container to other modules or code for easy use.

## Using structs or classes

Currently, the Core Data template provided by Xcode uses structs to define the Core Data Stack. Based on my personal experience, if your Core Data Stack has complex functionality requirements and code, classes may be a better choice. There are two reasons for this:

-   Typically, only one Core Data Stack instance is needed in an application. Using a class singleton will give me better security and make it easier to access the Stack in different parts of the code.
-   If you need to handle transaction notifications or call NSCoreDataCoreSpotlightDelegate in the Core Data Stack, using a class is easier for programming. For more information, refer to the two articles I provided earlier.

## Creating Multiple Configuration Modes for Core Data Stack

## Why Create Multiple Configurations

Nowadays, when creating a Core Data Stack, it is not only necessary to ensure the normal operation of the program, but also to prepare for scenarios such as Unit Test and SwiftUI’s Preview. Multiple configurations for Core Data Stack to respond to different scenarios can be created through the parameters of the Core Data Stack constructor or the application’s launch parameters.

## Memory Mode

In the Core Data template provided by Xcode, a configuration for memory mode and a demonstration of how to create test data in memory mode have been provided for developers.

It should be noted that the memory mode referred to here still corresponds to the SQLite storage type (not the memory mode supported by one of the four storage modes supported by NSPersistentStore), and the effect of only saving data in memory is achieved by setting the storage path of persistent storage to `/dev/null`.

Use parameters to set the memory mode:

```
/// Whether to enable only memory mode. Can be enabled by startup parameter -InMemory 1 or constructor parameter inMemory:true  
    private let \_inMemory: Bool  
    private lazy var inMemory: Bool \= {  
        let arguments \= ProcessInfo.processInfo.arguments  
        var allow \= false  
        for index in 0..<arguments.count \- 1 where arguments\[index\] \== "-InMemory" {  
            allow \= arguments.count \>= (index + 1) ? arguments\[index + 1\] \== "1" : false  
            break  
        }  
        return allow || \_inMemory  
    }()
```

In the template in Xcode, memory mode and non-memory mode cannot coexist, which is reasonable in most cases.

In the development of [Health Notes](https://www.fatbobman.com/healthnotes/), I need to make the memory mode coexist with the non-memory mode, that is, in specific situations, there will be two Containers with the same managed object model in the application at the same time, and they can be switched at any time. To solve the problem that the same managed object model file can only be held by one instance in the application, you can create an NSManagedObjectModel instance and use it to create NSPersistentCloudContainer separately.

```
class CoreDataStack {  
    private static var \_model: NSManagedObjectModel?  
    static func model(name: String \= CoreDataStackSetting.defaultModelName) -> NSManagedObjectModel {  
  
    if \_model \== nil {  
            do {  
                \_model \= try loadModel(name: name, bundle: Bundle.main)  
            } catch {  
                let err \= error.localizedDescription  
                fatalError("❌Database momd file cannot be loaded")  
            }  
        }  
        return \_model!  
    }  
    private static func loadModel(name: String, bundle: Bundle) throws -> NSManagedObjectModel {  
        guard let modelURL \= bundle.url(forResource: name, withExtension: "momd") else {  
            fatalError("❌Database momd file cannot be loaded")  
        }  
        guard let model \= NSManagedObjectModel(contentsOf: modelURL) else {  
            fatalError("❌Database momd file cannot be parsed")  
        }  
        return model  
    }  
    public lazy var persistentContainer: NSPersistentCloudKitContainer \= {  
        let container \= NSPersistentCloudKitContainer(  
            name: modelName,  
            managedObjectModel: Self.model(name: modelName)  
        )  
        // Other configuration code  
        ........  
    }  
}
```

## Mode without network synchronization required

In the application using Core Data with CloudKit, we do not need to enable network synchronization every time we debug the code. By turning off network synchronization through parameters, we can simplify the debugging process and reduce the large amount of console output caused by network synchronization.

Setting up network synchronization with parameters:

```
/// Whether to allow network synchronization or not, can use constructor parameter allowCloudKiteSync = false or start parameter -AllowCloudKitSync 0 to disable network synchronization  
    private let \_allowCloudKitSync: Bool  
    private lazy var allowCloudKitSync: Bool \= {  
        let arguments \= ProcessInfo.processInfo.arguments  
        var allow \= true  
        for index in 0..<arguments.count \- 1 where arguments\[index\] \== "-AllowCloudKitSync" {  
            allow \= arguments.count \>= (index + 1) ? arguments\[index + 1\] \== "1" : true  
            break  
        }  
        return allow && \_allowCloudKitSync  
    }()

To disable network synchronization:

if !allowCloudKitSync {  
            privateDescrition.cloudKitContainerOptions \= nil  
            shareDescription.cloudKitContainerOptions \= nil  
        }
```

Just set the cloudKitContainerOptions of the corresponding NSPersistentStoreDescription instance to nil.

It should be noted that if you have enabled Persistent History Tracking in your code, you still need to keep it enabled when turning off network synchronization.

## Test mode

In order not to damage the contents of the original SQLite database file during Unit Test testing, I usually create a test mode. In this mode, the data will still be persisted, but it will be saved in the user’s caches directory and cleared before each test.

```
/// Whether it is a test mode, used in Unit Test, in this mode, local storage will be saved in the Catch directory  
    private let \_testMode: Bool  
    private lazy var testMode: Bool \= {  
        let arguments \= ProcessInfo.processInfo.arguments  
        var allow \= false  
        for index in 0..<arguments.count \- 1 where arguments\[index\] \== "-TestMode" {  
            allow \= arguments.count \>= (index + 1) ? arguments\[index + 1\] \== "1" : false  
            break  
        }  
        return allow || \_testMode  
    }()  
  
    if !testMode {  
                privateDescrition \= NSPersistentStoreDescription(url: groupStoreURL)  
     } else {  
            // Saved in the Catch directory  
            privateDescrition \= NSPersistentStoreDescription(url: privateStoreTestURL)  
     }
```

> _Create a suitable mode for the Core Data Stack according to your needs and reference it through the singleton pattern._

```
public extension CoreDataStack {  
    /// Stack used by normal app  
    static let shared = CoreDataStack(modelName: "Model")  
  
    /// Preview Stack saved only in memory  
    static let previewInMemory = CoreDataStack(modelName: "Model", inMemory: true)  
    /// Preview Stack saved in local storage  
    static let previewInPersistentStore = CoreDataStack(modelName: "Model", allowCloudKitSync: false)  
    /// Unit Test mode  
    static let testMode = CoreDataStack(modelName: "Model",testMode: true)  
}
```

![](https://miro.medium.com/v2/resize:fit:700/0*aiMlHjd2RIxcDkfn.png)
## Summary

In recent years, the Core Data Stack has gradually gone through a process of simplification and expansion. Creating real code and practicing more will help you better understand and master it.

