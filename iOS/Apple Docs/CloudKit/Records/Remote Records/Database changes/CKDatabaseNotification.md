# CKDatabaseNotification | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKDatabaseNotification

Class

# CKDatabaseNotification

A notification that triggers when the contents of a database change.

```
class CKDatabaseNotification
```

## [Overview](/documentation/cloudkit/ckdatabasenotification#overview)

Database subscriptions execute when changes happen in any of a database’s record zones, for example, when CloudKit saves a new record. When the subscription registers a change, it sends push notifications to the user’s devices to inform your app about the change. You can then fetch the changes and cache them on-device. When appropriate, CloudKit excludes the device where the change originates.

You configure a subscription’s notifications by setting it’s [`notificationInfo`](/documentation/cloudkit/cksubscription/notificationinfo-swift.property) property. Do this before you save it to the server. A subscription generates either high-priority or medium-priority push notifications. CloudKit delivers medium-priority notifications to your app in the background. High-priority notifications are visual and the system displays them to the user. Visual notifications need the user’s permission. For more information, see [Asking permission to use notifications](/documentation/UserNotifications/asking-permission-to-use-notifications).

A subscription uses [`CKSubscription.NotificationInfo`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class) to configure its notifications. For background delivery, set only its [`shouldSendContentAvailable`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/shouldsendcontentavailable) property to [`true`](/documentation/swift/true). If you set any other property, CloudKit treats the notification as high-priority.

Don’t rely on push notifications for specific changes because the system can coalesce them. CloudKit can omit data to keep the notification’s payload size under the APNs size limit. Consider notifications an indication of remote changes. Use [`databaseScope`](/documentation/cloudkit/ckdatabasenotification/databasescope) to determine which database has changes, and then [`CKFetchDatabaseChangesOperation`](/documentation/cloudkit/ckfetchdatabasechangesoperation) to fetch those changes. A notification’s [`isPruned`](/documentation/cloudkit/cknotification/ispruned) property is [`true`](/documentation/swift/true) if CloudKit omits data.

You don’t instantiate this class. Instead, implement [`application(_:didReceiveRemoteNotification:fetchCompletionHandler:)`](/documentation/UIKit/UIApplicationDelegate/application\(_:didReceiveRemoteNotification:fetchCompletionHandler:\)) in your app delegate. Initialize [`CKNotification`](/documentation/cloudkit/cknotification) with the `userInfo` dictionary that CloudKit passes to the method. This returns an instance of the appropriate subclass. Use the [`notificationType`](/documentation/cloudkit/cknotification/notificationtype-swift.property) property to determine the type. Then cast to that type to access type-specific properties and methods.

## [Topics](/documentation/cloudkit/ckdatabasenotification#topics)

### [Getting the Database Scope](/documentation/cloudkit/ckdatabasenotification#Getting-the-Database-Scope)

[`var databaseScope: CKDatabase.Scope`](/documentation/cloudkit/ckdatabasenotification/databasescope)

The type of database.

## [Relationships](/documentation/cloudkit/ckdatabasenotification#relationships)

### [Inherits From](/documentation/cloudkit/ckdatabasenotification#inherits-from)

-   [`CKNotification`](/documentation/cloudkit/cknotification)

### [Conforms To](/documentation/cloudkit/ckdatabasenotification#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/ckdatabasenotification#see-also)

### [Database Changes](/documentation/cloudkit/ckdatabasenotification#Database-Changes)

[`class CKDatabaseSubscription`](/documentation/cloudkit/ckdatabasesubscription)

A subscription that generates push notifications when CloudKit modifies records in a database.

[`class CKFetchDatabaseChangesOperation`](/documentation/cloudkit/ckfetchdatabasechangesoperation)

An operation that fetches database changes.

Current page is CKDatabaseNotification

---
  
Getting the Database Scope

# databaseScope | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabaseNotification](/documentation/cloudkit/ckdatabasenotification)
-   databaseScope

Instance Property

# databaseScope

The type of database.

```
var databaseScope: CKDatabase.Scope { get }
```

## [Discussion](/documentation/cloudkit/ckdatabasenotification/databasescope#Discussion)

This property’s value is one of the constants that [`CKDatabase.Scope`](/documentation/cloudkit/ckdatabase/scope) defines.

Current page is databaseScope