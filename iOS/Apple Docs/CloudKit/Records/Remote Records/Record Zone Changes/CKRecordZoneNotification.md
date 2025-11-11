
# CKRecordZoneNotification | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKRecordZoneNotification

Class

# CKRecordZoneNotification

A notification that triggers when the contents of a record zone change.

```
class CKRecordZoneNotification
```

## [Overview](/documentation/cloudkit/ckrecordzonenotification#overview)

A record zone subscription executes when a user, or in certain scenarios, CloudKit, modifies a record in that zone, for example, when a field’s value changes in a record. When CloudKit registers the change, it sends push notifications to the user’s devices to inform your app about the change. You can then fetch the changes and cache them on-device. When appropriate, CloudKit excludes the device where the change originates.

You configure a subscription’s notifications by setting it’s [`notificationInfo`](/documentation/cloudkit/cksubscription/notificationinfo-swift.property) property. Do this before you save it to the server. A subscription generates either high-priority or medium-priority push notifications. CloudKit delivers medium-priority notifications to your app in the background. High-priority notifications are visual and the system displays them to the user. Visual notifications need the user’s permission. For more information, see [Asking permission to use notifications](/documentation/UserNotifications/asking-permission-to-use-notifications).

A subscription uses [`CKSubscription.NotificationInfo`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class) to configure its notifications. For background delivery, set only its [`shouldSendContentAvailable`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/shouldsendcontentavailable) property to [`true`](/documentation/swift/true). If you set any other property, CloudKit treats the notification as high-priority.

Don’t rely on push notifications for specific changes to records because the system can coalesce them. CloudKit can omit data to keep the notification’s payload size under the APNs size limit. Consider notifications an indication of remote changes. Use [`databaseScope`](/documentation/cloudkit/ckrecordzonenotification/databasescope) to determine which database contains the changed record zone, and [`recordZoneID`](/documentation/cloudkit/ckrecordzonenotification/recordzoneid) to determine which zone contains changed records. You can then fetch just those changes using [`CKFetchRecordZoneChangesOperation`](/documentation/cloudkit/ckfetchrecordzonechangesoperation). A notification’s [`isPruned`](/documentation/cloudkit/cknotification/ispruned) property is [`true`](/documentation/swift/true) if CloudKit omits data.

You don’t instantiate this class. Instead, implement [`application(_:didReceiveRemoteNotification:fetchCompletionHandler:)`](/documentation/UIKit/UIApplicationDelegate/application\(_:didReceiveRemoteNotification:fetchCompletionHandler:\)) in your app delegate. Initialize [`CKNotification`](/documentation/cloudkit/cknotification) with the `userInfo` dictionary that CloudKit passes to the method. This returns an instance of the appropriate subclass. Use the [`notificationType`](/documentation/cloudkit/cknotification/notificationtype-swift.property) property to determine the type. Then cast to that type to access type-specific properties and methods.

## [Topics](/documentation/cloudkit/ckrecordzonenotification#topics)

### [Getting the Record Zone ID](/documentation/cloudkit/ckrecordzonenotification#Getting-the-Record-Zone-ID)

[`var recordZoneID: CKRecordZone.ID?`](/documentation/cloudkit/ckrecordzonenotification/recordzoneid)

The ID of the record zone that has changes.

### [Getting the Database Scope](/documentation/cloudkit/ckrecordzonenotification#Getting-the-Database-Scope)

[`var databaseScope: CKDatabase.Scope`](/documentation/cloudkit/ckrecordzonenotification/databasescope)

The type of database for the record zone.

## [Relationships](/documentation/cloudkit/ckrecordzonenotification#relationships)

### [Inherits From](/documentation/cloudkit/ckrecordzonenotification#inherits-from)

-   [`CKNotification`](/documentation/cloudkit/cknotification)

### [Conforms To](/documentation/cloudkit/ckrecordzonenotification#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/ckrecordzonenotification#see-also)

### [Record Zone Changes](/documentation/cloudkit/ckrecordzonenotification#Record-Zone-Changes)

[`class CKRecordZoneSubscription`](/documentation/cloudkit/ckrecordzonesubscription)

A subscription that generates push notifications when CloudKit modifies records in a specific record zone.

[`class CKFetchRecordZoneChangesOperation`](/documentation/cloudkit/ckfetchrecordzonechangesoperation)

An operation that fetches record zone changes.

Current page is CKRecordZoneNotification

---
  
Getting the Record Zone ID

# recordZoneID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZoneNotification](/documentation/cloudkit/ckrecordzonenotification)
-   recordZoneID

Instance Property

# recordZoneID

The ID of the record zone that has changes.

```
@NSCopying
var recordZoneID: CKRecordZone.ID? { get }
```

Current page is recordZoneID

---
  
Getting the Database Scope

# databaseScope | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZoneNotification](/documentation/cloudkit/ckrecordzonenotification)
-   databaseScope

Instance Property

# databaseScope

The type of database for the record zone.

```
var databaseScope: CKDatabase.Scope { get }
```

## [Discussion](/documentation/cloudkit/ckrecordzonenotification/databasescope#Discussion)

This property’s value is one of the constants that [`CKDatabase.Scope`](/documentation/cloudkit/ckdatabase/scope) defines.

Current page is databaseScope