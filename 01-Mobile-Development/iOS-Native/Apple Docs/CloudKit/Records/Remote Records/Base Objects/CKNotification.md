# CKNotification | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKNotification

Class

# CKNotification

The abstract base class for CloudKit notifications.

```
class CKNotification
```

## [Overview](/documentation/cloudkit/cknotification#overview)

Use subclasses of `CKNotification` to extract data from push notifications that the system receives, or to fetch a container’s previous push notifications. In both cases, the object indicates the changed data.

`CKNotification` is an abstract class. When you create a notification from a payload dictionary, the [`init(fromRemoteNotificationDictionary:)`](/documentation/cloudkit/cknotification/init\(fromremotenotificationdictionary:\)) method returns an instance of the appropriate subclass. Similarly, when you fetch notifications from a container, you receive instances of a concrete subclass. `CKNotification` provides information about the push notification and its method of delivery. Subclasses contain specific data that provides the changes.

## [Topics](/documentation/cloudkit/cknotification#topics)

### [Creating a Notification](/documentation/cloudkit/cknotification#Creating-a-Notification)

[`convenience init?(fromRemoteNotificationDictionary: [AnyHashable : Any])`](/documentation/cloudkit/cknotification/init\(fromremotenotificationdictionary:\))

Creates a new notification using the specified payload data.

### [Identifying the Notification](/documentation/cloudkit/cknotification#Identifying-the-Notification)

[`var notificationID: CKNotification.ID?`](/documentation/cloudkit/cknotification/notificationid)

The notification’s ID.

[`class ID`](/documentation/cloudkit/cknotification/id)

An object that uniquely identifies a push notification that a container sends.

[`var notificationType: CKNotification.NotificationType`](/documentation/cloudkit/cknotification/notificationtype-swift.property)

The type of event that generates the notification.

[`enum NotificationType`](/documentation/cloudkit/cknotification/notificationtype-swift.enum)

Constants that indicate the type of event that generates the push notification.

[`var containerIdentifier: String?`](/documentation/cloudkit/cknotification/containeridentifier)

The ID of the container with the content that triggers the notification.

### [Getting the Notification’s Status](/documentation/cloudkit/cknotification#Getting-the-Notifications-Status)

[`var isPruned: Bool`](/documentation/cloudkit/cknotification/ispruned)

A Boolean value that indicates whether the system removes some push notification content before delivery.

### [Accessing the Notification Info](/documentation/cloudkit/cknotification#Accessing-the-Notification-Info)

[`var alertBody: String?`](/documentation/cloudkit/cknotification/alertbody)

The notification’s alert body.

Deprecated

[`var alertLocalizationKey: String?`](/documentation/cloudkit/cknotification/alertlocalizationkey)

The key that identifies the localized text for the alert body.

Deprecated

[`var alertLocalizationArgs: [String]?`](/documentation/cloudkit/cknotification/alertlocalizationargs)

The fields for building a notification’s alert.

Deprecated

[`var alertActionLocalizationKey: String?`](/documentation/cloudkit/cknotification/alertactionlocalizationkey)

The key that identifies the localized string for the notification’s action.

Deprecated

[`var alertLaunchImage: String?`](/documentation/cloudkit/cknotification/alertlaunchimage)

The filename of an image to use as a launch image.

Deprecated

[`var soundName: String?`](/documentation/cloudkit/cknotification/soundname)

The name of the sound file to play when a notification arrives.

Deprecated

[`var badge: NSNumber?`](/documentation/cloudkit/cknotification/badge)

The value that the app icon’s badge displays.

Deprecated

[`var category: String?`](/documentation/cloudkit/cknotification/category)

The name of the action group that corresponds to this notification.

Deprecated

[`var subscriptionID: CKSubscription.ID?`](/documentation/cloudkit/cknotification/subscriptionid-16ygj)

The ID of the subscription that triggers the notification.

[`var subscriptionOwnerUserRecordID: CKRecord.ID?`](/documentation/cloudkit/cknotification/subscriptionowneruserrecordid)

The ID of the user record that creates the subscription that generates the push notification.

[`var title: String?`](/documentation/cloudkit/cknotification/title)

The notification’s title.

Deprecated

