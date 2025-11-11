# CKQuerySubscription | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKQuerySubscription

Class

# CKQuerySubscription

A subscription that generates push notifications when CloudKit modifies records that match a predicate.

```
class CKQuerySubscription
```

## [Overview](/documentation/cloudkit/ckquerysubscription#overview)

Subscriptions track the creation, modification, and deletion of records in a database, and are fundamental in keeping data on the user’s device up to date. A subscription applies only to the user that creates it. When a subscription registers a change, such as CloudKit saving a new record, it sends push notifications to the user’s devices to inform your app about the change. You can then fetch the changes and cache them on-device. When appropriate, the server excludes the device where the change originates.

Query subscriptions execute whenever a change occurs in a database that matches the predicate and options you specify. You scope a query subscription to an individual record type that you provide during initialization. You can set the subscription’s [`zoneID`](/documentation/cloudkit/ckquerysubscription/zoneid) property to further specialize the subscription to a specific record zone in the database. This limits the scope of the subscription to only track changes in that record zone and reduces the number of notifications it generates. For more information about defining CloudKit-compatible predicates, see [`CKQuery`](/documentation/cloudkit/ckquery).

Create any subscriptions on your app’s first launch. After you initialize a subscription, save it to the server using [`CKModifySubscriptionsOperation`](/documentation/cloudkit/ckmodifysubscriptionsoperation). When the operation completes, record that state on-device (in [`UserDefaults`](/documentation/Foundation/UserDefaults), for example). You can then check that state on subsequent launches to prevent unnecessary trips to the server.

To configure the notification the subscription generates, set the subscription’s [`notificationInfo`](/documentation/cloudkit/cksubscription/notificationinfo-swift.property) property. Because the system coalesces notifications, don’t rely on them for specific changes. CloudKit can omit data to keep the payload size under the APNs size limit. Consider notifications an indication of remote changes and use [`CKQueryOperation`](/documentation/cloudkit/ckqueryoperation) to fetch the changed records. Create the operation with an instance of [`CKQuery`](/documentation/cloudkit/ckquery) that you configure with the same record type and predicate as the subscription. If you limit the subscription to a specific record zone, set the operation’s [`zoneID`](/documentation/cloudkit/ckqueryoperation/zoneid) property to that record zone’s ID. Because [`CKQueryOperation`](/documentation/cloudkit/ckqueryoperation) doesn’t employ server change tokens, dispose of any records you cache on-device and use the query’s results instead.

The example below shows how to create a query subscription in the user’s private database, configure the notifications it generates — in this case, silent push notifications — and then save that subscription to the server:

```
// Only proceed if the subscription doesn't already exist.
if([[NSUserDefaults standardUserDefaults]
    boolForKey:@"didCreateQuerySubscription"] == NO) {
        
// Define a predicate that matches records with a tags field
// that contains the word 'Swift'.
NSPredicate *predicate = [NSPredicate predicateWithFormat:
                          @"tags CONTAINS 'Swift'"];
        
// Create a subscription and scope it to the 'FeedItem' record type.
// Provide a unique identifier for the subscription and declare the
// circumstances for invoking it.
CKQuerySubscriptionOptions options = 
    CKQuerySubscriptionOptionsFiresOnRecordCreation;
CKQuerySubscription *subscription = [[CKQuerySubscription alloc]
                                     initWithRecordType:@"FeedItem"
                                     predicate:predicate
                                     subscriptionID:@"tagged-feed-changes"
                                     options:options];
        
// Further specialize the subscription to only evaluate
// records in a specific record zone
subscription.zoneID = recordZone.zoneID;
        
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
         setBool:YES forKey:@"didCreateQuerySubscription"];
    }
};
        
// Set an appropriate QoS and add the operation to the private
// database's operation queue to execute it.
operation.qualityOfService = NSQualityOfServiceUtility;
[CKContainer.defaultContainer.privateCloudDatabase addOperation:operation];
```

## [Topics](/documentation/cloudkit/ckquerysubscription#topics)

### [Creating a Subscription](/documentation/cloudkit/ckquerysubscription#Creating-a-Subscription)

[`convenience init(recordType: CKRecord.RecordType, predicate: NSPredicate, subscriptionID: CKSubscription.ID, options: CKQuerySubscription.Options)`](/documentation/cloudkit/ckquerysubscription/init\(recordtype:predicate:subscriptionid:options:\))

