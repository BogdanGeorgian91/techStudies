# CKRecordZoneSubscription | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKRecordZoneSubscription

Class

# CKRecordZoneSubscription

A subscription that generates push notifications when CloudKit modifies records in a specific record zone.

```
class CKRecordZoneSubscription
```

## [Overview](/documentation/cloudkit/ckrecordzonesubscription#overview)

Subscriptions track the creation, modification, and deletion of records in a database, and are fundamental in keeping data on the user’s device up to date. A subscription applies only to the user that creates it. When a subscription registers a change, such as CloudKit saving a new record, it sends push notifications to the user’s devices to inform your app about the change. You can then fetch the changes and cache them on-device. When appropriate, the server excludes the device where the change originates.

Record zone subscriptions execute whenever a change happens in the record zone you specify when you create the subscription. You can further specialize the subscription by setting its [`recordType`](/documentation/cloudkit/ckdatabasesubscription/recordtype-46v7a) property to a specific record type. This limits the scope of the subscription to only track changes to records of that type and reduces the number of notifications it generates.

Create any subscriptions on your app’s first launch. After you initialize a subscription, save it to the server using [`CKModifySubscriptionsOperation`](/documentation/cloudkit/ckmodifysubscriptionsoperation). When the operation completes, record that state on-device (in [`UserDefaults`](/documentation/Foundation/UserDefaults), for example). You can then check that state on subsequent launches to prevent unnecessary trips to the server.

To configure the notification that the subscription generates, set the subscription’s [`notificationInfo`](/documentation/cloudkit/cksubscription/notificationinfo-swift.property) property. Because the system coalesces notifications, don’t rely on them for specific changes. CloudKit can omit data to keep the payload size under the APNs size limit. Consider notifications an indication of remote changes and use [`CKFetchRecordZoneChangesOperation`](/documentation/cloudkit/ckfetchrecordzonechangesoperation) to fetch the changed records. Server change tokens allow you to limit the fetch results to just the changes since your previous fetch.

The example below shows how to create a record zone subscription in the user’s private database, configure the notifications it generates — in this case, silent push notifications — and then save that subscription to the server:

```
// Only proceed if the subscription doesn't already exist.
if([[NSUserDefaults standardUserDefaults]
    boolForKey:@"didCreateFeedSubscription"] == NO) {
        
    // Create a subscription that's scoped to a specific record zone. Provide
    // a subscription ID that's unique within the context of the user's 
    // private database.
    CKRecordZoneSubscription *subscription =
    [[CKRecordZoneSubscription alloc]
     initWithZoneID:recordZone.zoneID
     subscriptionID:@"feed-changes"];
        
    // Scope the subscription to just the 'FeedItem' record type.
    subscription.recordType = @"FeedItem";
        
    // Configure the notification so that the system delivers it silently
    // and therefore doesn't require permission from the user.
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

## [Topics](/documentation/cloudkit/ckrecordzonesubscription#topics)

### [Creating a Zone-Based Subscription](/documentation/cloudkit/ckrecordzonesubscription#Creating-a-Zone-Based-Subscription)

[`convenience init(zoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckrecordzonesubscription/init\(zoneid:\))

Creates a subscription for all records in the specified record zone.

Deprecated

[`convenience init(zoneID: CKRecordZone.ID, subscriptionID: CKSubscription.ID)`](/documentation/cloudkit/ckrecordzonesubscription/init\(zoneid:subscriptionid:\))

Creates a named subscription for all records in the specified record zone.

[`init(coder: NSCoder)`](/documentation/cloudkit/ckrecordzonesubscription/init\(coder:\))

Creates a zone-based subscription from a serialized instance.

### [Accessing the Subscription Metadata](/documentation/cloudkit/ckrecordzonesubscription#Accessing-the-Subscription-Metadata)

[`var recordType: CKRecord.RecordType?`](/documentation/cloudkit/ckrecordzonesubscription/recordtype-1fuqo)

The type of record that the subscription queries.

[`var zoneID: CKRecordZone.ID`](/documentation/cloudkit/ckrecordzonesubscription/zoneid)

The ID of the record zone that the subscription queries.

## [Relationships](/documentation/cloudkit/ckrecordzonesubscription#relationships)

### [Inherits From](/documentation/cloudkit/ckrecordzonesubscription#inherits-from)

-   [`CKSubscription`](/documentation/cloudkit/cksubscription)

### [Conforms To](/documentation/cloudkit/ckrecordzonesubscription#conforms-to)

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

## [See Also](/documentation/cloudkit/ckrecordzonesubscription#see-also)

### [Record Zone Changes](/documentation/cloudkit/ckrecordzonesubscription#Record-Zone-Changes)

[`class CKRecordZoneNotification`](/documentation/cloudkit/ckrecordzonenotification)

A notification that triggers when the contents of a record zone change.

[`class CKFetchRecordZoneChangesOperation`](/documentation/cloudkit/ckfetchrecordzonechangesoperation)

An operation that fetches record zone changes.

Current page is CKRecordZoneSubscription

---
  
Creating a Zone-Based Subscription