[`var titleLocalizationKey: String?`](/documentation/cloudkit/cknotification/titlelocalizationkey)

The key that identifies the localized string for the notification’s title.

Deprecated

[`var titleLocalizationArgs: [String]?`](/documentation/cloudkit/cknotification/titlelocalizationargs)

The fields for building a notification’s title.

Deprecated

[`var subtitle: String?`](/documentation/cloudkit/cknotification/subtitle)

The notification’s subtitle.

Deprecated

[`var subtitleLocalizationKey: String?`](/documentation/cloudkit/cknotification/subtitlelocalizationkey)

The key that identifies the localized string for the notification’s subtitle.

Deprecated

[`var subtitleLocalizationArgs: [String]?`](/documentation/cloudkit/cknotification/subtitlelocalizationargs)

The fields for building a notification’s subtitle.

Deprecated

## [Relationships](/documentation/cloudkit/cknotification#relationships)

### [Inherits From](/documentation/cloudkit/cknotification#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Inherited By](/documentation/cloudkit/cknotification#inherited-by)

-   [`CKDatabaseNotification`](/documentation/cloudkit/ckdatabasenotification)
-   [`CKQueryNotification`](/documentation/cloudkit/ckquerynotification)
-   [`CKRecordZoneNotification`](/documentation/cloudkit/ckrecordzonenotification)

### [Conforms To](/documentation/cloudkit/cknotification#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/cknotification#see-also)

### [Base Objects](/documentation/cloudkit/cknotification#Base-Objects)

[`class CKSubscription`](/documentation/cloudkit/cksubscription)

An abstract base class for subscriptions.

[`class CKDatabaseOperation`](/documentation/cloudkit/ckdatabaseoperation)

The abstract base class for operations that act upon databases in CloudKit.

Current page is CKNotification

---
### Creating a Notification

# init(fromRemoteNotificationDictionary:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKNotification](/documentation/cloudkit/cknotification)
-   init(fromRemoteNotificationDictionary:)

Initializer

# init(fromRemoteNotificationDictionary:)

Creates a new notification using the specified payload data.

```
convenience init?(fromRemoteNotificationDictionary notificationDictionary: [AnyHashable : Any])
```

## [Parameters](/documentation/cloudkit/cknotification/init\(fromremotenotificationdictionary:\)#parameters)

`notificationDictionary`

The push notification’s payload data. Use the dictionary that the system provides to your app delegate’s [`application(_:didReceiveRemoteNotification:fetchCompletionHandler:)`](/documentation/UIKit/UIApplicationDelegate/application\(_:didReceiveRemoteNotification:fetchCompletionHandler:\)) method. This parameter must not be `nil`.

Current page is init(fromRemoteNotificationDictionary:)

---
  
Identifying the Notification

# notificationID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKNotification](/documentation/cloudkit/cknotification)
-   notificationID

Instance Property

# notificationID

The notification’s ID.

```
@NSCopying
var notificationID: CKNotification.ID? { get }
```

## [Discussion](/documentation/cloudkit/cknotification/notificationid#Discussion)

Use this property to differentiate notifications.

## [See Also](/documentation/cloudkit/cknotification/notificationid#see-also)

### [Identifying the Notification](/documentation/cloudkit/cknotification/notificationid#Identifying-the-Notification)

[`class ID`](/documentation/cloudkit/cknotification/id)

An object that uniquely identifies a push notification that a container sends.

[`var notificationType: CKNotification.NotificationType`](/documentation/cloudkit/cknotification/notificationtype-swift.property)

The type of event that generates the notification.

[`enum NotificationType`](/documentation/cloudkit/cknotification/notificationtype-swift.enum)

Constants that indicate the type of event that generates the push notification.

[`var containerIdentifier: String?`](/documentation/cloudkit/cknotification/containeridentifier)

The ID of the container with the content that triggers the notification.

Current page is notificationID

# CKNotification.ID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKNotification](/documentation/cloudkit/cknotification)
-   CKNotification.ID

Class

# CKNotification.ID

An object that uniquely identifies a push notification that a container sends.

```
class ID
```

## [Overview](/documentation/cloudkit/cknotification/id#overview)