Creates a named query-based subscription that queries records of a specific type.

[`init(coder: NSCoder)`](/documentation/cloudkit/ckquerysubscription/init\(coder:\))

Creates a query-based subscription from a serialized instance.

### [Accessing the Subscription Search Parameters](/documentation/cloudkit/ckquerysubscription#Accessing-the-Subscription-Search-Parameters)

[`var predicate: NSPredicate`](/documentation/cloudkit/ckquerysubscription/predicate)

The matching criteria to apply to records.

[`var querySubscriptionOptions: CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/querysubscriptionoptions)

Options that define the behavior of the subscription.

[`struct Options`](/documentation/cloudkit/ckquerysubscription/options)

Configuration options for a query subscription.

### [Accessing the Subscription Metadata](/documentation/cloudkit/ckquerysubscription#Accessing-the-Subscription-Metadata)

[`var recordType: CKRecord.RecordType?`](/documentation/cloudkit/ckquerysubscription/recordtype-4qgdo)

The type of record that the subscription queries.

[`var zoneID: CKRecordZone.ID?`](/documentation/cloudkit/ckquerysubscription/zoneid)

The ID of the record zone that the subscription queries.

### [Initializers](/documentation/cloudkit/ckquerysubscription#Initializers)

[`convenience init(recordType: CKRecord.RecordType, predicate: NSPredicate, options: CKQuerySubscription.Options)`](/documentation/cloudkit/ckquerysubscription/init\(recordtype:predicate:options:\))

## [Relationships](/documentation/cloudkit/ckquerysubscription#relationships)

### [Inherits From](/documentation/cloudkit/ckquerysubscription#inherits-from)

-   [`CKSubscription`](/documentation/cloudkit/cksubscription)

### [Conforms To](/documentation/cloudkit/ckquerysubscription#conforms-to)

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

## [See Also](/documentation/cloudkit/ckquerysubscription#see-also)

### [Predicate-Driven Changes](/documentation/cloudkit/ckquerysubscription#Predicate-Driven-Changes)

[`class CKQueryNotification`](/documentation/cloudkit/ckquerynotification)

A notification that triggers when a record that matches the subscription’s predicate changes.

Current page is CKQuerySubscription

---
  
Creating a Subscription

# init(recordType:predicate:subscriptionID:options:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQuerySubscription](/documentation/cloudkit/ckquerysubscription)
-   init(recordType:predicate:subscriptionID:options:)

Initializer

# init(recordType:predicate:subscriptionID:options:)

Creates a named query-based subscription that queries records of a specific type.

```
convenience init(
    recordType: CKRecord.RecordType,
    predicate: NSPredicate,
    subscriptionID: CKSubscription.ID,
    options querySubscriptionOptions: CKQuerySubscription.Options = [.firesOnRecordCreation, .firesOnRecordUpdate, .firesOnRecordDeletion]
)
```

## [Parameters](/documentation/cloudkit/ckquerysubscription/init\(recordtype:predicate:subscriptionid:options:\)#parameters)

`recordType`

The record’s type. You’re responsible for defining your app’s record types.

`predicate`

The predicate that identifies the records for inclusion in the subscription. For information about the operators that predicates support, see the discussion in [`CKQuery`](/documentation/cloudkit/ckquery).

`subscriptionID`

The subscription’s name. It must be unique in the target database, and must not be an empty string.

`querySubscriptionOptions`

A bitmask of configuration options. See [`CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/options) for more information.

## [Discussion](/documentation/cloudkit/ckquerysubscription/init\(recordtype:predicate:subscriptionid:options:\)#Discussion)

The subscription that this method returns is a query-based subscription with a scope that includes all of the user’s record zones. When CloudKit modifies a record that matches the specified type and predicate, it uses `querySubscriptionOptions` to determine whether to send a push notification.

## [See Also](/documentation/cloudkit/ckquerysubscription/init\(recordtype:predicate:subscriptionid:options:\)#see-also)

### [Creating a Subscription](/documentation/cloudkit/ckquerysubscription/init\(recordtype:predicate:subscriptionid:options:\)#Creating-a-Subscription)