# init(zoneID:subscriptionID:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZoneSubscription](/documentation/cloudkit/ckrecordzonesubscription)
-   init(zoneID:subscriptionID:)

Initializer

# init(zoneID:subscriptionID:)

Creates a named subscription for all records in the specified record zone.

```
convenience init(
    zoneID: CKRecordZone.ID,
    subscriptionID: CKSubscription.ID
)
```

## [Parameters](/documentation/cloudkit/ckrecordzonesubscription/init\(zoneid:subscriptionid:\)#parameters)

`zoneID`

The ID of the record zone that contains the records you want to monitor.

`subscriptionID`

The subscription’s name. It must be unique in the container and must not be an empty string.

## [Discussion](/documentation/cloudkit/ckrecordzonesubscription/init\(zoneid:subscriptionid:\)#Discussion)

The subscription that this method returns is a zone-based subscription that generates push notifications when CloudKit changes any of the specificed record zone’s records.

## [See Also](/documentation/cloudkit/ckrecordzonesubscription/init\(zoneid:subscriptionid:\)#see-also)

### [Creating a Zone-Based Subscription](/documentation/cloudkit/ckrecordzonesubscription/init\(zoneid:subscriptionid:\)#Creating-a-Zone-Based-Subscription)

[`convenience init(zoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckrecordzonesubscription/init\(zoneid:\))

Creates a subscription for all records in the specified record zone.

Deprecated

[`init(coder: NSCoder)`](/documentation/cloudkit/ckrecordzonesubscription/init\(coder:\))

Creates a zone-based subscription from a serialized instance.

Current page is init(zoneID:subscriptionID:)

# init(coder:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZoneSubscription](/documentation/cloudkit/ckrecordzonesubscription)
-   init(coder:)

Initializer

# init(coder:)

Creates a zone-based subscription from a serialized instance.

```
init(coder aDecoder: NSCoder)
```

## [Parameters](/documentation/cloudkit/ckrecordzonesubscription/init\(coder:\)#parameters)

`aDecoder`

The coder for decoding the serialized record zone subscription.

## [See Also](/documentation/cloudkit/ckrecordzonesubscription/init\(coder:\)#see-also)

### [Creating a Zone-Based Subscription](/documentation/cloudkit/ckrecordzonesubscription/init\(coder:\)#Creating-a-Zone-Based-Subscription)

[`convenience init(zoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckrecordzonesubscription/init\(zoneid:\))

Creates a subscription for all records in the specified record zone.

Deprecated

[`convenience init(zoneID: CKRecordZone.ID, subscriptionID: CKSubscription.ID)`](/documentation/cloudkit/ckrecordzonesubscription/init\(zoneid:subscriptionid:\))

Creates a named subscription for all records in the specified record zone.

Current page is init(coder:)

---
### Accessing the Subscription Metadata

# recordType | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZoneSubscription](/documentation/cloudkit/ckrecordzonesubscription)
-   recordType

Instance Property

# recordType

The type of record that the subscription queries.

```
var recordType: CKRecord.RecordType? { get set }
```

## [Discussion](/documentation/cloudkit/ckrecordzonesubscription/recordtype-1fuqo#Discussion)

This property only applies to query-based subscriptions. CloudKit sets its value when you create the subscription. For all other types of subscription, CloudKit ignores this property and sets its value to `nil`.

## [See Also](/documentation/cloudkit/ckrecordzonesubscription/recordtype-1fuqo#see-also)

### [Accessing the Subscription Metadata](/documentation/cloudkit/ckrecordzonesubscription/recordtype-1fuqo#Accessing-the-Subscription-Metadata)

[`var zoneID: CKRecordZone.ID`](/documentation/cloudkit/ckrecordzonesubscription/zoneid)

The ID of the record zone that the subscription queries.

Current page is recordType

# zoneID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZoneSubscription](/documentation/cloudkit/ckrecordzonesubscription)
-   zoneID

Instance Property

# zoneID

The ID of the record zone that the subscription queries.

```
@NSCopying
var zoneID: CKRecordZone.ID { get }
```

## [Discussion](/documentation/cloudkit/ckrecordzonesubscription/zoneid#Discussion)

This property applies to query-based subscriptions and zone-based subscriptions. Specifying a record zone ID limits the scope of the query to only the records in that zone. For zone-based subscriptions, the query includes all records in the specified record zone. For a query-based subscription, the query includes only records of a specific type in the specified record zone.

For zone-based subscriptions, CloudKit sets this property’s value automatically. For all other subscription types, the default value is `nil`. If you want to scope your query-based subscription to a specific record zone, you must assign a value explicitly.

## [See Also](/documentation/cloudkit/ckrecordzonesubscription/zoneid#see-also)

### [Accessing the Subscription Metadata](/documentation/cloudkit/ckrecordzonesubscription/zoneid#Accessing-the-Subscription-Metadata)

[`var recordType: CKRecord.RecordType?`](/documentation/cloudkit/ckrecordzonesubscription/recordtype-1fuqo)

The type of record that the subscription queries.

Current page is zoneID