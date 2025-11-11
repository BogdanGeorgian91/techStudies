
# CKQueryNotification | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKQueryNotification

Class

# CKQueryNotification

A notification that triggers when a record that matches the subscription’s predicate changes.

```
class CKQueryNotification
```

## [Overview](/documentation/cloudkit/ckquerynotification#overview)

Query subscriptions execute when a record that matches the subscription’s predicate changes, for example, when the user modifies a field’s value in the record. When CloudKit registers the change, it sends push notifications to the user’s devices to inform your app about the change. You can then fetch the changes and cache them on-device. When appropriate, CloudKit excludes the device where the change originates.

You configure a subscription’s notifications by setting it’s [`notificationInfo`](/documentation/cloudkit/cksubscription/notificationinfo-swift.property) property. Do this before you save it to the server. A subscription generates either high-priority or medium-priority push notifications. CloudKit delivers medium-priority notifications to your app in the background. High-priority notifications are visual and the system displays them to the user. Visual notifications need the user’s permission. For more information, see [Asking permission to use notifications](/documentation/UserNotifications/asking-permission-to-use-notifications).

A subscription uses [`CKSubscription.NotificationInfo`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class) to configure its notifications. For background delivery, set only its [`shouldSendContentAvailable`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/shouldsendcontentavailable) property to [`true`](/documentation/swift/true). If you set any other property, CloudKit treats the notification as high-priority.

Don’t rely on push notifications for changes because the system can coalesce them. CloudKit can omit data to keep the notification’s payload size under the APNs size limit. If you use [`desiredKeys`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/desiredkeys) to include extra data in the payload, the server removes that first. A notification’s [`isPruned`](/documentation/cloudkit/cknotification/ispruned) property is [`true`](/documentation/swift/true) if CloudKit omits data.

Consider notifications an indication of remote changes. Use [`databaseScope`](/documentation/cloudkit/ckdatabasenotification/databasescope) to determine which database contains the changed record. To fetch the changes, configure an instance of [`CKQueryOperation`](/documentation/cloudkit/ckqueryoperation) to match the subscription and then execute it in the database. CloudKit returns all records that match the predicate, including the changed record. Dispose of any records you cache on-device and use the operation’s results instead.

You don’t instantiate this class. Instead, implement [`application(_:didReceiveRemoteNotification:fetchCompletionHandler:)`](/documentation/UIKit/UIApplicationDelegate/application\(_:didReceiveRemoteNotification:fetchCompletionHandler:\)) in your app delegate. Initialize [`CKNotification`](/documentation/cloudkit/cknotification) with the `userInfo` dictionary that CloudKit passes to the method. This returns an instance of the appropriate subclass. Use the [`notificationType`](/documentation/cloudkit/cknotification/notificationtype-swift.property) property to determine the type. Then cast to that type to access type-specific properties and methods.

## [Topics](/documentation/cloudkit/ckquerynotification#topics)

### [Getting the Database Scope](/documentation/cloudkit/ckquerynotification#Getting-the-Database-Scope)

[`var databaseScope: CKDatabase.Scope`](/documentation/cloudkit/ckquerynotification/databasescope)

The type of database for the record zone.

### [Getting the Notification Attributes](/documentation/cloudkit/ckquerynotification#Getting-the-Notification-Attributes)

[`var queryNotificationReason: CKQueryNotification.Reason`](/documentation/cloudkit/ckquerynotification/querynotificationreason)

The event that triggers the push notification.

[`enum Reason`](/documentation/cloudkit/ckquerynotification/reason)

Constants that indicate the event that triggers the notification.

### [Getting the Record Information](/documentation/cloudkit/ckquerynotification#Getting-the-Record-Information)

[`var recordID: CKRecord.ID?`](/documentation/cloudkit/ckquerynotification/recordid)

The ID of the record that CloudKit creates, updates, or deletes.

[`var recordFields: [String : Any]?`](/documentation/cloudkit/ckquerynotification/recordfields)

A dictionary of fields that have changes.

## [Relationships](/documentation/cloudkit/ckquerynotification#relationships)

### [Inherits From](/documentation/cloudkit/ckquerynotification#inherits-from)

-   [`CKNotification`](/documentation/cloudkit/cknotification)

### [Conforms To](/documentation/cloudkit/ckquerynotification#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/ckquerynotification#see-also)

### [Predicate-Driven Changes](/documentation/cloudkit/ckquerynotification#Predicate-Driven-Changes)

