
# CKSubscription | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKSubscription

Class

# CKSubscription

An abstract base class for subscriptions.

```
class CKSubscription
```

## [Overview](/documentation/cloudkit/cksubscription#overview)

A subscription acts like a persistent query on the server that can track the creation, deletion, and modification of records. When changes occur, they trigger the delivery of push notifications so that your app can respond appropriately.

Subscriptions don’t become active until you save them to the server and the server has time to index them. To save a subscription, use an instance of [`CKModifySubscriptionsOperation`](/documentation/cloudkit/ckmodifysubscriptionsoperation) or the [`save(_:completionHandler:)`](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-9pona) method of [`CKDatabase`](/documentation/cloudkit/ckdatabase). To cancel a subscription, delete the corresponding subscription from the server.

Most of a subscription’s configuration happens at initialization time. You must, however, specify how to deliver push notifications to the user’s device. Use the [`notificationInfo`](/documentation/cloudkit/cksubscription/notificationinfo-swift.property) property to configure the delivery options. You must save the subscription before the changes take effect.

### [Handling the Resulting Push Notifications](/documentation/cloudkit/cksubscription#Handling-the-Resulting-Push-Notifications)

When CloudKit modifies a record and triggers a subscription, the server sends push notifications to all devices with that subscription except for the one that makes the original changes. For subscription-based push notifications, the server can add data to the notification payload that indicates the condition that triggers the notification. In the [`application(_:didReceiveRemoteNotification:fetchCompletionHandler:)`](/documentation/UIKit/UIApplicationDelegate/application\(_:didReceiveRemoteNotification:fetchCompletionHandler:\)) method of your app delegate, create a [`CKNotification`](/documentation/cloudkit/cknotification) object from the provided `userInfo` dictionary. You can then query it for the information that’s relevant to the notification.

In addition to sending a record ID with a push notification, you can ask the server to send a limited amount of data from the record that triggers the notification. Use the [`desiredKeys`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/desiredkeys) property of the object you assign to [`notificationInfo`](/documentation/cloudkit/cksubscription/notificationinfo-swift.property) to specify the keys to include.

APNs limits the size of a push notification’s payload and CloudKit may omit keys and other pieces of data to keep the payload’s size under that limit. If this happens, you can fetch the entire payload from the server using an instance of `CKFetchNotificationChangesOperation`. This operation provides instances of [`CKQueryNotification`](/documentation/cloudkit/ckquerynotification) or [`CKRecordZoneNotification`](/documentation/cloudkit/ckrecordzonenotification), which contain information about the push notifications that CloudKit delivers to your app.

## [Topics](/documentation/cloudkit/cksubscription#topics)

### [Specifying the Push Notification Data](/documentation/cloudkit/cksubscription#Specifying-the-Push-Notification-Data)