You don’t create notification IDs directly. The server creates them when it creates instances of [`CKNotification`](/documentation/cloudkit/cknotification) that correspond to the push notifications that CloudKit sends to your app. You can compare two IDs using the [`isEqual(_:)`](/documentation/ObjectiveC/NSObjectProtocol/isEqual\(_:\)) method to determine whether two notifications are the same. This class defines no methods or properties.

## [Relationships](/documentation/cloudkit/cknotification/id#relationships)

### [Inherits From](/documentation/cloudkit/cknotification/id#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/cknotification/id#conforms-to)

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

## [See Also](/documentation/cloudkit/cknotification/id#see-also)

### [Identifying the Notification](/documentation/cloudkit/cknotification/id#Identifying-the-Notification)

[`var notificationID: CKNotification.ID?`](/documentation/cloudkit/cknotification/notificationid)

The notification’s ID.

[`var notificationType: CKNotification.NotificationType`](/documentation/cloudkit/cknotification/notificationtype-swift.property)

The type of event that generates the notification.

[`enum NotificationType`](/documentation/cloudkit/cknotification/notificationtype-swift.enum)

Constants that indicate the type of event that generates the push notification.

[`var containerIdentifier: String?`](/documentation/cloudkit/cknotification/containeridentifier)

The ID of the container with the content that triggers the notification.

Current page is CKNotification.ID

# notificationType | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKNotification](/documentation/cloudkit/cknotification)
-   notificationType

Instance Property

# notificationType

The type of event that generates the notification.

```
var notificationType: CKNotification.NotificationType { get }
```

## [Discussion](/documentation/cloudkit/cknotification/notificationtype-swift.property#Discussion)

Different notification types correspond to different subclasses of `CKNotification`, so you can use the value in this property to determine how to handle the notification data.

## [See Also](/documentation/cloudkit/cknotification/notificationtype-swift.property#see-also)

### [Identifying the Notification](/documentation/cloudkit/cknotification/notificationtype-swift.property#Identifying-the-Notification)

[`var notificationID: CKNotification.ID?`](/documentation/cloudkit/cknotification/notificationid)

The notification’s ID.

[`class ID`](/documentation/cloudkit/cknotification/id)

An object that uniquely identifies a push notification that a container sends.

[`enum NotificationType`](/documentation/cloudkit/cknotification/notificationtype-swift.enum)

Constants that indicate the type of event that generates the push notification.

[`var containerIdentifier: String?`](/documentation/cloudkit/cknotification/containeridentifier)

The ID of the container with the content that triggers the notification.

Current page is notificationType

---
# CKNotification.NotificationType

  
Notification Types

# CKNotification.NotificationType.query | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKNotification](/documentation/cloudkit/cknotification)
-   [CKNotification.NotificationType](/documentation/cloudkit/cknotification/notificationtype-swift.enum)
-   CKNotification.NotificationType.query

Case

# CKNotification.NotificationType.query

A notification that CloudKit generates from a query subscription’s predicate.

```
case query
```

## [See Also](/documentation/cloudkit/cknotification/notificationtype-swift.enum/query#see-also)

### [Notification Types](/documentation/cloudkit/cknotification/notificationtype-swift.enum/query#Notification-Types)

[`case database`](/documentation/cloudkit/cknotification/notificationtype-swift.enum/database)

A notification that CloudKit generates when the contents of a database change.

[`case recordZone`](/documentation/cloudkit/cknotification/notificationtype-swift.enum/recordzone)

A notification that CloudKit generates when the contents of a record zone change.

[`case readNotification`](/documentation/cloudkit/cknotification/notificationtype-swift.enum/readnotification)

A notification that your app marks as read.

Current page is CKNotification.NotificationType.query

# CKNotification.NotificationType.database | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKNotification](/documentation/cloudkit/cknotification)
-   [CKNotification.NotificationType](/documentation/cloudkit/cknotification/notificationtype-swift.enum)
-   CKNotification.NotificationType.database

Case

# CKNotification.NotificationType.database

A notification that CloudKit generates when the contents of a database change.

```
case database
```

## [See Also](/documentation/cloudkit/cknotification/notificationtype-swift.enum/database#see-also)

### [Notification Types](/documentation/cloudkit/cknotification/notificationtype-swift.enum/database#Notification-Types)

