Source:
```
https://fatbobman.medium.com/core-data-with-cloudkit-part-6-sharing-data-cc483046de7b
```

Creating an App that Shares Data with Multiple iCloud Users

In this article, we will explore how to create an app that shares data with multiple iCloud users using Core Data with CloudKit.

## Table of Contents

- Part 1: The Basics
- Part 2: Syncing Local Database to iCloud Private Database
- Part 3: Exploring the CloudKit Dashboard
- Part 4: Troubleshooting
- Part 5: Synchronizing Public Database
- Part 6: Sharing Data in the iCloud

Many of us have used the shared photo albums or shared notes features on iOS. These features were built on the CloudKit shared data API that Apple introduced several years ago. At WWDC 2021, Apple integrated this feature into Core Data with CloudKit, allowing us to create applications with the same functionality using the familiar Core Data approach and only minimal use of CloudKit API.

As mentioned in the WWDC session, "Build apps that share data through CloudKit and Core Data," implementing shared data functionality is more complex than syncing private or public databases. While Apple has provided new APIs to simplify the process, implementing this functionality completely in an application still poses significant challenges.

## Fundamentals

This section mainly introduces the sharing mechanism under Core Data with CloudKit, which differs from the native CloudKit sharing mechanism in some aspects.

## Owners and Participants

In each shared data relationship, there is an **owner** and several **participants**. Both the owner and the participants must be iCloud users and can only perform operations on Apple devices that are logged in with valid iCloud accounts.

The owner initiates the sharing and sends a sharing link to the participants. When a participant clicks on the sharing link, the device will automatically open the corresponding app and import the shared data.

The owner can specify specific participants or make the sharing available to anyone who clicks on the link. These two options are mutually exclusive and can be switched. When switching from specifying specific participants to anyone, the system will delete all specific participant information.

The owner can also set data operation permissions for participants, either read-only or read-write, and can modify these permissions later.

## CKShare

**CKShare** is a dedicated record type for managing sets of shared records. It contains information about the root record or custom zone information that needs to be shared, as well as information about the owner and participants in this sharing relationship.

In Core Data with CloudKit mode, the process of setting up a hosted object instance (NSManagedObject) for sharing is actually creating a CKShare instance for it.

```swift
let (ids, share, ckContainer) = try await stack.persistentContainer.share([note1,note2], to: nil)
```

- We can share multiple managed object instances in a sharing relationship at once.
- All data related to the managed object relationship will be automatically shared.
- Any modifications made to the shared managed objects will be automatically synced to the owner and participants' devices.
- Under the current Core Data with CloudKit mechanism, we cannot add the top-level managed object (such as note in the code above) after sharing.

## Cloud Sharing Mechanism

Before WWDC 2021, CloudKit's mechanism for sharing records was through a **rootRecord**, where the owner created a CKShare for a CKRecord to enable sharing of a single record (including its relationship data).

```swift
let user = CKRecord(recordType:"User")
let share = CKShare(rootRecord: user)
```

At WWDC 2021, CloudKit introduced a new sharing mechanism — **sharing custom zones**. The owner creates a new custom zone in their private database and creates a CKShare for that zone. Participants will then be able to share all the data in that zone.

```swift
init(recordZoneID: CKRecordZone.ID)
```

This sharing mechanism is more suitable for applications with larger datasets and more complex relationships. This is the sharing mechanism used in Core Data with CloudKit data sharing.

As we previously mentioned in our article about synchronizing private databases, a custom zone in a private database can create a CKDatabaseSubscription, which participants can use to stay up-to-date with changes to shared data.

When the owner creates a sharing relationship, the system will automatically:

1. Create a new custom zone (`com.apple.coredata.cloudkit.share.xxx-xx-xx-xx-xxx`) for it in their private database
2. Move the shared data (including its relationship data) from the `com.apple.coredata.cloudkit.zone` in the private database to the newly created zone

This process is automatically done by NSPersistentCloudContainer.

Each sharing relationship will create a new custom zone.

