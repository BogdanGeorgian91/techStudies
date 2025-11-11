# CKDatabaseSubscription | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKDatabaseSubscription

Class

# CKDatabaseSubscription

A subscription that generates push notifications when CloudKit modifies records in a database.

```
class CKDatabaseSubscription
```

## [Overview](/documentation/cloudkit/ckdatabasesubscription#overview)

Subscriptions track the creation, modification, and deletion of records in a database, and are fundamental in keeping data on the user’s device up to date. A subscription applies only to the user that creates it. When a subscription registers a change, such as CloudKit saving a new record, it sends push notifications to the user’s devices to inform your app about the change. You can then fetch the changes and cache them on-device. When appropriate, the server excludes the device where the change originates.

A database subscription executes whenever a change occurs in a custom record zone that resides in the database where you save the subscription. This is important for the shared database because you don’t know what record zones exist in advance. The only exception to this is the default record zone in the user’s private database, which doesn’t participate in database subscriptions.

You can further specialize a database subscription by setting its [`recordType`](/documentation/cloudkit/ckdatabasesubscription/recordtype-46v7a) property to a specific record type. This limits the scope of the subscription to only track changes to records of that type and reduces the number of notifications it generates.

Create any subscriptions on your app’s first launch. After you initialize a subscription, save it to the server using [`CKModifySubscriptionsOperation`](/documentation/cloudkit/ckmodifysubscriptionsoperation). After the operation completes, record that state on-device (in [`UserDefaults`](/documentation/Foundation/UserDefaults), for example). You can then check that state on subsequent launches to prevent unnecessary trips to the server.

To configure the notification that the subscription generates, set the subscription’s [`notificationInfo`](/documentation/cloudkit/cksubscription/notificationinfo-swift.property) property. Because the system coalesces notifications, don’t rely on them for specific changes. CloudKit can omit data to keep the payload size under the APNs size limit. Consider notifications an indication of remote changes, and use [`CKFetchDatabaseChangesOperation`](/documentation/cloudkit/ckfetchdatabasechangesoperation) to fetch the record zones that contain those changes. After you have the record zones, use [`CKFetchRecordZoneChangesOperation`](/documentation/cloudkit/ckfetchrecordzonechangesoperation) to fetch the changed records in each zone. Server change tokens allow you to limit the fetch results to just the changes since your previous fetch.

The example below shows how to create a database subscription in the user’s private database, configure the notifications it generates — in this case, silent push notifications — and then save that subscription to the server:

```
// Only proceed if the subscription doesn't already exist.
if([[NSUserDefaults standardUserDefaults]
    boolForKey:@"didCreateFeedSubscription"] == NO) {
        
    // Create a subscription with an ID that's unique within the scope of
    // the user's private database.
    CKDatabaseSubscription *subscription = 
        [[CKDatabaseSubscription alloc]
         initWithSubscriptionID:@"feed-changes"];
        
    // Scope the subscription to just the 'FeedItem' record type.
    subscription.recordType = @"FeedItem";
        
    // Configure the notification so that the system delivers it silently
    // and, therefore, doesn't require permission from the user.
    CKNotificationInfo *notificationInfo = [CKNotificationInfo new];
    notificationInfo.shouldSendContentAvailable = YES;
    subscription.notificationInfo = notificationInfo;
        
    // Create an operation that saves the subscription to the server.
    CKModifySubscriptionsOperation *operation = 
        [[CKModifySubscriptionsOperation alloc]
         initWithSubscriptionsToSave:@[subscription]
         subscriptionIDsToDelete:NULL];
    
    operation.modifySubscriptionsCompletionBlock = 
        ^(NSArray *subscriptions, NSArray *deleted, NSError *error) {
        if (error) {
            // Handle the error.
        } else {
            // Record that the system successfully creates the subscription
            // to prevent unnecessary trips to the server in later launches.
            [[NSUserDefaults standardUserDefaults] 
             setBool:YES forKey:@"didCreateFeedSubscription"];
        }
    };
        
    // Set an appropriate QoS and add the operation to the private
    // database's operation queue to execute it.
    operation.qualityOfService = NSQualityOfServiceUtility;
    [CKContainer.defaultContainer.privateCloudDatabase addOperation:operation];
}
```

## [Topics](/documentation/cloudkit/ckdatabasesubscription#topics)