[`case query`](/documentation/cloudkit/cknotification/notificationtype-swift.enum/query)

A notification that CloudKit generates from a query subscription’s predicate.

[`case recordZone`](/documentation/cloudkit/cknotification/notificationtype-swift.enum/recordzone)

A notification that CloudKit generates when the contents of a record zone change.

[`case readNotification`](/documentation/cloudkit/cknotification/notificationtype-swift.enum/readnotification)

A notification that your app marks as read.

Current page is CKNotification.NotificationType.database

# CKNotification.NotificationType.recordZone | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKNotification](/documentation/cloudkit/cknotification)
-   [CKNotification.NotificationType](/documentation/cloudkit/cknotification/notificationtype-swift.enum)
-   CKNotification.NotificationType.recordZone

Case

# CKNotification.NotificationType.recordZone

A notification that CloudKit generates when the contents of a record zone change.

```
case recordZone
```

## [See Also](/documentation/cloudkit/cknotification/notificationtype-swift.enum/recordzone#see-also)

### [Notification Types](/documentation/cloudkit/cknotification/notificationtype-swift.enum/recordzone#Notification-Types)

[`case query`](/documentation/cloudkit/cknotification/notificationtype-swift.enum/query)

A notification that CloudKit generates from a query subscription’s predicate.

[`case database`](/documentation/cloudkit/cknotification/notificationtype-swift.enum/database)

A notification that CloudKit generates when the contents of a database change.

[`case readNotification`](/documentation/cloudkit/cknotification/notificationtype-swift.enum/readnotification)

A notification that your app marks as read.

Current page is CKNotification.NotificationType.recordZone

# CKNotification.NotificationType.readNotification | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKNotification](/documentation/cloudkit/cknotification)
-   [CKNotification.NotificationType](/documentation/cloudkit/cknotification/notificationtype-swift.enum)
-   CKNotification.NotificationType.readNotification

Case

# CKNotification.NotificationType.readNotification

A notification that your app marks as read.

```
case readNotification
```

## [See Also](/documentation/cloudkit/cknotification/notificationtype-swift.enum/readnotification#see-also)

### [Notification Types](/documentation/cloudkit/cknotification/notificationtype-swift.enum/readnotification#Notification-Types)

[`case query`](/documentation/cloudkit/cknotification/notificationtype-swift.enum/query)

A notification that CloudKit generates from a query subscription’s predicate.

[`case database`](/documentation/cloudkit/cknotification/notificationtype-swift.enum/database)

A notification that CloudKit generates when the contents of a database change.

[`case recordZone`](/documentation/cloudkit/cknotification/notificationtype-swift.enum/recordzone)

A notification that CloudKit generates when the contents of a record zone change.

Current page is CKNotification.NotificationType.readNotification

# init(rawValue:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKNotification](/documentation/cloudkit/cknotification)
-   [CKNotification.NotificationType](/documentation/cloudkit/cknotification/notificationtype-swift.enum)
-   init(rawValue:)

Initializer

# init(rawValue:)

```
init?(rawValue: Int)
```

Current page is init(rawValue:)

---
# containerIdentifier | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKNotification](/documentation/cloudkit/cknotification)
-   containerIdentifier

Instance Property

# containerIdentifier

The ID of the container with the content that triggers the notification.

```
var containerIdentifier: String? { get }
```

## [Discussion](/documentation/cloudkit/cknotification/containeridentifier#Discussion)

Use this property to determine the location of the changed content.

## [See Also](/documentation/cloudkit/cknotification/containeridentifier#see-also)

### [Identifying the Notification](/documentation/cloudkit/cknotification/containeridentifier#Identifying-the-Notification)

[`var notificationID: CKNotification.ID?`](/documentation/cloudkit/cknotification/notificationid)

The notification’s ID.

[`class ID`](/documentation/cloudkit/cknotification/id)

An object that uniquely identifies a push notification that a container sends.

[`var notificationType: CKNotification.NotificationType`](/documentation/cloudkit/cknotification/notificationtype-swift.property)

The type of event that generates the notification.

[`enum NotificationType`](/documentation/cloudkit/cknotification/notificationtype-swift.enum)

Constants that indicate the type of event that generates the push notification.