[`class CKQuerySubscription`](/documentation/cloudkit/ckquerysubscription)

A subscription that generates push notifications when CloudKit modifies records that match a predicate.

Current page is CKQueryNotification

---
### Getting the Database Scope

# databaseScope | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryNotification](/documentation/cloudkit/ckquerynotification)
-   databaseScope

Instance Property

# databaseScope

The type of database for the record zone.

```
var databaseScope: CKDatabase.Scope { get }
```

## [Discussion](/documentation/cloudkit/ckquerynotification/databasescope#Discussion)

This property’s value is one of the constants that [`CKDatabase.Scope`](/documentation/cloudkit/ckdatabase/scope) defines.

Current page is databaseScope

---
  
Getting the Notification Attributes

# queryNotificationReason | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryNotification](/documentation/cloudkit/ckquerynotification)
-   queryNotificationReason

Instance Property

# queryNotificationReason

The event that triggers the push notification.

```
var queryNotificationReason: CKQueryNotification.Reason { get }
```

## [Discussion](/documentation/cloudkit/ckquerynotification/querynotificationreason#Discussion)

Subscription notifications result from the creation, deletion, or updating of a single record. The record in question must match the subscription’s predicate for an event to trigger.

## [See Also](/documentation/cloudkit/ckquerynotification/querynotificationreason#see-also)

### [Getting the Notification Attributes](/documentation/cloudkit/ckquerynotification/querynotificationreason#Getting-the-Notification-Attributes)

[`enum Reason`](/documentation/cloudkit/ckquerynotification/reason)

Constants that indicate the event that triggers the notification.

Current page is queryNotificationReason

---
# CKQueryNotification.Reason

# CKQueryNotification.Reason | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryNotification](/documentation/cloudkit/ckquerynotification)
-   CKQueryNotification.Reason

Enumeration

# CKQueryNotification.Reason

Constants that indicate the event that triggers the notification.

```
enum Reason
```

## [Topics](/documentation/cloudkit/ckquerynotification/reason#topics)

### [Constants](/documentation/cloudkit/ckquerynotification/reason#Constants)

[`case recordCreated`](/documentation/cloudkit/ckquerynotification/reason/recordcreated)

A notification that indicates the creation of a record matching the subscription’s predicate.

[`case recordUpdated`](/documentation/cloudkit/ckquerynotification/reason/recordupdated)

A notification that indicates the update of a record matching the subscription’s predicate.

[`case recordDeleted`](/documentation/cloudkit/ckquerynotification/reason/recorddeleted)

A notification that indicates the deletion of a record matching the subscription’s predicate.

### [Initializers](/documentation/cloudkit/ckquerynotification/reason#Initializers)

[`init?(rawValue: Int)`](/documentation/cloudkit/ckquerynotification/reason/init\(rawvalue:\))

## [Relationships](/documentation/cloudkit/ckquerynotification/reason#relationships)

### [Conforms To](/documentation/cloudkit/ckquerynotification/reason#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/ckquerynotification/reason#see-also)

### [Getting the Notification Attributes](/documentation/cloudkit/ckquerynotification/reason#Getting-the-Notification-Attributes)

[`var queryNotificationReason: CKQueryNotification.Reason`](/documentation/cloudkit/ckquerynotification/querynotificationreason)

The event that triggers the push notification.

Current page is CKQueryNotification.Reason

  
Constants: 

# CKQueryNotification.Reason.recordCreated | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryNotification](/documentation/cloudkit/ckquerynotification)
-   [CKQueryNotification.Reason](/documentation/cloudkit/ckquerynotification/reason)
-   CKQueryNotification.Reason.recordCreated

Case

# CKQueryNotification.Reason.recordCreated

A notification that indicates the creation of a record matching the subscription’s predicate.

```
case recordCreated
```

## [See Also](/documentation/cloudkit/ckquerynotification/reason/recordcreated#see-also)

### [Constants](/documentation/cloudkit/ckquerynotification/reason/recordcreated#Constants)

[`case recordUpdated`](/documentation/cloudkit/ckquerynotification/reason/recordupdated)

A notification that indicates the update of a record matching the subscription’s predicate.

[`case recordDeleted`](/documentation/cloudkit/ckquerynotification/reason/recorddeleted)

A notification that indicates the deletion of a record matching the subscription’s predicate.

Current page is CKQueryNotification.Reason.recordCreated