[`var notificationInfo: CKSubscription.NotificationInfo?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.property)

The configuration for a subscription’s push notifications.

[`class NotificationInfo`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)

An object that describes the configuration of a subscription’s push notifications.

### [Accessing the Subscription Metadata](/documentation/cloudkit/cksubscription#Accessing-the-Subscription-Metadata)

[`var subscriptionID: CKSubscription.ID`](/documentation/cloudkit/cksubscription/subscriptionid-6fp3j)

The subscription’s unique identifier.

[`typealias ID`](/documentation/cloudkit/cksubscription/id)

A type that represents a subscription’s identifier.

[`var subscriptionType: CKSubscription.SubscriptionType`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.property)

The behavior that a subscription provides.

[`enum SubscriptionType`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum)

Constants that identify a subscription’s behavior.

## [Relationships](/documentation/cloudkit/cksubscription#relationships)

### [Inherits From](/documentation/cloudkit/cksubscription#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Inherited By](/documentation/cloudkit/cksubscription#inherited-by)

-   [`CKDatabaseSubscription`](/documentation/cloudkit/ckdatabasesubscription)
-   [`CKQuerySubscription`](/documentation/cloudkit/ckquerysubscription)
-   [`CKRecordZoneSubscription`](/documentation/cloudkit/ckrecordzonesubscription)

### [Conforms To](/documentation/cloudkit/cksubscription#conforms-to)

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

## [See Also](/documentation/cloudkit/cksubscription#see-also)

### [Base Objects](/documentation/cloudkit/cksubscription#Base-Objects)

[`class CKNotification`](/documentation/cloudkit/cknotification)

The abstract base class for CloudKit notifications.

[`class CKDatabaseOperation`](/documentation/cloudkit/ckdatabaseoperation)

The abstract base class for operations that act upon databases in CloudKit.

Current page is CKSubscription

---
  
Specifying the Push Notification Data

# notificationInfo | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   notificationInfo

Instance Property

# notificationInfo

The configuration for a subscription’s push notifications.

```
@NSCopying
var notificationInfo: CKSubscription.NotificationInfo? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.property#Discussion)

If you want the system to display your subscription’s push notifications, assign a value to this property. The server uses the configuration you provide to determine the delivery options for notifications. For example, you can specify the text to display to the user, and the sound to play. You can also specify which fields of the record to include in the notification’s payload.

If you don’t assign a value to this property, CloudKit still sends push notifications, but the system doesn’t display them to the user. The default value of this property is `nil`.

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.property#see-also)

### [Specifying the Push Notification Data](/documentation/cloudkit/cksubscription/notificationinfo-swift.property#Specifying-the-Push-Notification-Data)

[`class NotificationInfo`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)

An object that describes the configuration of a subscription’s push notifications.

Current page is notificationInfo

---
# CKSubscription.NotificationInfo

### Creating Notification Information

# init(alertBody:alertLocalizationKey:alertLocalizationArgs:title:titleLocalizationKey:titleLocalizationArgs:subtitle:subtitleLocalizationKey:subtitleLocalizationArgs:alertActionLocalizationKey:alertLaunchImage:soundName:desiredKeys:shouldBadge:shouldSendContentAvailable:shouldSendMutableContent:category:collapseIDKey:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   init(alertBody:alertLocalizationKey:alertLocalizationArgs:title:titleLocalizationKey:titleLocalizationArgs:subtitle:subtitleLocalizationKey:subtitleLocalizationArgs:alertActionLocalizationKey:alertLaunchImage:soundName:desiredKeys:shouldBadge:shouldSendContentAvailable:shouldSendMutableContent:category:collapseIDKey:)

Initializer

# init(alertBody:alertLocalizationKey:alertLocalizationArgs:title:titleLocalizationKey:titleLocalizationArgs:subtitle:subtitleLocalizationKey:subtitleLocalizationArgs:alertActionLocalizationKey:alertLaunchImage:soundName:desiredKeys:shouldBadge:shouldSendContentAvailable:shouldSendMutableContent:category:collapseIDKey:)

Creates a notification with the specified values.

```
convenience init(
    alertBody: String? = nil,
    alertLocalizationKey: String? = nil,
    alertLocalizationArgs: [CKRecord.FieldKey] = [],
    title: String? = nil,
    titleLocalizationKey: String? = nil,
    titleLocalizationArgs: [CKRecord.FieldKey] = [],
    subtitle: String? = nil,
    subtitleLocalizationKey: String? = nil,
    subtitleLocalizationArgs: [CKRecord.FieldKey] = [],
    alertActionLocalizationKey: String? = nil,
    alertLaunchImage: String? = nil,
    soundName: String? = nil,
    desiredKeys: [CKRecord.FieldKey] = [],
    shouldBadge: Bool = false,
    shouldSendContentAvailable: Bool = false,
    shouldSendMutableContent: Bool = false,
    category: String? = nil,
    collapseIDKey: String? = nil
)
```

## [Parameters](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/init\(alertbody:alertlocalizationkey:alertlocalizationargs:title:titlelocalizationkey:titlelocalizationargs:subtitle:subtitlelocalizationkey:subtitlelocalizationargs:alertactionlocalizationkey:alertlaunchimage:soundname:desiredkeys:shouldbad-47rj5#parameters)

`alertBody`

The text for the notification’s alert.

`alertLocalizationKey`

The key that identifies the localized string for the notification’s alert.

`alertLocalizationArgs`

The fields for building a notification’s alert.

`title`

The notification’s title.

`titleLocalizationKey`

The key that identifies the localized string for the notification’s title.

`titleLocalizationArgs`

The fields for building a notification’s title.

`subtitle`

The notification’s subtitle.

`subtitleLocalizationKey`

The key that identifies the localized string for the notification’s subtitle.

`subtitleLocalizationArgs`

The fields for building a notification’s subtitle.

`alertActionLocalizationKey`

The key that identifies the localized string for the notification’s action.

`alertLaunchImage`

The filename of an image to use as a launch image.

`soundName`

The filename of the sound file to play when a notification arrives.

`desiredKeys`

The names of fields to include in the push notification’s payload.

`shouldBadge`

A Boolean value that determines whether an app’s icon badge increments its value.

`shouldSendContentAvailable`

A Boolean value that indicates whether the push notification includes the content available flag.

`shouldSendMutableContent`

A Boolean value that indicates whether the push notification includes the content available flag.

`category`

The name of the action group that corresponds to this notification.

`collapseIDKey`

The value that the system uses to coalesce unseen push notifications.

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/init\(alertbody:alertlocalizationkey:alertlocalizationargs:title:titlelocalizationkey:titlelocalizationargs:subtitle:subtitlelocalizationkey:subtitlelocalizationargs:alertactionlocalizationkey:alertlaunchimage:soundname:desiredkeys:shouldbad-47rj5#Discussion)

See the corresponding properties for more details about each parameter.

Current page is init(alertBody:alertLocalizationKey:alertLocalizationArgs:title:titleLocalizationKey:titleLocalizationArgs:subtitle:subtitleLocalizationKey:subtitleLocalizationArgs:alertActionLocalizationKey:alertLaunchImage:soundName:desiredKeys:shouldBadge:shouldSendContentAvailable:shouldSendMutableContent:category:collapseIDKey:)

### Grouping Notifications

# category | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   category

Instance Property

# category

The name of the action group that corresponds to this notification.

```
var category: String? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/category#Discussion)

Categories allow you to present custom actions to the user on your push notifications. For more information, see [`UIMutableUserNotificationCategory`](/documentation/UIKit/UIMutableUserNotificationCategory).

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/category#see-also)

### [Grouping Notifications](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/category#Grouping-Notifications)

[`var collapseIDKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/collapseidkey)

A value that the system uses to coalesce unseen push notifications.

Current page is category

# collapseIDKey | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   collapseIDKey

Instance Property

# collapseIDKey

A value that the system uses to coalesce unseen push notifications.

```
var collapseIDKey: String? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/collapseidkey#Discussion)

When CloudKit generates a push notification, it sets the notification’s `apns-collapse-id` header to this property’s value. The system uses this header to coalesce unseen notifications.

See [Sending notification requests to APNs](/documentation/UserNotifications/sending-notification-requests-to-apns) for more information about sending notifications using the Apple Push Notification service.

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/collapseidkey#see-also)

### [Grouping Notifications](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/collapseidkey#Grouping-Notifications)

[`var category: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/category)