Current page is containerIdentifier

---
  
Getting the Notification’s Status

# isPruned | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKNotification](/documentation/cloudkit/cknotification)
-   isPruned

Instance Property

# isPruned

A Boolean value that indicates whether the system removes some push notification content before delivery.

```
var isPruned: Bool { get }
```

## [Discussion](/documentation/cloudkit/cknotification/ispruned#Discussion)

The server may truncate the payload data of a push notification if the size of that data exceeds the allowed maximum. For notifications you create using a payload dictionary, the value of this property is [`true`](/documentation/swift/true) if the payload data doesn’t contain all information regarding the change. The value is [`false`](/documentation/swift/false) if the payload data is complete.

For notifications you fetch from the database using a `CKFetchNotificationChangesOperation` operation, this property’s value is always [`true`](/documentation/swift/true).

When CloudKit must remove payload data, it removes it in a specific order. This class’s properties are among the last that CloudKit removes because they define information about how to deliver the push notification. The following list shows the properties that CloudKit removes, and the order for removing them:

1.  [`containerIdentifier`](/documentation/cloudkit/cknotification/containeridentifier)
    
2.  Keys that subclasses of `CKNotification` define.
    
3.  [`soundName`](/documentation/cloudkit/cknotification/soundname)
    
4.  [`alertLaunchImage`](/documentation/cloudkit/cknotification/alertlaunchimage)
    
5.  [`alertActionLocalizationKey`](/documentation/cloudkit/cknotification/alertactionlocalizationkey)
    
6.  [`alertBody`](/documentation/cloudkit/cknotification/alertbody)
    
7.  [`alertLocalizationArgs`](/documentation/cloudkit/cknotification/alertlocalizationargs)
    
8.  [`alertLocalizationKey`](/documentation/cloudkit/cknotification/alertlocalizationkey)
    
9.  [`badge`](/documentation/cloudkit/cknotification/badge)
    
10.  [`notificationID`](/documentation/cloudkit/cknotification/notificationid)
    

Current page is isPruned

---
  
Accessing the Notification Info

# subscriptionID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKNotification](/documentation/cloudkit/cknotification)
-   subscriptionID

Instance Property

# subscriptionID

The ID of the subscription that triggers the notification.

```
var subscriptionID: CKSubscription.ID? { get }
```

## [See Also](/documentation/cloudkit/cknotification/subscriptionid-16ygj#see-also)

### [Accessing the Notification Info](/documentation/cloudkit/cknotification/subscriptionid-16ygj#Accessing-the-Notification-Info)

[`var alertBody: String?`](/documentation/cloudkit/cknotification/alertbody)

The notification’s alert body.

Deprecated

[`var alertLocalizationKey: String?`](/documentation/cloudkit/cknotification/alertlocalizationkey)

The key that identifies the localized text for the alert body.

Deprecated

[`var alertLocalizationArgs: [String]?`](/documentation/cloudkit/cknotification/alertlocalizationargs)

The fields for building a notification’s alert.

Deprecated

[`var alertActionLocalizationKey: String?`](/documentation/cloudkit/cknotification/alertactionlocalizationkey)

The key that identifies the localized string for the notification’s action.

Deprecated

[`var alertLaunchImage: String?`](/documentation/cloudkit/cknotification/alertlaunchimage)

The filename of an image to use as a launch image.

Deprecated

[`var soundName: String?`](/documentation/cloudkit/cknotification/soundname)

The name of the sound file to play when a notification arrives.

Deprecated

[`var badge: NSNumber?`](/documentation/cloudkit/cknotification/badge)

The value that the app icon’s badge displays.

Deprecated

[`var category: String?`](/documentation/cloudkit/cknotification/category)

The name of the action group that corresponds to this notification.

Deprecated

[`var subscriptionOwnerUserRecordID: CKRecord.ID?`](/documentation/cloudkit/cknotification/subscriptionowneruserrecordid)

The ID of the user record that creates the subscription that generates the push notification.

[`var title: String?`](/documentation/cloudkit/cknotification/title)

The notification’s title.

Deprecated

[`var titleLocalizationKey: String?`](/documentation/cloudkit/cknotification/titlelocalizationkey)

The key that identifies the localized string for the notification’s title.