# CKQueryNotification.Reason.recordUpdated | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryNotification](/documentation/cloudkit/ckquerynotification)
-   [CKQueryNotification.Reason](/documentation/cloudkit/ckquerynotification/reason)
-   CKQueryNotification.Reason.recordUpdated

Case

# CKQueryNotification.Reason.recordUpdated

A notification that indicates the update of a record matching the subscription’s predicate.

```
case recordUpdated
```

## [See Also](/documentation/cloudkit/ckquerynotification/reason/recordupdated#see-also)

### [Constants](/documentation/cloudkit/ckquerynotification/reason/recordupdated#Constants)

[`case recordCreated`](/documentation/cloudkit/ckquerynotification/reason/recordcreated)

A notification that indicates the creation of a record matching the subscription’s predicate.

[`case recordDeleted`](/documentation/cloudkit/ckquerynotification/reason/recorddeleted)

A notification that indicates the deletion of a record matching the subscription’s predicate.

Current page is CKQueryNotification.Reason.recordUpdated

# CKQueryNotification.Reason.recordDeleted | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryNotification](/documentation/cloudkit/ckquerynotification)
-   [CKQueryNotification.Reason](/documentation/cloudkit/ckquerynotification/reason)
-   CKQueryNotification.Reason.recordDeleted

Case

# CKQueryNotification.Reason.recordDeleted

A notification that indicates the deletion of a record matching the subscription’s predicate.

```
case recordDeleted
```

## [See Also](/documentation/cloudkit/ckquerynotification/reason/recorddeleted#see-also)

### [Constants](/documentation/cloudkit/ckquerynotification/reason/recorddeleted#Constants)

[`case recordCreated`](/documentation/cloudkit/ckquerynotification/reason/recordcreated)

A notification that indicates the creation of a record matching the subscription’s predicate.

[`case recordUpdated`](/documentation/cloudkit/ckquerynotification/reason/recordupdated)

A notification that indicates the update of a record matching the subscription’s predicate.

Current page is CKQueryNotification.Reason.recordDeleted

# init(rawValue:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryNotification](/documentation/cloudkit/ckquerynotification)
-   [CKQueryNotification.Reason](/documentation/cloudkit/ckquerynotification/reason)
-   init(rawValue:)

Initializer

# init(rawValue:)

```
init?(rawValue: Int)
```

Current page is init(rawValue:)

---
### Getting the Record Information

# recordID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryNotification](/documentation/cloudkit/ckquerynotification)
-   recordID

Instance Property

# recordID

The ID of the record that CloudKit creates, updates, or deletes.

```
@NSCopying
var recordID: CKRecord.ID? { get }
```

## [Discussion](/documentation/cloudkit/ckquerynotification/recordid#Discussion)

Use this value to fetch the record.

## [See Also](/documentation/cloudkit/ckquerynotification/recordid#see-also)

### [Getting the Record Information](/documentation/cloudkit/ckquerynotification/recordid#Getting-the-Record-Information)

[`var recordFields: [String : Any]?`](/documentation/cloudkit/ckquerynotification/recordfields)

A dictionary of fields that have changes.

Current page is recordID

# recordFields | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryNotification](/documentation/cloudkit/ckquerynotification)
-   recordFields

Instance Property

# recordFields

A dictionary of fields that have changes.

```
var recordFields: [String : Any]? { get }
```

## [Discussion](/documentation/cloudkit/ckquerynotification/recordfields#Discussion)

For updated and created records, this property contains the subscription’s desired keys. When you configure the notification info of a subscription, you specify the names of one or more fields in the [`desiredKeys`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/desiredkeys) property. When a push notification triggers, CloudKit retrieves the values for each of those keys from the record and includes them in the notification’s payload.

For query notifications that you fetch from a container, all keys and values are present. For query notifications that you create from push notifications, one or more keys and values may be missing. Push notification payloads have a size limit, and CloudKit can exclude record fields when a payload exceeds that limit. For information about the order, see the overview of this class.

## [See Also](/documentation/cloudkit/ckquerynotification/recordfields#see-also)

### [Getting the Record Information](/documentation/cloudkit/ckquerynotification/recordfields#Getting-the-Record-Information)

[`var recordID: CKRecord.ID?`](/documentation/cloudkit/ckquerynotification/recordid)

The ID of the record that CloudKit creates, updates, or deletes.

Current page is recordFields

