# Shared Records | Apple Developer Documentation

Collection

-   [CloudKit](/documentation/cloudkit)
-   Shared Records

API Collection

# Shared Records

Share one or more records with other iCloud users.

## [Overview](/documentation/cloudkit/shared-records#overview)

CloudKit users can share records in their private databases with other iCloud users, which enables collaboration between the people using your app. The user that initiates sharing, the owner, handles all aspects of the collaboration, from inviting people to participate to restricting what actions the participants can perform.

![A diagram that shows the owner of a private database using iCloud to share records with two participants. The owner’s private database stores the records, which CloudKit makes available to each participant in their shared database.](https://docs-assets.developer.apple.com/published/c20e124d54f7123e04eb675844c16793/media-3761001%402x.png)

CloudKit allows you to share both record zones and record hierarchies. If you want to share an unbounded collection of records that don’t have natural parent-child relationships, share their containing record zone. However, if you want to share only a specific set of related records, define an explicit record hierarchy and share that instead.

For more information, see [Sharing CloudKit Data with Other iCloud Users](/documentation/cloudkit/sharing-cloudkit-data-with-other-icloud-users).

### [Share a Record Zone or Hierarchy](/documentation/cloudkit/shared-records#Share-a-Record-Zone-or-Hierarchy)

You store shareable records in a custom record zone in the user’s private database. As you create records in that zone, they become eligible for record zone sharing. If you then choose to share that record zone, CloudKit allows participants to access all the records it contains.

Alternatively, you can build a hierarchy by defining relationships between records as you create them. Set a record’s [`parent`](/documentation/cloudkit/ckrecord/parent) property to designate it as a child of the referenced record. If your data model is hierarchical, this is good practice even if you don’t plan to share the records. Whereas sharing a record zone is catch-all, sharing a record hierarchy allows you to specify exactly which records to include.

To begin sharing, create an instance of [`CKShare`](/documentation/cloudkit/ckshare) with either the ID of the record zone to share, or the root record, which defines the point in the record hierarchy where you want to start sharing. CloudKit shares all the records in the record zone, or every record in the hierarchy below the root. A record can take part in only a single share. This applies to every record in the shared record zone or hierarchy.

After you create the share, save it using [`CKModifyRecordsOperation`](/documentation/cloudkit/ckmodifyrecordsoperation). The shared records must already exist in iCloud or be part of the same save operation.

### [Invite Participants](/documentation/cloudkit/shared-records#Invite-Participants)

After saving the share, CloudKit assigns it a stable share URL. Use this URL to invite other users to participate. In iOS, [`UICloudSharingController`](/documentation/UIKit/UICloudSharingController) provides a consistent and familiar experience for managing a share’s participants and their permissions, and for distributing the share URL. Use [`NSItemProvider`](/documentation/Foundation/NSItemProvider) and [`NSSharingService`](/documentation/AppKit/NSSharingService) in macOS (with the [`cloudSharing`](/documentation/AppKit/NSSharingService/Name/cloudSharing) service name) to achieve similar functionality. Only invited participants can join a private share. Anyone with the share URL can join a public share. For more information, see [`publicPermission`](/documentation/cloudkit/ckshare/publicpermission).

When an invited user taps or clicks the share URL, CloudKit verifies they have an active iCloud account, which must match their participant details. After successful verification, the system launches your app. CloudKit provides share metadata to your app’s scene delegate or app delegate. The method the system calls varies by platform and app configuration. For more information, see [`CKShare.Metadata`](/documentation/cloudkit/ckshare/metadata).

### [Manage Share Participation](/documentation/cloudkit/shared-records#Manage-Share-Participation)

After receiving the share metadata from CloudKit, use [`CKAcceptSharesOperation`](/documentation/cloudkit/ckacceptsharesoperation) to confirm the user’s participation. CloudKit then creates a record zone in the participant’s shared database that provides a view into the owner’s private database. The record zone contains only the records in the share; no other data is accessible. A participant with write permissions can change or delete records in this new record zone. Any changes they make are visible to all participants.

Create a database subscription in the user’s shared database the first time they launch your app. Then, when they confirm participation in a share, iCloud notifies your app, on all of the user’s devices, of any changes to the shared records. For more information, see [`CKDatabaseSubscription`](/documentation/cloudkit/ckdatabasesubscription).

To stop sharing, the share’s owner must delete the share or, for shared hierarchies, the root record. If a participant wants to leave the share, delete the share record from their shared database. Use [`UICloudSharingController`](/documentation/UIKit/UICloudSharingController) or [`NSSharingService`](/documentation/AppKit/NSSharingService) to allow a participant to stop participating. Or remove them from the share using the [`removeParticipant(_:)`](/documentation/cloudkit/ckshare/removeparticipant\(_:\)) method, and then save the updated share to iCloud.

### [Customize the Sharing Experience](/documentation/cloudkit/shared-records#Customize-the-Sharing-Experience)

You can use the framework’s share-related operations to implement behavior similar to that of [`UICloudSharingController`](/documentation/UIKit/UICloudSharingController) and [`NSSharingService`](/documentation/AppKit/NSSharingService) to build a custom sharing experience by following these steps:

1.  Use [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) to generate participants and add them to the share using [`addParticipant(_:)`](/documentation/cloudkit/ckshare/addparticipant\(_:\)). Your app presents a list of potential participants to the user. You can also allow the owner to add participants by entering a participant’s email address or phone number.
    
2.  Save the share to iCloud.
    
3.  Provide the share URL to the participants. You can send the URL in an email or a message, or your app might have secure, in-app chat between users to facilitate distribution of the URL.
    
4.  For each participant, fetch the share’s metadata using [`CKFetchShareMetadataOperation`](/documentation/cloudkit/ckfetchsharemetadataoperation) and the share URL.
    
5.  Use [`CKAcceptSharesOperation`](/documentation/cloudkit/ckacceptsharesoperation) to confirm participation.
    
6.  After you share records, use the properties and methods on [`CKShare`](/documentation/cloudkit/ckshare) to manage the share’s participants.
    

For public shares, you can skip the first step. Accepting a public share’s metadata automatically adds the user as a participant.

## [Topics](/documentation/cloudkit/shared-records#topics)

### [Collaboration](/documentation/cloudkit/shared-records#Collaboration)

[

Sharing CloudKit Data with Other iCloud Users](/documentation/cloudkit/sharing-cloudkit-data-with-other-icloud-users)

Create and share private CloudKit data with other users by implementing the sharing UI.

[

Sharing Core Data objects between iCloud users](/documentation/CoreData/sharing-core-data-objects-between-icloud-users)

Use Core Data and CloudKit to synchronize data between devices of an iCloud user and share data between different iCloud users.

[`class CKShare`](/documentation/cloudkit/ckshare)

A specialized record type that manages a collection of shared records.

[`struct CKShareTransferRepresentation`](/documentation/cloudkit/cksharetransferrepresentation)

A transfer representation the system uses to share an item.

[`class CKAllowedSharingOptions`](documentation/cloudkit/CKAllowedSharingOptions.md)

An object that controls participant access and permission options.

[`class CKSystemSharingUIObserver`](/documentation/cloudkit/cksystemsharinguiobserver)

An object the system uses to monitor changes in sharing.

[`@MainActor class UICloudSharingController`](/documentation/UIKit/UICloudSharingController)

A view controller that presents standard screens for adding and removing people from a CloudKit share record.

[`CKSharingSupported`](/documentation/BundleResources/Information-Property-List/CKSharingSupported)

A Boolean value that indicates your app supports CloudKit Sharing.

### [Share Requests](/documentation/cloudkit/shared-records#Share-Requests)

[`class CKFetchShareMetadataOperation`](/documentation/cloudkit/ckfetchsharemetadataoperation)

An operation that fetches metadata for one or more shares.

[`class Metadata`](/documentation/cloudkit/ckshare/metadata)

An object that describes a shared record’s metadata.

[`class CKAcceptSharesOperation`](/documentation/cloudkit/ckacceptsharesoperation)

An operation that confirms a user’s participation in a share.

### [Participants](/documentation/cloudkit/shared-records#Participants)

[`class CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation)

An operation that converts user identities into share participants.

[`class Participant`](/documentation/cloudkit/ckshare/participant)

An object that describes a user’s participation in a share.

### [Base Objects](/documentation/cloudkit/shared-records#Base-Objects)

[`class CKOperation`](/documentation/cloudkit/ckoperation)

The abstract base class for all operations that execute in a database.

## [See Also](/documentation/cloudkit/shared-records#see-also)

### [Records](/documentation/cloudkit/shared-records#Records)

[

API Reference

Local Records](/documentation/cloudkit/local-records)

Manipulate records on-device and save changes to the server.

[

API Reference

Remote Records](/documentation/cloudkit/remote-records)

Use subscriptions and change tokens to efficiently manage modifications to remote records.

[`class CKSyncEngine`](/documentation/cloudkit/cksyncengine-5sie5)

An object that manages the synchronization of local and remote record data.

Current page is Shared Records

---
# Sharing CloudKit Data with Other iCloud Users | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [Shared Records](/documentation/cloudkit/shared-records)
-   Sharing CloudKit Data with Other iCloud Users

Sample Code

# Sharing CloudKit Data with Other iCloud Users

Create and share private CloudKit data with other users by implementing the sharing UI.

[Download](https://docs-assets.developer.apple.com/published/2c475bde7d81/SharingCloudKitDataWithOtherICloudUsers.zip)

## [Overview](/documentation/cloudkit/sharing-cloudkit-data-with-other-icloud-users#Overview)

As technology advances, people collaborate with others through cloud-based apps more than ever. They can share digital assets with friends, or invite their colleagues living around the world to work together. To support such use cases, apps need to move user data to the cloud and implement a data-sharing flow that includes features like sharing management, data synchronization, and access control.

This sample demonstrates how to use CloudKit to implement these features by allowing users to create topic and note records in their private databases and share them with other users. With the CloudKit sharing UI, users can send a share link, stop sharing topics, and manage permissions for a shared topic. Users who accept the share, called _participants_, can view or edit the shared record, or stop participating in the share.

The sample also demonstrates how to create an in-memory cache for a CloudKit record zone. Because of this local cache, the sample doesn’t have to query the server while navigating the UI within the zone.

### [Configure the Sample Code Project](/documentation/cloudkit/sharing-cloudkit-data-with-other-icloud-users#Configure-the-Sample-Code-Project)

Before building the sample, perform the following steps in Xcode:

1.  In the General pane of the `CloudKitShare` target, update the Bundle Identifier field with a new identifier.
    
2.  In the Signing & Capabilities pane, select the applicable team from the Team drop-down menu to let Xcode automatically manage the provisioning profile. See [Assign a project to a team](https://help.apple.com/xcode/mac/current/#/dev23aab79b4) for details.
    
3.  Make sure the iCloud capability is present and the CloudKit option is in a selected state, then select the iCloud container with your bundle identifier from step 1 from the Containers list. If the container doesn’t exist, click the Add button (+), enter the container name (iCloud.<_bundle identifier_\>), and click OK to let Xcode create the container and associate it with the app.
    
4.  If you prefer to use a different container, select it from the Containers list, and specify the container identifier when creating the `container` variable in the `AppDelegate` class. An iCloud container identifier is case-sensitive and must begin with “`iCloud.`”.
    

Before running the sample on a device, configure the device as follows:

1.  Log in with an Apple ID. For the CloudKit private database to synchronize across devices, the Apple ID must be the same on the devices.
    
2.  Choose Settings > Apple ID > iCloud, and turn on iCloud Drive, if it is off.
    

### [Create a CloudKit Schema for the App](/documentation/cloudkit/sharing-cloudkit-data-with-other-icloud-users#Create-a-CloudKit-Schema-for-the-App)

CloudKit apps need to create a schema to define the record types and fields, and [CloudKit Dashboard](http://icloud.developer.apple.com/dashboard/) is the tool for doing that. For more information, see [Inspecting and Editing an iCloud Container’s Schema](/documentation/cloudkit/inspecting-and-editing-an-icloud-container-s-schema).

The sample uses the following record types and fields:

```
Topic
    name (String)
Note
    title (String)
    topic (Reference, pointing to the parent topic)
```

In this instance, there is no need to manually create the schema before running the sample because:

-   When an app saves a record in the development environment, CloudKit automatically creates the corresponding record type if it doesn’t exist. For more information, see [Designing and Creating a CloudKit Database](/documentation/cloudkit/designing-and-creating-a-cloudkit-database).
    

cloudkit/designing-and-creating-a-cloudkit-database

-   Before saving a record, the sample doesn’t use any record-type information.
    

For real-world apps that use record types at an earlier phase, like creating a [`CKQuerySubscription`](/documentation/CloudKit/CKQuerySubscription) at the beginning of a launch session, the schema must be ready first.

### [Create and Share a Topic](/documentation/cloudkit/sharing-cloudkit-data-with-other-icloud-users#Create-and-Share-a-Topic)

To create and share a topic with another iCloud user using the sample, follow these steps:

1.  Prepare two devices, A and B, and log in to each device with a different iCloud account.
    
2.  Use Xcode to build and run the sample app on the devices. If the system shows an alert that requests permission to use notifications, allow it.
    
3.  On device A, tap the Zones button to show the zone view, then tap the Edit button and add a zone in the private database.
    
4.  Tap the new zone to navigate to the topic view, then tap the Edit button and add a topic. Each topic has a Share button on the right.
    
5.  Tap the Share button to show the CloudKit sharing UI, then follow the UI to send a link to the iCloud account for device B. Try to use Messages because it’s easier to set up.
    
6.  After receiving the link on device B, tap it to accept and open the share. The sample app launches, and then the shared topic and its zone appear in the shared database.
    

To discover more features in the CloudKit sharing UI:

-   On device A, find the shared topic and tap the Share button. Because it’s a shared topic, the sharing UI allows users to stop sharing or to change the permission for a participant.
    
-   On device B, tap the Share button of the accepted topic. On the participant side, the sharing UI allows users to remove their participation from the topic.
    
-   On device B, navigate to the shared database’s topic view, then tap the Edit button and add a note under the shared topic. The new note synchronizes within seconds to the private database on device A. (This assumes the topic’s “Anyone with this link can make changes” option is in an enabled state. If the topic’s “Anyone with this link can view” option is in an enabled state, participants have read-only permissions, and can’t add a note under the topic.)
    
-   On device A, add a note under the shared topic. The note synchronizes within seconds to device B. When creating a note, the sample sets its `parent` property to the topic, so the system automatically shares the note with its parent topic.
    

```
newNoteRecord.parent = CKRecord.Reference(record: topicRecord, action: .none)
```

### [Share a Record](/documentation/cloudkit/sharing-cloudkit-data-with-other-icloud-users#Share-a-Record)

The sample uses [`UICloudSharingController`](/documentation/UIKit/UICloudSharingController) to implement the sharing flow. Depending on whether the root record is in a shared state, there are different ways to create a sharing controller.

```
if rootRecord.share != nil {
    newSharingController(sharedRootRecord: rootRecord, database: database,
                         completionHandler: completionHandler)
} else {
    newSharingController(unsharedRootRecord: rootRecord, database: database, zone: zone,
                         completionHandler: completionHandler)
}
```

If the root record is in a shared state, the sample grabs the `recordID` from the [`share`](/documentation/cloudkit/ckrecord/share) property of the root record, uses it to fetch the share, which is the associated [`CKShare`](/documentation/CloudKit/CKShare) object, from the server, and calls [`init(share:container:)`](/documentation/UIKit/UICloudSharingController/init\(share:container:\)) to create a sharing controller.

```
let sharingController = UICloudSharingController(share: share, container: self)
```

If the root record isn’t in a shared state, the sample uses [`init(preparationHandler:)`](/documentation/UIKit/UICloudSharingController/init\(preparationHandler:\)) to create the sharing controller.

```
let sharingController = UICloudSharingController { (_, prepareCompletionHandler) in
```

In the preparation handler, the sample sets up a new `CKShare` object using the root record.

```
let shareID = CKRecord.ID(recordName: UUID().uuidString, zoneID: zone.zoneID)
var share = CKShare(rootRecord: unsharedRootRecord, shareID: shareID)
share[CKShare.SystemFieldKey.title] = "A cool topic to share!" as CKRecordValue
share.publicPermission = .readWrite
```

The sample then saves the share and its root record together using [`CKModifyRecordsOperation`](/documentation/CloudKit/CKModifyRecordsOperation).

```
let modifyRecordsOp = CKModifyRecordsOperation(recordsToSave: [share, unsharedRootRecord], recordIDsToDelete: nil)
```

After creating the sharing controller, the sample uses the following code to present it:

```
sharingController.popoverPresentationController?.sourceView = sender as? UIView
self.rootRecord = topicRecord
sharingController.delegate = self
sharingController.availablePermissions = [.allowPublic, .allowReadOnly, .allowReadWrite]
self.present(sharingController, animated: true) { self.spinner.stopAnimating() }
```

Using the sharing UI, users can send a link, stop sharing the record, change the permission for a participant, or quit the flow by closing the UI. According to what users do, the sharing controller may change the root record and its share, and notify the app through the [`UICloudSharingControllerDelegate`](/documentation/UIKit/UICloudSharingControllerDelegate) protocol. To ensure the cached data is consistent with the server truth, the sample implements the following delegate methods:

-   [`cloudSharingControllerDidSaveShare(_:)`](/documentation/UIKit/UICloudSharingControllerDelegate/cloudSharingControllerDidSaveShare\(_:\)) — CloudKit calls this method when it successfully shares a topic. When this happens, it creates the share and updates the shared topic and notes on the server, so the sample fetches the changes and updates the local cache.
    
-   [`cloudSharingControllerDidStopSharing(_:)`](/documentation/UIKit/UICloudSharingControllerDelegate/cloudSharingControllerDidStopSharing\(_:\)) — CloudKit calls this method when users stop sharing a record. When this happens, it removes the share and updates the shared topic and notes on the server, so the sample fetches the changes and updates the local cache.
    
-   [`cloudSharingController(_:failedToSaveShareWithError:)`](/documentation/UIKit/UICloudSharingControllerDelegate/cloudSharingController\(_:failedToSaveShareWithError:\)) — CloudKit calls this method when the sharing controller fails to save a share. When this happens, the sample alerts the error and updates the cached root record to avoid an inconsistent status.
    

### [Maintain a Local Cache of CloudKit Records](/documentation/cloudkit/sharing-cloudkit-data-with-other-icloud-users#Maintain-a-Local-Cache-of-CloudKit-Records)

To avoid fetching data from the server each time the zone view and topic view are about to appear, the sample caches the zones in the container, and the topics and notes in the current zone. The caches are both in-memory because the sample doesn’t tend to add more complexity by introducing a persistence layer. Real-world apps can persist their cache to avoid doing an initial fetch on each launch.

The sample establishes the local caches with two steps: initial fetch and incremental update. In [`sceneWillEnterForeground(_:)`](/documentation/UIKit/UISceneDelegate/sceneWillEnterForeground\(_:\)), the sample checks the account status, and then starts the initial fetch if there isn’t a cache for the current account.

```
let building = appDelegate.buildZoneCacheIfNeed(for: newUserRecordID)
```

CloudKit notifications trigger the incremental updates. The sample uses [`CKDatabaseSubscription`](/documentation/CloudKit/CKDatabaseSubscription) to subscribe to CloudKit database changes.

```
let subscription = CKDatabaseSubscription(subscriptionID: subscriptionID)
let notificationInfo = CKSubscription.NotificationInfo()
notificationInfo.shouldBadge = true
notificationInfo.alertBody = "Database (\(subscriptionID)) was changed!"
subscription.notificationInfo = notificationInfo


let operation = CKModifySubscriptionsOperation(subscriptionsToSave: [subscription], subscriptionIDsToDelete: nil)
operation.modifySubscriptionsCompletionBlock = { _, _, error in
    completionHandler(error as NSError?)
}


add(operation, to: operationQueue)
```

With the subscriptions, the sample gets push notifications when the database changes, and starts the incremental update from the following [`UNUserNotificationCenterDelegate`](/documentation/UserNotifications/UNUserNotificationCenterDelegate) method:

```
func userNotificationCenter(_ center: UNUserNotificationCenter, willPresent notification: UNNotification,
                            withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
    print("\(#function)")
    updateWithNotificationUserInfo(notification.request.content.userInfo)
    completionHandler([])
}
```

The sample uses [`CKFetchDatabaseChangesOperation`](/documentation/cloudkit/ckfetchdatabasechangesoperation) to fetch the deleted or changed zones. When doing the fetch, CloudKit provides a `serverChangeToken` ([`CKServerChangeToken`](/documentation/CloudKit/CKServerChangeToken)) by calling the operation’s [`changeTokenUpdatedBlock`](/documentation/cloudkit/ckfetchdatabasechangesoperation/changetokenupdatedblock). Apps keep the token and use it as `previousServerChangeToken` for the next fetch.

```
operation.changeTokenUpdatedBlock = { serverChangeToken in
    self.setServerChangeToken(newToken: serverChangeToken, cloudKitDB: cloudKitDB)
}
```

When apps use the token to create and run a CloudKit operation, the token tells the server which portions of the zones to return. If the token is `nil`, the server returns all zones.

```
let serverChangeToken = getServerChangeToken(for: cloudKitDB)
let operation = CKFetchDatabaseChangesOperation(previousServerChangeToken: serverChangeToken)
```

After gathering the deleted and changed zones, the sample updates the zone cache and makes it consistent with the server truth.

Similarly, the sample uses [`CKFetchRecordZoneChangesOperation`](/documentation/CloudKit/CKFetchRecordZoneChangesOperation) to gather the deleted and changed topic and notes, and uses them to maintain the topic cache.

```
let configuration = CKFetchRecordZoneChangesOperation.ZoneConfiguration()
configuration.previousServerChangeToken = getServerChangeToken()


let operation = CKFetchRecordZoneChangesOperation(
    recordZoneIDs: [zone.zoneID], configurationsByRecordZoneID: [zone.zoneID: configuration]
)
```

To avoid blocking an app’s main queue, CloudKit operations and their callbacks must run on a secondary queue, which can be an app-provided operation queue ([`OperationQueue`](/documentation/Foundation/OperationQueue)), or a private operation queue that CloudKit manages. The sample provides an operation queue to run CloudKit operations and update the cached data when the operations complete. This means the system can access the cached data from different queues: the app’s main queue that reads the data and updates the app UI, and a secondary queue that runs CloudKit operations and updates the data.

To be thread-safe, the sample serializes data access with a dispatch queue ([`DispatchQueue`](/documentation/Dispatch/DispatchQueue)). One caveat of this solution is when the main queue needs to read the cached data while the secondary queue is updating it, the main queue must wait until the data update finishes. If the update takes a long time, it blocks the main queue for a long time, which leads to UI unresponsiveness. Real-world apps using the same method to serialize data access need to update the data quickly enough to avoid this issue.

## [See Also](/documentation/cloudkit/sharing-cloudkit-data-with-other-icloud-users#see-also)

### [Collaboration](/documentation/cloudkit/sharing-cloudkit-data-with-other-icloud-users#Collaboration)

[

Sharing Core Data objects between iCloud users](/documentation/CoreData/sharing-core-data-objects-between-icloud-users)

Use Core Data and CloudKit to synchronize data between devices of an iCloud user and share data between different iCloud users.

[`class CKShare`](/documentation/cloudkit/ckshare)

A specialized record type that manages a collection of shared records.

[`struct CKShareTransferRepresentation`](/documentation/cloudkit/cksharetransferrepresentation)

A transfer representation the system uses to share an item.

[`class CKAllowedSharingOptions`](documentation/cloudkit/CKAllowedSharingOptions.md)

An object that controls participant access and permission options.

[`class CKSystemSharingUIObserver`](/documentation/cloudkit/cksystemsharinguiobserver)

An object the system uses to monitor changes in sharing.

[`@MainActor class UICloudSharingController`](/documentation/UIKit/UICloudSharingController)

A view controller that presents standard screens for adding and removing people from a CloudKit share record.

[`CKSharingSupported`](/documentation/BundleResources/Information-Property-List/CKSharingSupported)

A Boolean value that indicates your app supports CloudKit Sharing.

Current page is Sharing CloudKit Data with Other iCloud Users

---

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

## [See Also](/documentation/coredata/sharing-core-data-objects-between-icloud-users#see-also)

### [CloudKit mirroring](/documentation/coredata/sharing-core-data-objects-between-icloud-users#CloudKit-mirroring)

[

Mirroring a Core Data store with CloudKit](/documentation/coredata/mirroring-a-core-data-store-with-cloudkit)

Back user interfaces with a local replica of a CloudKit private database.

[

Synchronizing a local store to the cloud](/documentation/coredata/synchronizing-a-local-store-to-the-cloud)

Share data between a user’s devices and other iCloud users.

[`class NSPersistentCloudKitContainer`](/documentation/coredata/nspersistentcloudkitcontainer)

A container that encapsulates the Core Data stack in your app, and mirrors select persistent stores to a CloudKit private database.

[`class NSPersistentCloudKitContainerOptions`](/documentation/coredata/nspersistentcloudkitcontaineroptions)

An object that customizes how a store description aligns with a CloudKit database.

Current page is Sharing Core Data objects between iCloud users