Deprecated

[`var titleLocalizationArgs: [String]?`](/documentation/cloudkit/cknotification/titlelocalizationargs)

The fields for building a notification’s title.

Deprecated

[`var subtitle: String?`](/documentation/cloudkit/cknotification/subtitle)

The notification’s subtitle.

Deprecated

[`var subtitleLocalizationKey: String?`](/documentation/cloudkit/cknotification/subtitlelocalizationkey)

The key that identifies the localized string for the notification’s subtitle.

Deprecated

[`var subtitleLocalizationArgs: [String]?`](/documentation/cloudkit/cknotification/subtitlelocalizationargs)

The fields for building a notification’s subtitle.

Deprecated

Current page is subscriptionID

# subscriptionOwnerUserRecordID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKNotification](/documentation/cloudkit/cknotification)
-   subscriptionOwnerUserRecordID

Instance Property

# subscriptionOwnerUserRecordID

The ID of the user record that creates the subscription that generates the push notification.

```
@NSCopying
var subscriptionOwnerUserRecordID: CKRecord.ID? { get }
```

## [Discussion](/documentation/cloudkit/cknotification/subscriptionowneruserrecordid#Discussion)

On a system that supports multiple users, such as tvOS, use this identifier to check whether the pending content is for the current user. If your app always fetches data from CloudKit on launch, you may improve efficiency by disregarding notifications for other users.

For more information about supporting a multiuser environment, see [Personalizing Your App for Each User on Apple TV](/documentation/TVServices/personalizing-your-app-for-each-user-on-apple-tv).

## [See Also](/documentation/cloudkit/cknotification/subscriptionowneruserrecordid#see-also)

### [Accessing the Notification Info](/documentation/cloudkit/cknotification/subscriptionowneruserrecordid#Accessing-the-Notification-Info)

[`var alertBody: String?`](/documentation/cloudkit/cknotification/alertbody)

The notification’s alert body.

Deprecated

[`var alertLocalizationKey: String?`](/documentation/cloudkit/cknotification/alertlocalizationkey)

The key that identifies the localized text for the alert body.

Deprecated

[`var alertLocalizationArgs: [String]?`](/documentation/cloudkit/cknotification/alertlocalizationargs)

The fields for building a notification’s alert.

Deprecated

[`var alertActionLocalizationKey: String?`](/documentation/cloudkit/cknotification/alertactionlocalizationkey)

The key that identifies the localized string for the notification’s action.

Deprecated

[`var alertLaunchImage: String?`](/documentation/cloudkit/cknotification/alertlaunchimage)

The filename of an image to use as a launch image.

Deprecated

[`var soundName: String?`](/documentation/cloudkit/cknotification/soundname)

The name of the sound file to play when a notification arrives.

Deprecated

[`var badge: NSNumber?`](/documentation/cloudkit/cknotification/badge)

The value that the app icon’s badge displays.

Deprecated

[`var category: String?`](/documentation/cloudkit/cknotification/category)

The name of the action group that corresponds to this notification.

Deprecated

[`var subscriptionID: CKSubscription.ID?`](/documentation/cloudkit/cknotification/subscriptionid-16ygj)

The ID of the subscription that triggers the notification.

[`var title: String?`](/documentation/cloudkit/cknotification/title)

The notification’s title.

Deprecated

[`var titleLocalizationKey: String?`](/documentation/cloudkit/cknotification/titlelocalizationkey)

The key that identifies the localized string for the notification’s title.

Deprecated

[`var titleLocalizationArgs: [String]?`](/documentation/cloudkit/cknotification/titlelocalizationargs)

The fields for building a notification’s title.

Deprecated

[`var subtitle: String?`](/documentation/cloudkit/cknotification/subtitle)

The notification’s subtitle.

Deprecated

[`var subtitleLocalizationKey: String?`](/documentation/cloudkit/cknotification/subtitlelocalizationkey)

The key that identifies the localized string for the notification’s subtitle.

Deprecated

[`var subtitleLocalizationArgs: [String]?`](/documentation/cloudkit/cknotification/subtitlelocalizationargs)

The fields for building a notification’s subtitle.

Deprecated

Current page is subscriptionOwnerUserRecordID