The name of the action group that corresponds to this notification.

Current page is collapseIDKey

  
### Displaying Badges
# shouldBadge | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   shouldBadge

Instance Property

# shouldBadge

A Boolean value that determines whether an app’s icon badge increments its value.

```
var shouldBadge: Bool { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/shouldbadge#Discussion)

The default value of this property is [`false`](/documentation/swift/false). Set it to [`true`](/documentation/swift/true) to cause the system to increment the badge value whenever it receives the corresponding push notification.

Current page is shouldBadge

---
  
### Accessing the Notification Alert

# alertBody | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   alertBody

Instance Property

# alertBody

The text for the notification’s alert.

```
var alertBody: String? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertbody#Discussion)

Set this property’s value to have the system display the specified string when it receives the corresponding push notification. If you localize your app’s content, use the [`alertLocalizationKey`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationkey) property instead.

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertbody#see-also)

### [Accessing the Notification Alert](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertbody#Accessing-the-Notification-Alert)

[`var alertLocalizationKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationkey)

The key that identifies the localized string for the notification’s alert.

[`var alertLocalizationArgs: [CKRecord.FieldKey]?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationargs)

The fields for building a notification’s alert.

[`var alertActionLocalizationKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertactionlocalizationkey)

The key that identifies the localized string for the notification’s action.

[`var alertLaunchImage: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlaunchimage)