[`init(coder: NSCoder)`](/documentation/cloudkit/ckquerysubscription/init\(coder:\))

Creates a query-based subscription from a serialized instance.

Current page is init(recordType:predicate:subscriptionID:options:)

# init(coder:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQuerySubscription](/documentation/cloudkit/ckquerysubscription)
-   init(coder:)

Initializer

# init(coder:)

Creates a query-based subscription from a serialized instance.

```
init(coder aDecoder: NSCoder)
```

## [Parameters](/documentation/cloudkit/ckquerysubscription/init\(coder:\)#parameters)

`aDecoder`

The coder for decoding the serialized query subscription.

## [See Also](/documentation/cloudkit/ckquerysubscription/init\(coder:\)#see-also)

### [Creating a Subscription](/documentation/cloudkit/ckquerysubscription/init\(coder:\)#Creating-a-Subscription)

[`convenience init(recordType: CKRecord.RecordType, predicate: NSPredicate, subscriptionID: CKSubscription.ID, options: CKQuerySubscription.Options)`](/documentation/cloudkit/ckquerysubscription/init\(recordtype:predicate:subscriptionid:options:\))

Creates a named query-based subscription that queries records of a specific type.

Current page is init(coder:)

---
  
Accessing the Subscription Search Parameters

# predicate | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQuerySubscription](/documentation/cloudkit/ckquerysubscription)
-   predicate

Instance Property

# predicate

The matching criteria to apply to records.

```
@NSCopying
var predicate: NSPredicate { get }
```

## [Discussion](/documentation/cloudkit/ckquerysubscription/predicate#Discussion)

A query-based subscription uses its search predicate to identify potential matches for records. It combines the predicate information with the value in the [`querySubscriptionOptions`](/documentation/cloudkit/ckquerysubscription/querysubscriptionoptions) property to determine when to send a push notification to the app.

The search predicate defines the records that the subscription object monitors for changes. The system only uses the property’s value when the [`subscriptionType`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.property) property is [`CKSubscription.SubscriptionType.query`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum/query). Otherwise, the system ignores it.

## [See Also](/documentation/cloudkit/ckquerysubscription/predicate#see-also)

### [Accessing the Subscription Search Parameters](/documentation/cloudkit/ckquerysubscription/predicate#Accessing-the-Subscription-Search-Parameters)

[`var querySubscriptionOptions: CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/querysubscriptionoptions)

Options that define the behavior of the subscription.

[`struct Options`](/documentation/cloudkit/ckquerysubscription/options)

Configuration options for a query subscription.

Current page is predicate

# querySubscriptionOptions | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQuerySubscription](/documentation/cloudkit/ckquerysubscription)
-   querySubscriptionOptions

Instance Property

# querySubscriptionOptions

Options that define the behavior of the subscription.

```
var querySubscriptionOptions: CKQuerySubscription.Options { get }
```

## [Discussion](/documentation/cloudkit/ckquerysubscription/querysubscriptionoptions#Discussion)

Set the value of this property at initialization time. When you configure a query-based subscription, use one of the following values:

-   [`firesOnRecordCreation`](/documentation/cloudkit/ckquerysubscription/options/firesonrecordcreation)
    
-   [`firesOnRecordUpdate`](/documentation/cloudkit/ckquerysubscription/options/firesonrecordupdate)
    
-   [`firesOnRecordDeletion`](/documentation/cloudkit/ckquerysubscription/options/firesonrecorddeletion)
    

If you don’t set an option, the system throws an [`invalidArgumentException`](/documentation/Foundation/NSExceptionName/invalidArgumentException).

## [See Also](/documentation/cloudkit/ckquerysubscription/querysubscriptionoptions#see-also)

### [Accessing the Subscription Search Parameters](/documentation/cloudkit/ckquerysubscription/querysubscriptionoptions#Accessing-the-Subscription-Search-Parameters)

[`var predicate: NSPredicate`](/documentation/cloudkit/ckquerysubscription/predicate)

The matching criteria to apply to records.

[`struct Options`](/documentation/cloudkit/ckquerysubscription/options)

Configuration options for a query subscription.

Current page is querySubscriptionOptions

---

# CKQuerySubscription.Options

### Creating Query Subscription Options

# init(rawValue:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQuerySubscription](/documentation/cloudkit/ckquerysubscription)
-   [CKQuerySubscription.Options](/documentation/cloudkit/ckquerysubscription/options)
-   init(rawValue:)

Initializer

# init(rawValue:)

Creates a query subscription option using the specified raw value.

```
init(rawValue: UInt)
```

## [Parameters](/documentation/cloudkit/ckquerysubscription/options/init\(rawvalue:\)#parameters)

`rawValue`

An integer that represents a set of combined options.

Current page is init(rawValue:)

  
### Accessing Subscription Options

# firesOnRecordCreation | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQuerySubscription](/documentation/cloudkit/ckquerysubscription)
-   [CKQuerySubscription.Options](/documentation/cloudkit/ckquerysubscription/options)
-   firesOnRecordCreation

Type Property

# firesOnRecordCreation

An option that instructs CloudKit to send a push notification when it creates a record that matches a subscription’s criteria.

```
static var firesOnRecordCreation: CKQuerySubscription.Options { get }
```

## [See Also](/documentation/cloudkit/ckquerysubscription/options/firesonrecordcreation#see-also)

### [Accessing Subscription Options](/documentation/cloudkit/ckquerysubscription/options/firesonrecordcreation#Accessing-Subscription-Options)

[`static var firesOnRecordDeletion: CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/options/firesonrecorddeletion)