Participants will see a custom zone with the same name as the newly created zone in their **shared network database** (as we previously mentioned, the shared database is a data projection of other users' private databases).

In the world of data, owners perform all operations within their own custom area of a private database, while participants work within their corresponding custom area of a shared database in their own network.

Regardless of whether a user initiates or accepts a share, the logic for saving data remains unchanged for all roles in a sharing relationship.

## Local Storage Mechanism

In a previous article, we introduced how to create multiple persistent stores by using multiple NSPersistentStoreDescriptions. Similarly, when sharing data through Core Data with CloudKit on a user's device, **two local SQLite databases** need to be created. These two databases correspond to the private and shared databases on the network side.

### Owner Perspective

From the perspective of the owner in a sharing relationship, all data created by the owner is stored in their local private database. Even if the data is shared, any modifications made by other participants will still be saved in the owner's private database.

### Participant Perspective

From the perspective of a participant, any data shared by an owner is saved in the participant's local shared database file, even if it is added or modified by the participant themselves.

The behavior described above is exactly the same as the logic on the network side.

To implement the above functions, Apple has done a lot of work behind the scenes. When syncing data, NSPersistentCloudContainer needs to perform a lot of work such as judging, converting, etc., to determine whether the data belongs to the network private database or local persistent storage. Therefore, in practical use, **the sync speed is slower than simply syncing a local database**.

Since the shared database is a projection of the network private database, the data model used by both databases is identical. Therefore, in terms of code implementation, simple Copy can be used.

```swift
guard let shareDesc = privateDesc.copy() as? NSPersistentStoreDescription else {
    fatalError("Create shareDesc error") 
}
```

Last year, Apple added the `databaseScope` property to `cloudKitContainerOptions`, supporting private and public, and this year they added the shared option to support shared data types.

```swift
shareDescOption.databaseScope = .shared
```

Since all shared data requires a corresponding CKRecord information, the local private database must also support network synchronization.

Just like syncing public databases, in order to shorten the time required to query CloudKit data over the network, Core Data with CloudKit saves all NSManagedObject-related CKRecords in the local database file. In the case of sharing data, the local database also saves the corresponding custom area and all CKShare information.

These measures greatly improve the efficiency of data queries and also impose higher requirements on maintaining the validity of local cache data. Apple provides some APIs to address the freshness issue of the cache, but they are not perfect and developers still need to write additional code. Additionally, the system's built-in UICloudSharingController still does not support cache updates (as of Xcode 13 beta 5).

## New API

Apple has made significant updates to the CloudKit API this year, adding Async/Await versions to all callback-based asynchronous methods. Additionally, they have updated and added many methods to Core Data with CloudKit to support data sharing. As mentioned in the previous article, Apple has greatly enhanced the presence of NSPersistentCloudContainer, and most of the new methods are added to NSPersistentCloudContainer.

### Key New Methods

- **acceptShareInvitations**: Allows participants to accept invitations and runs in the AppDelegate.
    
- **share**: Creates a CKShare for a managed object.
    
- **fetchShares(in:)**: Retrieves all CKShares in the persistent store.
    
- **fetchShares(matching:)**: Retrieves the CKShare for a specified managed object.
    
- **fetchParticipants**: Retrieves participant information from the shared relationship through CKUserIdentity.LookupInfo. For example, you can find a participant by email or phone number.
    
- **persistUpdatedShare**: Persists the updated CKShare in the local catch. After developers modify a CKShare through code, the updated CKShare should be persisted in the local catch after being updated through the network. Currently, UICloudSharingController lacks this step, leading to bugs after stopping updates.
    
- **purgeObjectsAndrecordsInZone**: Deletes a specified custom zone and all associated managed objects in the local persistent store. In the current version (XCode 13 beta 5), the owner does not complete enough cleanup work after stopping updates. As a result, the CKShare is still saved in the local catch, and the managed object cannot be invoked by UICloudSharingController. The data on the network is still stored in the custom zone created for sharing (and should be moved back to the normal custom zone).
    

## UICloudSharingController

**UICloudSharingController** is a view controller provided by UIKit for adding and removing people from CloudKit shared records. With only a small amount of code, developers can have the following features:

- Invite people to view or collaborate on shared records
- Set access permissions to determine who can access shared records (only invited people or anyone with the shared link)
- Set general or individual permissions (read-only or read/write)
- Revoke access for one or more participants
- Stop participating (if the user is a participant)
- Stop sharing with all participants (if the user is the owner of the shared record)

UICloudSharingController provides two initializer methods, one for CKShare already generated and one for not generated.

In SwiftUI, the initializer method for situations where CKShare has not been generated is abnormal when using UIViewControllerRepresentable, so it is recommended to first use code (`share`) to manually generate CKShare for the managed object, and then use another initializer method for CKShare already generated.

UICloudSharingController provides several delegate methods, and we need to do some post-sharing cleanup work in them after stopping sharing.

The current version (Xcode 13 beta 5) of UICloudSharingController still has bugs, and we hope they can be fixed as soon as possible.

## Example

I have created a demo on Github to showcase the key features discussed in this article.

**Link**: https://github.com/fatbobman/ShareData_Demo_For_CoreDataWithCloudKit

## Project Settings

### info.plist

To enable the ability to open shared links within the application, add `CKSharingSupported` to the info.plist file. Starting with Xcode 13, this can be done directly in the info section.

### Signing&Capabilities

Similar to syncing local data, add the corresponding capabilities (iCloud, background) and CKContainer in Signing&Capabilities.

### Setup AppDelegate

In order for the application to accept sharing invitations, we must respond to incoming share metadata in the UIApplicationDelegate. In UIKit lifecycle mode, simply add code similar to the following in the AppDelegate:

```swift
func application(_ application: UIApplication, userDidAcceptCloudKitShareWith cloudKitShareMetadata: CKShare.Metadata) {
    let shareStore = CoreDataStack.shared.sharedPersistentStore
    let persistentContainer = CoreDataStack.shared.persistentContainer
    persistentContainer.acceptShareInvitations(from: [cloudKitShareMetadata], into: shareStore, completion: { metas,error in
        if let error = error {
            print("accepteShareInvitation error :\(error)")
        }
    })
}
```

Use the `acceptShareInvitations` method of NSPersistentCloudContainer to accept CKShare.Metadata.

In SwiftUI lifecycle mode, this response occurs in the UIWindowSceneDelegate. Therefore, a delegate class is needed in the AppDelegate to handle the transfer.

```swift
final class AppDelegate:NSObject,UIApplicationDelegate{
    func application(_ application: UIApplication,
                     configurationForConnecting connectingSceneSession: UISceneSession,
                     options: UIScene.ConnectionOptions) -> UISceneConfiguration {
        let sceneConfig = UISceneConfiguration(name: nil, sessionRole: connectingSceneSession.role)
        sceneConfig.delegateClass = SceneDelegate.self
        return sceneConfig
    }
}

final class SceneDelegate:NSObject,UIWindowSceneDelegate{
    func windowScene(_ windowScene: UIWindowScene, userDidAcceptCloudKitShareWith cloudKitShareMetadata: CKShare.Metadata) {
        let shareStore = CoreDataStack.shared.sharedPersistentStore
        let persistentContainer = CoreDataStack.shared.persistentContainer
        persistentContainer.acceptShareInvitations(from: [cloudKitShareMetadata], into: shareStore, completion: { metas,error in
            if let error = error {
                print("accepteShareInvitation error :\(error)")
            }
        })
    }
}
```

## Core Data Stack

The setup of CoreDataStack is basically similar to the setup in previous articles. However, it's worth noting that, for the convenience of persistence storage determination, `privatePersistentStore` and `sharedPersistentStore` have been added at the Stack level to store local private and shared persistent databases respectively.

```swift
let dbURL = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first!

let privateDesc = NSPersistentStoreDescription(url: dbURL.appendingPathComponent("model.sqlite"))
privateDesc.configuration = "Private"
privateDesc.cloudKitContainerOptions = NSPersistentCloudKitContainerOptions(containerIdentifier: ckContainerID)
privateDesc.cloudKitContainerOptions?.databaseScope = .private

guard let shareDesc = privateDesc.copy() as? NSPersistentStoreDescription else {
    fatalError("Create shareDesc error")
}
shareDesc.url = dbURL.appendingPathComponent("share.sqlite")
let shareDescOption = NSPersistentCloudKitContainerOptions(containerIdentifier: ckContainerID)
shareDescOption.databaseScope = .shared
shareDesc.cloudKitContainerOptions = shareDescOption
```

The local shared database is created by copying the private database description. Two persistent stores are set up with different URLs, and the `shareDescOption.databaseScope` is set to `.shared` for the shared description.

Convenience methods have been added to the Stack to make logical judgments in views easier.

### Checking if an Object is Shared

The following code checks whether a managed object is a shared object. To speed up the check, it first checks whether the data is stored in the local shared database, and then uses `fetchShares` to check whether a CKShare has been generated.

```swift
func isShared(objectID: NSManagedObjectID) -> Bool {
    var isShared = false
    if let persistentStore = objectID.persistentStore {
        if persistentStore == sharedPersistentStore {
            isShared = true
        } else {
            let container = persistentContainer
            do {
                let shares = try container.fetchShares(matching: [objectID])
                if shares.first != nil {
                    isShared = true
                }
            } catch {
                print("Failed to fetch share for \(objectID): \(error)")
            }
        }
    }
    return isShared
}
```

### Checking Object Ownership

The following code checks whether the current user is the owner of a shared object:

```swift
func isOwner(object: NSManagedObject) -> Bool {
    guard isShared(object: object) else { return false }
    guard let share = try? persistentContainer.fetchShares(matching: [object.objectID])[object.objectID] else {
        print("Get ckshare error")
        return false
    }
    if let currentUser = share.currentUserParticipant, currentUser == share.owner {
        return true
    }
    return false
}
```

## Wrapping UICloudSharingController

To learn more about the usage of UIViewControllerRepresentable, please read my other article "Using UIKit Views in SwiftUI."

Wrapping UICloudSharingController is not difficult, but a few things need to be noted:

### Key Requirements

1. **Ensure that the managed object being shared has created a CKShare.**
    
2. **Handle Constructor Issues**: Since UICloudSharingController behaves abnormally for the constructor without creating CKShare when used for UIViewControllerRepresentable, for the first shared managed object, we need to create CKShare for it in the code. Creating CKShare usually takes a few seconds and has a certain impact on user experience. In the demo, I also demonstrated another way to call UICloudSharingController without using UIViewControllerRepresentable.
    
3. **Set CKShare Title**: The code for creating CKShare is as follows:
    

```swift
func getShare(_ note: Note) -> CKShare? {
    guard isShared(object: note) else { return nil }
    guard let share = try? persistentContainer.fetchShares(matching: [note.objectID])[note.objectID] else {
        print("Get ckshare error")
        return nil
    }
    share[CKShare.SystemFieldKey.title] = note.name
    return share
}
```

4. **Ensure Metadata Title**: Ensure that the metadata `CKShare.SystemFieldKey.title` of CKShare has a value, otherwise sharing via email or messages will not be possible. The content can be defined by yourself, as long as it can clearly represent the content you want to share.

```swift
func makeUIViewController(context: Context) -> UICloudSharingController {
    share[CKShare.SystemFieldKey.title] = note.name
    let controller = UICloudSharingController(share: share, container: container)
    controller.modalPresentationStyle = .formSheet
    controller.delegate = context.coordinator
    context.coordinator.note = note
    return controller
}
```

### Important Notes

- **Coordinator Lifecycle**: The lifecycle of the coordinator should be longer than that of UIViewControllerRepresentable. Since sharing operations require network operations, it usually takes a few seconds to return results. UICloudSharingController will be destroyed after sending the shared link. If the coordinator is defined in UIViewControllerRepresentable, it will cause the delegate method to be unable to be called back after returning the result.
    
- **Item Title**: The delegate method `itemTitle` needs to return content, otherwise email sharing cannot be invoked.
    
- **Handle Cleanup**: Handle the aftermath of stopping sharing in the delegate method `cloudSharingControllerDidStopSharing`.
    

## Initiating Sharing

Before calling UICloudSharingController on a managed object, it is necessary to check whether a CKShare has already been created for it. If not, the CKShare must be created first. If the managed object is already shared, calling UICloudSharingController will display information about all current participants in the sharing relationship and allow modifying sharing options and user permissions.

```swift
if isShared {
    showShareController = true
} else {
    Task.detached {
        await createShare(note)
    }
}
```

Using `Task.detached` to avoid thread blocking during CKShare generation.

In addition, there is another way to directly call UICloudSharingController in the demo (which has been commented out), which provides a better user experience but is not very SwiftUI-like.

```swift
private func openSharingController(note: Note) {
    let keyWindow = UIApplication.shared.connectedScenes
        .filter { $0.activationState == .foregroundActive }
        .map { $0 as? UIWindowScene }
        .compactMap { $0 }
        .first?.windows
        .filter { $0.isKeyWindow }.first

    let sharingController = UICloudSharingController {
        (_, completion: @escaping (CKShare?, CKContainer?, Error?) -> Void) in

        stack.persistentContainer.share([note], to: nil) { _, share, container, error in
            if let actualShare = share {
                note.managedObjectContext?.performAndWait {
                    actualShare[CKShare.SystemFieldKey.title] = note.name
                }
            }
            completion(share, container, error)
        }
    }

    keyWindow?.rootViewController?.present(sharingController, animated: true)
}
```

## Checking Permissions

Before making any modification or deletion operation on managed objects in the application, it is necessary to check the operation permissions. Only data with read and write permissions should have the modification function enabled.

```swift
if canEdit {
    Button {
        withAnimation {
            stack.addMemo(note)
        }
    }
    label: {
        Image(systemName: "plus")
    }
}

func canEdit(object: NSManagedObject) -> Bool {
    return persistentContainer.canUpdateRecord(forManagedObjectWith: object.objectID)
}
```

## Debugging Notes

Compared to syncing the local database and syncing the public database, debugging shared data is more difficult, which also tests the developer's mentality more.

### Key Challenges

- **Device Requirements**: Since debugging cannot be done on the simulator, developers need to prepare at least two devices with different iCloud accounts.
    
- **Slow Response**: Due to being in the testing phase, the response speed of shared synchronization is much slower than that of syncing the local private database alone. Usually, it takes several tens of seconds for data created locally to sync to the private database in the cloud. After the invitee receives the synchronization invitation, it also takes some time for the CKShare data of the two devices to refresh.
    
- **Background Switching**: If you feel that the data has not been synchronized after a certain period of time, please switch the application to the background and then switch back. Sometimes it may even require a cold start of the application.
    

In addition, certain known bugs may cause abnormal situations, so please read the following known issues before debugging and avoid the pits I stepped on during debugging.

## Known Issues

### Issue 1: Relationship Data Access

When sharing, if set to be receivable by anyone, participants will not be able to obtain the relationship data of the hosted object before sharing, and the modified hosted object (or newly added relationship data) will only be displayed in the participant's application after the hosted object has been shared. It is unclear whether this is a bug or an intentional design by Apple.

### Issue 2: Share Link Problems

When sharing, if set to be receivable by anyone, it is not recommended to directly send the share link via message, email, or other means from the UICloudSharingController to another valid iCloud account, as the share link may not be opened and will display "Share Canceled". To solve this issue, you can copy the link and send it via message or email.

### Issue 3: Link Opening Method

It is recommended to open the share link via information or system email (which will activate the Deep Link). Other methods may directly access the link through the browser, which may result in an inability to accept the invitation.

### Issue 4: Participant Permission Revocation

After the data owner stops sharing permissions for a participant through the UICloudSharingController, the modified CKShare cannot be refreshed properly, which results in the UICloudSharingController being unable to be awakened again. There is currently no direct solution as there is no corresponding delegate method. The normal logic is that after modifying the CKShare, the server returns a new CKShare, which is then updated locally through `persistUpdatedShare`.

### Issue 5: Stop All Sharing Cleanup

After the data owner stops sharing (stops all sharing), the UICloudSharingController will have a similar problem as the previous issue — it will not delete the CKShare in the local catch. This problem can be solved by performing a Deep Copy (including all relationship data) of the hosted object being stopped in `cloudSharingControllerDidStopSharing`, and then executing `purgeObjectsAndRecordsInZone`. If there is a large amount of data, this solution may take a longer execution time. Hopefully, Apple can provide a more direct solution.

### Issue 6: Participant-side CKShare Refresh

After the data owner cancels a participant's sharing permission, the participant's CKShare refresh may be incomplete. The shared data on the participant's device may disappear (it will definitely disappear after the next cold start of the application), or it may not disappear. If the participant operates on the shared data at this time, it may cause the application to crash, affecting the user experience.

### Issue 7: Self-cancellation Issues

After the participant cancels their own sharing through the UICloudSharingController, the CKShare refresh is incomplete, and the same symptoms as the previous issue may occur. However, this problem can be solved by deleting the hosted object on the participant's device in `cloudSharingControllerDidStopSharing`.

### Workaround

Issues 4, 5, and 7 can be avoided by creating your own UICloudSharingController implementation.