### [Creating a Database Subscription](/documentation/cloudkit/ckdatabasesubscription#Creating-a-Database-Subscription)

[`convenience init()`](/documentation/cloudkit/ckdatabasesubscription/init\(\))

Creates an empty database subscription.

Deprecated

[`convenience init(subscriptionID: CKSubscription.ID)`](/documentation/cloudkit/ckdatabasesubscription/init\(subscriptionid:\))

Creates a named subscription for all records in a database.

[`init(coder: NSCoder)`](/documentation/cloudkit/ckdatabasesubscription/init\(coder:\))

Creates a database subscription from a serialized instance.

### [Accessing the Subscription Metadata](/documentation/cloudkit/ckdatabasesubscription#Accessing-the-Subscription-Metadata)

[`var recordType: CKRecord.RecordType?`](/documentation/cloudkit/ckdatabasesubscription/recordtype-46v7a)

The type of record that the subscription queries.

## [Relationships](/documentation/cloudkit/ckdatabasesubscription#relationships)

### [Inherits From](/documentation/cloudkit/ckdatabasesubscription#inherits-from)

-   [`CKSubscription`](/documentation/cloudkit/cksubscription)

### [Conforms To](/documentation/cloudkit/ckdatabasesubscription#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSCoding`](/documentation/Foundation/NSCoding)
-   [`NSCopying`](/documentation/Foundation/NSCopying)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`NSSecureCoding`](/documentation/Foundation/NSSecureCoding)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/ckdatabasesubscription#see-also)

### [Database Changes](/documentation/cloudkit/ckdatabasesubscription#Database-Changes)

[`class CKDatabaseNotification`](/documentation/cloudkit/ckdatabasenotification)

A notification that triggers when the contents of a database change.

[`class CKFetchDatabaseChangesOperation`](/documentation/cloudkit/ckfetchdatabasechangesoperation)

An operation that fetches database changes.

Current page is CKDatabaseSubscription

---
# init(subscriptionID:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabaseSubscription](/documentation/cloudkit/ckdatabasesubscription)
-   init(subscriptionID:)

Initializer

# init(subscriptionID:)

Creates a named subscription for all records in a database.

```
convenience init(subscriptionID: CKSubscription.ID)
```

## [Parameters](/documentation/cloudkit/ckdatabasesubscription/init\(subscriptionid:\)#parameters)

`subscriptionID`

The subscription’s name. It must be unique in the container, and must not be an empty string.

## [See Also](/documentation/cloudkit/ckdatabasesubscription/init\(subscriptionid:\)#see-also)

### [Creating a Database Subscription](/documentation/cloudkit/ckdatabasesubscription/init\(subscriptionid:\)#Creating-a-Database-Subscription)

[`convenience init()`](/documentation/cloudkit/ckdatabasesubscription/init\(\))

Creates an empty database subscription.

Deprecated

[`init(coder: NSCoder)`](/documentation/cloudkit/ckdatabasesubscription/init\(coder:\))

Creates a database subscription from a serialized instance.

Current page is init(subscriptionID:)

# init(coder:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabaseSubscription](/documentation/cloudkit/ckdatabasesubscription)
-   init(coder:)

Initializer

# init(coder:)

Creates a database subscription from a serialized instance.

```
init(coder aDecoder: NSCoder)
```

## [Parameters](/documentation/cloudkit/ckdatabasesubscription/init\(coder:\)#parameters)

`aDecoder`

The object that decodes the serialized database subscription.

## [See Also](/documentation/cloudkit/ckdatabasesubscription/init\(coder:\)#see-also)

### [Creating a Database Subscription](/documentation/cloudkit/ckdatabasesubscription/init\(coder:\)#Creating-a-Database-Subscription)

[`convenience init()`](/documentation/cloudkit/ckdatabasesubscription/init\(\))

Creates an empty database subscription.

Deprecated

[`convenience init(subscriptionID: CKSubscription.ID)`](/documentation/cloudkit/ckdatabasesubscription/init\(subscriptionid:\))

Creates a named subscription for all records in a database.

Current page is init(coder:)

---
  
Accessing the Subscription Metadata

# recordType | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabaseSubscription](/documentation/cloudkit/ckdatabasesubscription)
-   recordType

Instance Property

# recordType

The type of record that the subscription queries.

```
var recordType: CKRecord.RecordType? { get set }
```

Current page is recordType