An option that instructs CloudKit to send a push notification when it deletes a record that matches a subscription’s criteria.

[`static var firesOnRecordUpdate: CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/options/firesonrecordupdate)

An option that instructs CloudKit to send a push notification when it modifies a record that matches a subscription’s criteria.

[`static var firesOnce: CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/options/firesonce)

An option that instructs CloudKit to send a push notification only once.

Current page is firesOnRecordCreation

# firesOnRecordDeletion | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQuerySubscription](/documentation/cloudkit/ckquerysubscription)
-   [CKQuerySubscription.Options](/documentation/cloudkit/ckquerysubscription/options)
-   firesOnRecordDeletion

Type Property

# firesOnRecordDeletion

An option that instructs CloudKit to send a push notification when it deletes a record that matches a subscription’s criteria.

```
static var firesOnRecordDeletion: CKQuerySubscription.Options { get }
```

## [See Also](/documentation/cloudkit/ckquerysubscription/options/firesonrecorddeletion#see-also)

### [Accessing Subscription Options](/documentation/cloudkit/ckquerysubscription/options/firesonrecorddeletion#Accessing-Subscription-Options)

[`static var firesOnRecordCreation: CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/options/firesonrecordcreation)

An option that instructs CloudKit to send a push notification when it creates a record that matches a subscription’s criteria.

[`static var firesOnRecordUpdate: CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/options/firesonrecordupdate)

An option that instructs CloudKit to send a push notification when it modifies a record that matches a subscription’s criteria.

[`static var firesOnce: CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/options/firesonce)

An option that instructs CloudKit to send a push notification only once.

Current page is firesOnRecordDeletion

# firesOnRecordUpdate | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQuerySubscription](/documentation/cloudkit/ckquerysubscription)
-   [CKQuerySubscription.Options](/documentation/cloudkit/ckquerysubscription/options)
-   firesOnRecordUpdate

Type Property

# firesOnRecordUpdate

An option that instructs CloudKit to send a push notification when it modifies a record that matches a subscription’s criteria.

```
static var firesOnRecordUpdate: CKQuerySubscription.Options { get }
```

## [See Also](/documentation/cloudkit/ckquerysubscription/options/firesonrecordupdate#see-also)

### [Accessing Subscription Options](/documentation/cloudkit/ckquerysubscription/options/firesonrecordupdate#Accessing-Subscription-Options)

[`static var firesOnRecordCreation: CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/options/firesonrecordcreation)

An option that instructs CloudKit to send a push notification when it creates a record that matches a subscription’s criteria.

[`static var firesOnRecordDeletion: CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/options/firesonrecorddeletion)

An option that instructs CloudKit to send a push notification when it deletes a record that matches a subscription’s criteria.

[`static var firesOnce: CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/options/firesonce)

An option that instructs CloudKit to send a push notification only once.

Current page is firesOnRecordUpdate