The filename of an image to use as a launch image.

[`var soundName: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/soundname)

The filename of the sound file to play when a notification arrives.

Current page is alertBody

# alertLocalizationKey | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   alertLocalizationKey

Instance Property

# alertLocalizationKey

The key that identifies the localized string for the notification’s alert.

```
var alertLocalizationKey: String? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationkey#Discussion)

Set this property’s value to have the system display a localized string when it receives the corresponding push notification. The system uses the key to find the matching string in your app’s `Localizable.string` file. If you specify a value for this property, CloudKit ignores the [`alertBody`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertbody) property’s value.

For information about localizing string resources, see [Internationalization and Localization Guide](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i).

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationkey#see-also)

### [Accessing the Notification Alert](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationkey#Accessing-the-Notification-Alert)

[`var alertBody: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertbody)

The text for the notification’s alert.

[`var alertLocalizationArgs: [CKRecord.FieldKey]?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationargs)

The fields for building a notification’s alert.

[`var alertActionLocalizationKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertactionlocalizationkey)

The key that identifies the localized string for the notification’s action.

[`var alertLaunchImage: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlaunchimage)

The filename of an image to use as a launch image.

[`var soundName: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/soundname)

The filename of the sound file to play when a notification arrives.

Current page is alertLocalizationKey

# alertLocalizationArgs | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   alertLocalizationArgs

Instance Property

# alertLocalizationArgs

The fields for building a notification’s alert.

```
var alertLocalizationArgs: [CKRecord.FieldKey]? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationargs#Discussion)

This property is an array of fields that CloudKit uses to extract the corresponding values from the record that triggers the push notification. The values must be strings, numbers, or dates. Don’t specify keys that use other value types. CloudKit may truncate strings with a length greater than 100 characters when it adds them to a notification’s payload.

If you use `%@` for your substitution variables, CloudKit replaces those variables by traversing the array in order. If you use variables of the form `%n$@`, where `n` is an integer, `n` represents the index (starting at 1) of the item in the array to use. So, the first item in the array replaces the variable `%1$@`, the second item replaces the variable `%2$@`, and so on. You can use indexed substitution variables to change the order of items in the resulting string, which might be necessary when you localize your app’s content.

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationargs#see-also)

### [Accessing the Notification Alert](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationargs#Accessing-the-Notification-Alert)

[`var alertBody: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertbody)

The text for the notification’s alert.

[`var alertLocalizationKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationkey)

The key that identifies the localized string for the notification’s alert.

[`var alertActionLocalizationKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertactionlocalizationkey)

The key that identifies the localized string for the notification’s action.

[`var alertLaunchImage: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlaunchimage)

The filename of an image to use as a launch image.

[`var soundName: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/soundname)

The filename of the sound file to play when a notification arrives.

Current page is alertLocalizationArgs

# alertActionLocalizationKey | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   alertActionLocalizationKey

Instance Property

# alertActionLocalizationKey

The key that identifies the localized string for the notification’s action.

```
var alertActionLocalizationKey: String? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertactionlocalizationkey#Discussion)

Set this property’s value to have the system use a localized string for the text of the notification’s button that opens your app. The system uses the key to find the matching string in your app’s `Localizable.string` file.

If this property’s value is `nil`, the system displays a single button to dismiss the alert.

For information about localizing string resources, see [Internationalization and Localization Guide](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i).

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertactionlocalizationkey#see-also)

### [Accessing the Notification Alert](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertactionlocalizationkey#Accessing-the-Notification-Alert)

[`var alertBody: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertbody)

The text for the notification’s alert.

[`var alertLocalizationKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationkey)

The key that identifies the localized string for the notification’s alert.

[`var alertLocalizationArgs: [CKRecord.FieldKey]?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationargs)

The fields for building a notification’s alert.

[`var alertLaunchImage: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlaunchimage)

The filename of an image to use as a launch image.

[`var soundName: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/soundname)

The filename of the sound file to play when a notification arrives.

Current page is alertActionLocalizationKey

# alertLaunchImage | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   alertLaunchImage

Instance Property

# alertLaunchImage

The filename of an image to use as a launch image.

```
var alertLaunchImage: String? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlaunchimage#Discussion)

If you specify a value, the system uses it to locate an image in the app’s bundle, and displays it as a launch image when the user launches the app after receiving a push notification.

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlaunchimage#see-also)

### [Accessing the Notification Alert](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlaunchimage#Accessing-the-Notification-Alert)

[`var alertBody: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertbody)

The text for the notification’s alert.

[`var alertLocalizationKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationkey)

The key that identifies the localized string for the notification’s alert.

[`var alertLocalizationArgs: [CKRecord.FieldKey]?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationargs)

The fields for building a notification’s alert.

[`var alertActionLocalizationKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertactionlocalizationkey)

The key that identifies the localized string for the notification’s action.

[`var soundName: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/soundname)

The filename of the sound file to play when a notification arrives.

Current page is alertLaunchImage

# soundName | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   soundName

Instance Property

# soundName

The filename of the sound file to play when a notification arrives.

```
var soundName: String? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/soundname#Discussion)

If you specify a value, the system uses it to locate a sound file in the app’s bundle. The sound plays when the system receives a push notification. If the system can’t find the specified file, or if you use the string `default`, the system plays the default sound.

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/soundname#see-also)

### [Accessing the Notification Alert](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/soundname#Accessing-the-Notification-Alert)

[`var alertBody: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertbody)

The text for the notification’s alert.

[`var alertLocalizationKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationkey)

The key that identifies the localized string for the notification’s alert.

[`var alertLocalizationArgs: [CKRecord.FieldKey]?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlocalizationargs)

The fields for building a notification’s alert.

[`var alertActionLocalizationKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertactionlocalizationkey)

The key that identifies the localized string for the notification’s action.

[`var alertLaunchImage: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/alertlaunchimage)

The filename of an image to use as a launch image.

Current page is soundName

### Accessing the Notification Info

# shouldSendContentAvailable | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   shouldSendContentAvailable

Instance Property

# shouldSendContentAvailable

A Boolean value that indicates whether the push notification includes the content available flag.

```
var shouldSendContentAvailable: Bool { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/shouldsendcontentavailable#Discussion)

When this property is [`true`](/documentation/swift/true), the server includes the `content-available` flag in the push notification’s payload. That flag causes the system to wake or launch an app that isn’t currently running. The app then receives background execution time to download any data for the push notification, such as the set of changed records. If the app is already running in the foreground, the inclusion of this flag has no additional effect and the system delivers the notification to the app delegate for processing as usual.

The default value of this property is [`false`](/documentation/swift/false).

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/shouldsendcontentavailable#see-also)

### [Accessing the Notification Info](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/shouldsendcontentavailable#Accessing-the-Notification-Info)

[`var shouldSendMutableContent: Bool`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/shouldsendmutablecontent)

A Boolean value that indicates whether the push notification sets the mutable content flag.

Current page is shouldSendContentAvailable

# shouldSendMutableContent | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   shouldSendMutableContent

Instance Property

# shouldSendMutableContent

A Boolean value that indicates whether the push notification sets the mutable content flag.

```
var shouldSendMutableContent: Bool { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/shouldsendmutablecontent#Discussion)

When this property is [`true`](/documentation/swift/true), the server includes the `mutable-content` flag with a value of `1` in the push notification’s payload. When the value is `1`, the system passes the notification to your app extension for modification before delivery.

See [Generating a remote notification](/documentation/UserNotifications/generating-a-remote-notification) for more information about the `mutable-content` flag, and [Modifying content in newly delivered notifications](/documentation/UserNotifications/modifying-content-in-newly-delivered-notifications) for information about how to modify push notifiction content in your app extension prior to delivery.

The default value of this property is [`false`](/documentation/swift/false).

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/shouldsendmutablecontent#see-also)

### [Accessing the Notification Info](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/shouldsendmutablecontent#Accessing-the-Notification-Info)

[`var shouldSendContentAvailable: Bool`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/shouldsendcontentavailable)

A Boolean value that indicates whether the push notification includes the content available flag.

Current page is shouldSendMutableContent

### Accessing the Record’s Data

# desiredKeys | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   desiredKeys

Instance Property

# desiredKeys

The names of fields to include in the push notification’s payload.

```
var desiredKeys: [CKRecord.FieldKey]? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/desiredkeys#Discussion)

This property contains an array of strings, each of which corresponds to the name of a field in the record that triggers the notification. When the system receives a notification, it includes the keys and their corresponding values. You can request a maximum of three keys.

For the keys you specify, the allowable types are [`NSString`](/documentation/Foundation/NSString), [`NSNumber`](/documentation/Foundation/NSNumber), [`CLLocation`](/documentation/CoreLocation/CLLocation), [`NSDate`](/documentation/Foundation/NSDate), and [`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference). You can’t specify keys with values that contain other data types. CloudKit may truncate strings that are more than 100 characters when it adds them to the notification’s payload.

Current page is desiredKeys

  
### Accessing the Notification Title

# title | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   title

Instance Property

# title

The notification’s title.

```
var title: String? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/title#Discussion)

CloudKit uses this value to set the `title` push notification property.

See [Generating a remote notification](/documentation/UserNotifications/generating-a-remote-notification) for more detail about push notification properties.

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/title#see-also)

### [Accessing the Notification Title](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/title#Accessing-the-Notification-Title)

[`var titleLocalizationKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/titlelocalizationkey)

The key that identifies the localized string for the notification’s title.

[`var titleLocalizationArgs: [CKRecord.FieldKey]?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/titlelocalizationargs)

The fields for building a notification’s title.

Current page is title

# titleLocalizationKey | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   titleLocalizationKey

Instance Property

# titleLocalizationKey

The key that identifies the localized string for the notification’s title.

```
var titleLocalizationKey: String? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/titlelocalizationkey#Discussion)

CloudKit uses this value to set the `title-loc-key` push notification property.

See [Generating a remote notification](/documentation/UserNotifications/generating-a-remote-notification) for more details about push notification properties.

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/titlelocalizationkey#see-also)

### [Accessing the Notification Title](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/titlelocalizationkey#Accessing-the-Notification-Title)

[`var title: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/title)

The notification’s title.

[`var titleLocalizationArgs: [CKRecord.FieldKey]?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/titlelocalizationargs)

The fields for building a notification’s title.

Current page is titleLocalizationKey

# titleLocalizationArgs | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   titleLocalizationArgs

Instance Property

# titleLocalizationArgs

The fields for building a notification’s title.

```
var titleLocalizationArgs: [CKRecord.FieldKey]? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/titlelocalizationargs#Discussion)

This property is an array of field names that CloudKit uses to extract the corresponding values from the record that triggers the push notification. The values must be strings, numbers, or dates. Don’t specify keys that use other value types. CloudKit may truncate strings with a length greater than 100 characters when it adds them to a notification’s payload.

If you use `%@` for your substitution variables, CloudKit replaces those variables by traversing the array in order. If you use variables of the form `%n$@`, where `n` is an integer, `n` represents the index (starting at 1) of the item in the array to use. So, the first item in the array replaces the variable `%1$@`, the second item replaces the variable `%2$@`, and so on. You can use indexed substitution variables to change the order of items in the resulting string, which might be necessary when you localize your app’s content.

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/titlelocalizationargs#see-also)

### [Accessing the Notification Title](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/titlelocalizationargs#Accessing-the-Notification-Title)

[`var title: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/title)

The notification’s title.

[`var titleLocalizationKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/titlelocalizationkey)

The key that identifies the localized string for the notification’s title.

Current page is titleLocalizationArgs

### Accessing the Notification Subtitle

# subtitle | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   subtitle

Instance Property

# subtitle

The notification’s subtitle.

```
var subtitle: String? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitle#Discussion)

CloudKit uses this value to set the `subtitle` push notification property. If you set [`subtitleLocalizationKey`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitlelocalizationkey), CloudKit ignores this value.

See [Generating a remote notification](/documentation/UserNotifications/generating-a-remote-notification) for more details about push notification properties.

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitle#see-also)

### [Accessing the Notification Subtitle](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitle#Accessing-the-Notification-Subtitle)

[`var subtitleLocalizationKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitlelocalizationkey)

The key that identifies the localized string for the notification’s subtitle.

[`var subtitleLocalizationArgs: [CKRecord.FieldKey]?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitlelocalizationargs)

The fields for building a notification’s subtitle.

Current page is subtitle

# subtitleLocalizationKey | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   subtitleLocalizationKey

Instance Property

# subtitleLocalizationKey

The key that identifies the localized string for the notification’s subtitle.

```
var subtitleLocalizationKey: String? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitlelocalizationkey#Discussion)

CloudKit uses this value to set the `subtitle-loc-key` push notification property. Setting this property overrides any value in [`subtitle`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitle).

See [Generating a remote notification](/documentation/UserNotifications/generating-a-remote-notification) for more details about push notification properties.

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitlelocalizationkey#see-also)

### [Accessing the Notification Subtitle](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitlelocalizationkey#Accessing-the-Notification-Subtitle)

[`var subtitle: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitle)

The notification’s subtitle.

[`var subtitleLocalizationArgs: [CKRecord.FieldKey]?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitlelocalizationargs)

The fields for building a notification’s subtitle.

Current page is subtitleLocalizationKey

# subtitleLocalizationArgs | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.NotificationInfo](/documentation/cloudkit/cksubscription/notificationinfo-swift.class)
-   subtitleLocalizationArgs

Instance Property

# subtitleLocalizationArgs

The fields for building a notification’s subtitle.

```
var subtitleLocalizationArgs: [CKRecord.FieldKey]? { get set }
```

## [Discussion](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitlelocalizationargs#Discussion)

This property is an array of field names that CloudKit uses to extract the corresponding values from the record that triggers the push notification. The values must be strings, numbers, or dates. Don’t specify keys that use other value types. CloudKit may truncate strings with a length greater than 100 characters when it adds them to a notification’s payload.

If you use `%@` for your substitution variables, CloudKit replaces those variables by traversing the array in order. If you use variables of the form `%n$@`, where `n` is an integer, `n` represents the index (starting at 1) of the item in the array to use. So, the first item in the array replaces the variable `%1$@`, the second item replaces the variable `%2$@`, and so on. You can use indexed substitution variables to change the order of items in the resulting string, which might be necessary when you localize your app’s content.

## [See Also](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitlelocalizationargs#see-also)

### [Accessing the Notification Subtitle](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitlelocalizationargs#Accessing-the-Notification-Subtitle)

[`var subtitle: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitle)

The notification’s subtitle.

[`var subtitleLocalizationKey: String?`](/documentation/cloudkit/cksubscription/notificationinfo-swift.class/subtitlelocalizationkey)

The key that identifies the localized string for the notification’s subtitle.

Current page is subtitleLocalizationArgs

---
  
Accessing the Subscription Metadata

# subscriptionID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   subscriptionID

Instance Property

# subscriptionID

The subscription’s unique identifier.

```
var subscriptionID: CKSubscription.ID { get }
```

## [Discussion](/documentation/cloudkit/cksubscription/subscriptionid-6fp3j#Discussion)

This property’s value is the subscription ID that you provide to the `init(recordType:predicate:subscriptionID:options:)` or `init(zoneID:subscriptionID:options:)` methods when you create the subscription. If you use a different method to create the subscription, CloudKit automatically assigns a UUID as the subscription ID.

## [See Also](/documentation/cloudkit/cksubscription/subscriptionid-6fp3j#see-also)

### [Accessing the Subscription Metadata](/documentation/cloudkit/cksubscription/subscriptionid-6fp3j#Accessing-the-Subscription-Metadata)

[`typealias ID`](/documentation/cloudkit/cksubscription/id)

A type that represents a subscription’s identifier.

[`var subscriptionType: CKSubscription.SubscriptionType`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.property)

The behavior that a subscription provides.

[`enum SubscriptionType`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum)

Constants that identify a subscription’s behavior.

Current page is subscriptionID

# CKSubscription.ID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   CKSubscription.ID

Type Alias

# CKSubscription.ID

A type that represents a subscription’s identifier.

```
typealias ID = String
```

## [See Also](/documentation/cloudkit/cksubscription/id#see-also)

### [Accessing the Subscription Metadata](/documentation/cloudkit/cksubscription/id#Accessing-the-Subscription-Metadata)

[`var subscriptionID: CKSubscription.ID`](/documentation/cloudkit/cksubscription/subscriptionid-6fp3j)

The subscription’s unique identifier.

[`var subscriptionType: CKSubscription.SubscriptionType`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.property)

The behavior that a subscription provides.

[`enum SubscriptionType`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum)

Constants that identify a subscription’s behavior.

Current page is CKSubscription.ID

# subscriptionType | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   subscriptionType

Instance Property

# subscriptionType

The behavior that a subscription provides.

```
var subscriptionType: CKSubscription.SubscriptionType { get }
```

## [See Also](/documentation/cloudkit/cksubscription/subscriptiontype-swift.property#see-also)

### [Accessing the Subscription Metadata](/documentation/cloudkit/cksubscription/subscriptiontype-swift.property#Accessing-the-Subscription-Metadata)

[`var subscriptionID: CKSubscription.ID`](/documentation/cloudkit/cksubscription/subscriptionid-6fp3j)

The subscription’s unique identifier.

[`typealias ID`](/documentation/cloudkit/cksubscription/id)

A type that represents a subscription’s identifier.

[`enum SubscriptionType`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum)

Constants that identify a subscription’s behavior.

Current page is subscriptionType

---

# CKSubscription.SubscriptionType

Subscription types
# CKSubscription.SubscriptionType.query | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.SubscriptionType](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum)
-   CKSubscription.SubscriptionType.query

Case

# CKSubscription.SubscriptionType.query

A constant that indicates the subscription is query-based.

```
case query
```

## [See Also](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum/query#see-also)

### [Subscription Types](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum/query#Subscription-Types)

[`case database`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum/database)

A constant that indicates the subscription is database-based.

[`case recordZone`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum/recordzone)

A constant that indicates the subscription is zone-based.

Current page is CKSubscription.SubscriptionType.query

# CKSubscription.SubscriptionType.database | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.SubscriptionType](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum)
-   CKSubscription.SubscriptionType.database

Case

# CKSubscription.SubscriptionType.database

A constant that indicates the subscription is database-based.

```
case database
```

## [See Also](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum/database#see-also)

### [Subscription Types](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum/database#Subscription-Types)

[`case query`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum/query)

A constant that indicates the subscription is query-based.

[`case recordZone`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum/recordzone)

A constant that indicates the subscription is zone-based.

Current page is CKSubscription.SubscriptionType.database

# CKSubscription.SubscriptionType.recordZone | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.SubscriptionType](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum)
-   CKSubscription.SubscriptionType.recordZone

Case

# CKSubscription.SubscriptionType.recordZone

A constant that indicates the subscription is zone-based.

```
case recordZone
```

## [See Also](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum/recordzone#see-also)

### [Subscription Types](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum/recordzone#Subscription-Types)

[`case query`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum/query)

A constant that indicates the subscription is query-based.

[`case database`](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum/database)

A constant that indicates the subscription is database-based.

Current page is CKSubscription.SubscriptionType.recordZone

# init(rawValue:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSubscription](/documentation/cloudkit/cksubscription)
-   [CKSubscription.SubscriptionType](/documentation/cloudkit/cksubscription/subscriptiontype-swift.enum)
-   init(rawValue:)

Initializer

# init(rawValue:)

```
init?(rawValue: Int)
```

Current page is init(rawValue:)