# firesOnce | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQuerySubscription](/documentation/cloudkit/ckquerysubscription)
-   [CKQuerySubscription.Options](/documentation/cloudkit/ckquerysubscription/options)
-   firesOnce

Type Property

# firesOnce

An option that instructs CloudKit to send a push notification only once.

```
static var firesOnce: CKQuerySubscription.Options { get }
```

## [Discussion](/documentation/cloudkit/ckquerysubscription/options/firesonce#Discussion)

You combine this option with one or more of the other subscription options. This option applies only to query-based subscriptions. CloudKit deletes the subscription after it sends the push notification. If you want to generate subsequent push notifications using the same criteria, create and save a new subscription.

## [See Also](/documentation/cloudkit/ckquerysubscription/options/firesonce#see-also)

### [Accessing Subscription Options](/documentation/cloudkit/ckquerysubscription/options/firesonce#Accessing-Subscription-Options)

[`static var firesOnRecordCreation: CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/options/firesonrecordcreation)

An option that instructs CloudKit to send a push notification when it creates a record that matches a subscription’s criteria.

[`static var firesOnRecordDeletion: CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/options/firesonrecorddeletion)

An option that instructs CloudKit to send a push notification when it deletes a record that matches a subscription’s criteria.

[`static var firesOnRecordUpdate: CKQuerySubscription.Options`](/documentation/cloudkit/ckquerysubscription/options/firesonrecordupdate)

An option that instructs CloudKit to send a push notification when it modifies a record that matches a subscription’s criteria.

Current page is firesOnce

---
### Accessing the Subscription Metadata

# recordType | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQuerySubscription](/documentation/cloudkit/ckquerysubscription)
-   recordType

Instance Property

# recordType

The type of record that the subscription queries.

```
var recordType: CKRecord.RecordType? { get }
```

## [Discussion](/documentation/cloudkit/ckquerysubscription/recordtype-4qgdo#Discussion)

This property only applies to query-based subscriptions. CloudKit sets its value when you create the subscription. For all other types of subscription, CloudKit ignores this property and sets its value to `nil`.

## [See Also](/documentation/cloudkit/ckquerysubscription/recordtype-4qgdo#see-also)

### [Accessing the Subscription Metadata](/documentation/cloudkit/ckquerysubscription/recordtype-4qgdo#Accessing-the-Subscription-Metadata)

[`var zoneID: CKRecordZone.ID?`](/documentation/cloudkit/ckquerysubscription/zoneid)

The ID of the record zone that the subscription queries.

Current page is recordType

# zoneID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQuerySubscription](/documentation/cloudkit/ckquerysubscription)
-   zoneID

Instance Property

# zoneID

The ID of the record zone that the subscription queries.

```
@NSCopying
var zoneID: CKRecordZone.ID? { get set }
```

## [Discussion](/documentation/cloudkit/ckquerysubscription/zoneid#Discussion)

This property applies to query-based subscriptions and zone-based subscriptions. Specifying a record zone ID limits the scope of the query to only the records in that zone. For zone-based subscriptions, the query includes all records in the specified record zone. For a query-based subscription, the query includes only records of a specific type in the specified record zone.

For zone-based subscriptions, CloudKit sets this property’s value automatically. For all other subscription types, the default value is `nil`. If you want to scope your query-based subscription to a specific record zone, you must assign a value explicitly.

## [See Also](/documentation/cloudkit/ckquerysubscription/zoneid#see-also)

### [Accessing the Subscription Metadata](/documentation/cloudkit/ckquerysubscription/zoneid#Accessing-the-Subscription-Metadata)

[`var recordType: CKRecord.RecordType?`](/documentation/cloudkit/ckquerysubscription/recordtype-4qgdo)

The type of record that the subscription queries.

Current page is zoneID

initializers

# init(recordType:predicate:options:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQuerySubscription](/documentation/cloudkit/ckquerysubscription)
-   init(recordType:predicate:options:)

Initializer

# init(recordType:predicate:options:)

```
convenience init(
    recordType: CKRecord.RecordType,
    predicate: NSPredicate,
    options querySubscriptionOptions: CKQuerySubscription.Options = [.firesOnRecordCreation, .firesOnRecordUpdate, .firesOnRecordDeletion]
)
```

Current page is init(recordType:predicate:options:)



