# CKAllowedSharingOptions | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKAllowedSharingOptions

Class

# CKAllowedSharingOptions

An object that controls participant access and permission options.

```
class CKAllowedSharingOptions
```

## [Overview](documentation/cloudkit/CKAllowedSharingOptions.md#overview)

Register an instance of this class with an [`NSItemProvider`](/documentation/Foundation/NSItemProvider) or when preparing a [`CKShareTransferRepresentation.ExportedShare`](/documentation/cloudkit/cksharetransferrepresentation/exportedshare) before your app invokes the share sheet. The share sheet uses the registered `CKAllowedSharingOptions` object to let the user choose between the allowed options when sharing.

## [Topics](documentation/cloudkit/CKAllowedSharingOptions.md#topics)

### [Creating sharing options](documentation/cloudkit/CKAllowedSharingOptions.md#Creating-sharing-options)

[`init(allowedParticipantPermissionOptions: CKSharingParticipantPermissionOption, allowedParticipantAccessOptions: CKSharingParticipantAccessOption)`](/documentation/cloudkit/ckallowedsharingoptions/init\(allowedparticipantpermissionoptions:allowedparticipantaccessoptions:\))

Creates and initializes an allowed sharing options object.

### [Using the standard options](documentation/cloudkit/CKAllowedSharingOptions.md#Using-the-standard-options)

[`class var standard: CKAllowedSharingOptions`](/documentation/cloudkit/ckallowedsharingoptions/standard)

An object set to the most permissive sharing options.

### [Configuring the options](documentation/cloudkit/CKAllowedSharingOptions.md#Configuring-the-options)

[`var allowedParticipantAccessOptions: CKSharingParticipantAccessOption`](/documentation/cloudkit/ckallowedsharingoptions/allowedparticipantaccessoptions)

The permission option the system uses to control whether a user can share publicly or privately.

[`var allowedParticipantPermissionOptions: CKSharingParticipantPermissionOption`](/documentation/cloudkit/ckallowedsharingoptions/allowedparticipantpermissionoptions)

The permission option the system uses to control whether a user can grant read-only or write access.

[`struct CKSharingParticipantAccessOption`](/documentation/cloudkit/cksharingparticipantaccessoption)

An object that controls participant access options.

[`struct CKSharingParticipantPermissionOption`](/documentation/cloudkit/cksharingparticipantpermissionoption)

An object that controls participant permission options.

### [Instance Properties](documentation/cloudkit/CKAllowedSharingOptions.md#Instance-Properties)

[`var allowsAccessRequests: Bool`](/documentation/cloudkit/ckallowedsharingoptions/allowsaccessrequests)

Default value is `NO`. If set, the system sharing UI will allow the user to configure whether access requests are enabled on the share.

[`var allowsParticipantsToInviteOthers: Bool`](/documentation/cloudkit/ckallowedsharingoptions/allowsparticipantstoinviteothers)

Default value is `NO`. If set, the system sharing UI will allow the user to choose whether added participants can invite others to the share. Shares with [`CKShare.ParticipantRole.administrator`](/documentation/cloudkit/ckshare/participantrole/administrator) participants will be returned as read-only to devices running OS versions prior to this role being introduced. Administrator participants on these read-only shares will be returned as [`CKShare.ParticipantRole.privateUser`](/documentation/cloudkit/ckshare/participantrole/privateuser).

## [Relationships](documentation/cloudkit/CKAllowedSharingOptions.md#relationships)

### [Inherits From](documentation/cloudkit/CKAllowedSharingOptions.md#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](documentation/cloudkit/CKAllowedSharingOptions.md#conforms-to)

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

## [See Also](documentation/cloudkit/CKAllowedSharingOptions.md#see-also)

### [Collaboration](documentation/cloudkit/CKAllowedSharingOptions.md#Collaboration)

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

[`class CKSystemSharingUIObserver`](/documentation/cloudkit/cksystemsharinguiobserver)

An object the system uses to monitor changes in sharing.

[`@MainActor class UICloudSharingController`](/documentation/UIKit/UICloudSharingController)

A view controller that presents standard screens for adding and removing people from a CloudKit share record.

[`CKSharingSupported`](/documentation/BundleResources/Information-Property-List/CKSharingSupported)

A Boolean value that indicates your app supports CloudKit Sharing.

Current page is CKAllowedSharingOptions

---
  
Creating sharing options

# init(allowedParticipantPermissionOptions:allowedParticipantAccessOptions:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKAllowedSharingOptions](/documentation/cloudkit/ckallowedsharingoptions)
-   init(allowedParticipantPermissionOptions:allowedParticipantAccessOptions:)

Initializer

# init(allowedParticipantPermissionOptions:allowedParticipantAccessOptions:)

Creates and initializes an allowed sharing options object.

```
init(
    allowedParticipantPermissionOptions: CKSharingParticipantPermissionOption,
    allowedParticipantAccessOptions: CKSharingParticipantAccessOption
)
```

## [Parameters](/documentation/cloudkit/ckallowedsharingoptions/init\(allowedparticipantpermissionoptions:allowedparticipantaccessoptions:\)#parameters)

`allowedParticipantPermissionOptions`

The [`CKSharingParticipantPermissionOption`](/documentation/cloudkit/cksharingparticipantpermissionoption) setting.

`allowedParticipantAccessOptions`

The [`CKSharingParticipantAccessOption`](/documentation/cloudkit/cksharingparticipantaccessoption) setting.

Current page is init(allowedParticipantPermissionOptions:allowedParticipantAccessOptions:)

---
### Using the standard options

# standard | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKAllowedSharingOptions](/documentation/cloudkit/ckallowedsharingoptions)
-   standard

Type Property

# standard

An object set to the most permissive sharing options.

```
class var standard: CKAllowedSharingOptions { get }
```

## [Discussion](/documentation/cloudkit/ckallowedsharingoptions/standard#Discussion)

The `standardOptions` has [`allowedParticipantPermissionOptions`](/documentation/cloudkit/ckallowedsharingoptions/allowedparticipantpermissionoptions) set to `CKSharingParticipantPermissionOptionAny` and [`allowedParticipantAccessOptions`](/documentation/cloudkit/ckallowedsharingoptions/allowedparticipantaccessoptions) set to `CKSharingParticipantAccessOptionAny`.

Current page is standard

---
  
Configuring the options

# allowedParticipantAccessOptions | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKAllowedSharingOptions](/documentation/cloudkit/ckallowedsharingoptions)
-   allowedParticipantAccessOptions

Instance Property

# allowedParticipantAccessOptions

The permission option the system uses to control whether a user can share publicly or privately.

```
var allowedParticipantAccessOptions: CKSharingParticipantAccessOption { get set }
```

## [See Also](/documentation/cloudkit/ckallowedsharingoptions/allowedparticipantaccessoptions#see-also)

### [Configuring the options](/documentation/cloudkit/ckallowedsharingoptions/allowedparticipantaccessoptions#Configuring-the-options)

[`var allowedParticipantPermissionOptions: CKSharingParticipantPermissionOption`](/documentation/cloudkit/ckallowedsharingoptions/allowedparticipantpermissionoptions)

The permission option the system uses to control whether a user can grant read-only or write access.

[`struct CKSharingParticipantAccessOption`](/documentation/cloudkit/cksharingparticipantaccessoption)

An object that controls participant access options.

[`struct CKSharingParticipantPermissionOption`](/documentation/cloudkit/cksharingparticipantpermissionoption)

An object that controls participant permission options.

Current page is allowedParticipantAccessOptions

# allowedParticipantPermissionOptions | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKAllowedSharingOptions](/documentation/cloudkit/ckallowedsharingoptions)
-   allowedParticipantPermissionOptions

Instance Property

# allowedParticipantPermissionOptions

The permission option the system uses to control whether a user can grant read-only or write access.

```
var allowedParticipantPermissionOptions: CKSharingParticipantPermissionOption { get set }
```

## [See Also](/documentation/cloudkit/ckallowedsharingoptions/allowedparticipantpermissionoptions#see-also)

### [Configuring the options](/documentation/cloudkit/ckallowedsharingoptions/allowedparticipantpermissionoptions#Configuring-the-options)

[`var allowedParticipantAccessOptions: CKSharingParticipantAccessOption`](/documentation/cloudkit/ckallowedsharingoptions/allowedparticipantaccessoptions)

The permission option the system uses to control whether a user can share publicly or privately.

[`struct CKSharingParticipantAccessOption`](/documentation/cloudkit/cksharingparticipantaccessoption)

An object that controls participant access options.

[`struct CKSharingParticipantPermissionOption`](/documentation/cloudkit/cksharingparticipantpermissionoption)

An object that controls participant permission options.

Current page is allowedParticipantPermissionOptions

# CKSharingParticipantAccessOption | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKSharingParticipantAccessOption

Structure

# CKSharingParticipantAccessOption

An object that controls participant access options.

```
struct CKSharingParticipantAccessOption
```

## [Topics](/documentation/cloudkit/cksharingparticipantaccessoption#topics)

### [Creating an access option](/documentation/cloudkit/cksharingparticipantaccessoption#Creating-an-access-option)

[`init(rawValue: UInt)`](/documentation/cloudkit/cksharingparticipantaccessoption/init\(rawvalue:\))

Creates and initializes a participant access option object.

### [Configuring the options](/documentation/cloudkit/cksharingparticipantaccessoption#Configuring-the-options)

[`static var any: CKSharingParticipantAccessOption`](/documentation/cloudkit/cksharingparticipantaccessoption/any)

The permission option the system uses to control whether a user can share publicly or privately.

[`static var anyoneWithLink: CKSharingParticipantAccessOption`](/documentation/cloudkit/cksharingparticipantaccessoption/anyonewithlink)

The permission option the system uses to control whether a user can share publicly.

[`static var specifiedRecipientsOnly: CKSharingParticipantAccessOption`](/documentation/cloudkit/cksharingparticipantaccessoption/specifiedrecipientsonly)

The permission option the system uses to control whether a user can share privately.

## [Relationships](/documentation/cloudkit/cksharingparticipantaccessoption#relationships)

### [Conforms To](/documentation/cloudkit/cksharingparticipantaccessoption#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`ExpressibleByArrayLiteral`](/documentation/Swift/ExpressibleByArrayLiteral)
-   [`OptionSet`](/documentation/Swift/OptionSet)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)
-   [`SetAlgebra`](/documentation/Swift/SetAlgebra)

## [See Also](/documentation/cloudkit/cksharingparticipantaccessoption#see-also)

### [Configuring the options](/documentation/cloudkit/cksharingparticipantaccessoption#Configuring-the-options)

[`var allowedParticipantAccessOptions: CKSharingParticipantAccessOption`](/documentation/cloudkit/ckallowedsharingoptions/allowedparticipantaccessoptions)

The permission option the system uses to control whether a user can share publicly or privately.

[`var allowedParticipantPermissionOptions: CKSharingParticipantPermissionOption`](/documentation/cloudkit/ckallowedsharingoptions/allowedparticipantpermissionoptions)

The permission option the system uses to control whether a user can grant read-only or write access.

[`struct CKSharingParticipantPermissionOption`](/documentation/cloudkit/cksharingparticipantpermissionoption)

An object that controls participant permission options.

Current page is CKSharingParticipantAccessOption

# init(rawValue:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSharingParticipantAccessOption](/documentation/cloudkit/cksharingparticipantaccessoption)
-   init(rawValue:)

Initializer

# init(rawValue:)

Creates and initializes a participant access option object.

```
init(rawValue: UInt)
```

## [Parameters](/documentation/cloudkit/cksharingparticipantaccessoption/init\(rawvalue:\)#parameters)

`rawValue`

An unsigned integer access value.

Current page is init(rawValue:)

# any | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSharingParticipantAccessOption](/documentation/cloudkit/cksharingparticipantaccessoption)
-   any

Type Property

# any

The permission option the system uses to control whether a user can share publicly or privately.

```
static var any: CKSharingParticipantAccessOption { get }
```

## [See Also](/documentation/cloudkit/cksharingparticipantaccessoption/any#see-also)

### [Configuring the options](/documentation/cloudkit/cksharingparticipantaccessoption/any#Configuring-the-options)

[`static var anyoneWithLink: CKSharingParticipantAccessOption`](/documentation/cloudkit/cksharingparticipantaccessoption/anyonewithlink)

The permission option the system uses to control whether a user can share publicly.

[`static var specifiedRecipientsOnly: CKSharingParticipantAccessOption`](/documentation/cloudkit/cksharingparticipantaccessoption/specifiedrecipientsonly)

The permission option the system uses to control whether a user can share privately.

Current page is any

# anyoneWithLink | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSharingParticipantAccessOption](/documentation/cloudkit/cksharingparticipantaccessoption)
-   anyoneWithLink

Type Property

# anyoneWithLink

The permission option the system uses to control whether a user can share publicly.

```
static var anyoneWithLink: CKSharingParticipantAccessOption { get }
```

## [See Also](/documentation/cloudkit/cksharingparticipantaccessoption/anyonewithlink#see-also)

### [Configuring the options](/documentation/cloudkit/cksharingparticipantaccessoption/anyonewithlink#Configuring-the-options)

[`static var any: CKSharingParticipantAccessOption`](/documentation/cloudkit/cksharingparticipantaccessoption/any)

The permission option the system uses to control whether a user can share publicly or privately.

[`static var specifiedRecipientsOnly: CKSharingParticipantAccessOption`](/documentation/cloudkit/cksharingparticipantaccessoption/specifiedrecipientsonly)

The permission option the system uses to control whether a user can share privately.

Current page is anyoneWithLink

# specifiedRecipientsOnly | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSharingParticipantAccessOption](/documentation/cloudkit/cksharingparticipantaccessoption)
-   specifiedRecipientsOnly

Type Property

# specifiedRecipientsOnly

The permission option the system uses to control whether a user can share privately.

```
static var specifiedRecipientsOnly: CKSharingParticipantAccessOption { get }
```

## [See Also](/documentation/cloudkit/cksharingparticipantaccessoption/specifiedrecipientsonly#see-also)

### [Configuring the options](/documentation/cloudkit/cksharingparticipantaccessoption/specifiedrecipientsonly#Configuring-the-options)

[`static var any: CKSharingParticipantAccessOption`](/documentation/cloudkit/cksharingparticipantaccessoption/any)

The permission option the system uses to control whether a user can share publicly or privately.

[`static var anyoneWithLink: CKSharingParticipantAccessOption`](/documentation/cloudkit/cksharingparticipantaccessoption/anyonewithlink)

The permission option the system uses to control whether a user can share publicly.

Current page is specifiedRecipientsOnly

# CKSharingParticipantPermissionOption | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKSharingParticipantPermissionOption

Structure

# CKSharingParticipantPermissionOption

An object that controls participant permission options.

```
struct CKSharingParticipantPermissionOption
```

## [Topics](/documentation/cloudkit/cksharingparticipantpermissionoption#topics)

### [Creating an access option](/documentation/cloudkit/cksharingparticipantpermissionoption#Creating-an-access-option)

[`init(rawValue: UInt)`](/documentation/cloudkit/cksharingparticipantpermissionoption/init\(rawvalue:\))

Creates and initializes a participant permission option object.

### [Configuring the options](/documentation/cloudkit/cksharingparticipantpermissionoption#Configuring-the-options)

[`static var any: CKSharingParticipantPermissionOption`](/documentation/cloudkit/cksharingparticipantpermissionoption/any)

The permission option the system uses to control whether a user can grant read-only or write access.

[`static var readOnly: CKSharingParticipantPermissionOption`](/documentation/cloudkit/cksharingparticipantpermissionoption/readonly)

The permission option the system uses to control whether a user can grant read-only access.

[`static var readWrite: CKSharingParticipantPermissionOption`](/documentation/cloudkit/cksharingparticipantpermissionoption/readwrite)

The permission option the system uses to control whether a user can grant write access.

## [Relationships](/documentation/cloudkit/cksharingparticipantpermissionoption#relationships)

### [Conforms To](/documentation/cloudkit/cksharingparticipantpermissionoption#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`ExpressibleByArrayLiteral`](/documentation/Swift/ExpressibleByArrayLiteral)
-   [`OptionSet`](/documentation/Swift/OptionSet)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)
-   [`SetAlgebra`](/documentation/Swift/SetAlgebra)

## [See Also](/documentation/cloudkit/cksharingparticipantpermissionoption#see-also)

### [Configuring the options](/documentation/cloudkit/cksharingparticipantpermissionoption#Configuring-the-options)

[`var allowedParticipantAccessOptions: CKSharingParticipantAccessOption`](/documentation/cloudkit/ckallowedsharingoptions/allowedparticipantaccessoptions)

The permission option the system uses to control whether a user can share publicly or privately.

[`var allowedParticipantPermissionOptions: CKSharingParticipantPermissionOption`](/documentation/cloudkit/ckallowedsharingoptions/allowedparticipantpermissionoptions)

The permission option the system uses to control whether a user can grant read-only or write access.

[`struct CKSharingParticipantAccessOption`](/documentation/cloudkit/cksharingparticipantaccessoption)

An object that controls participant access options.

Current page is CKSharingParticipantPermissionOption

# any | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSharingParticipantPermissionOption](/documentation/cloudkit/cksharingparticipantpermissionoption)
-   any

Type Property

# any

The permission option the system uses to control whether a user can grant read-only or write access.

```
static var any: CKSharingParticipantPermissionOption { get }
```

## [See Also](/documentation/cloudkit/cksharingparticipantpermissionoption/any#see-also)

### [Configuring the options](/documentation/cloudkit/cksharingparticipantpermissionoption/any#Configuring-the-options)

[`static var readOnly: CKSharingParticipantPermissionOption`](/documentation/cloudkit/cksharingparticipantpermissionoption/readonly)

The permission option the system uses to control whether a user can grant read-only access.

[`static var readWrite: CKSharingParticipantPermissionOption`](/documentation/cloudkit/cksharingparticipantpermissionoption/readwrite)

The permission option the system uses to control whether a user can grant write access.

Current page is any

# readOnly | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSharingParticipantPermissionOption](/documentation/cloudkit/cksharingparticipantpermissionoption)
-   readOnly

Type Property

# readOnly

The permission option the system uses to control whether a user can grant read-only access.

```
static var readOnly: CKSharingParticipantPermissionOption { get }
```

## [See Also](/documentation/cloudkit/cksharingparticipantpermissionoption/readonly#see-also)

### [Configuring the options](/documentation/cloudkit/cksharingparticipantpermissionoption/readonly#Configuring-the-options)

[`static var any: CKSharingParticipantPermissionOption`](/documentation/cloudkit/cksharingparticipantpermissionoption/any)

The permission option the system uses to control whether a user can grant read-only or write access.

[`static var readWrite: CKSharingParticipantPermissionOption`](/documentation/cloudkit/cksharingparticipantpermissionoption/readwrite)

The permission option the system uses to control whether a user can grant write access.

Current page is readOnly

# readWrite | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSharingParticipantPermissionOption](/documentation/cloudkit/cksharingparticipantpermissionoption)
-   readWrite

Type Property

# readWrite

The permission option the system uses to control whether a user can grant write access.

```
static var readWrite: CKSharingParticipantPermissionOption { get }
```

## [See Also](/documentation/cloudkit/cksharingparticipantpermissionoption/readwrite#see-also)

### [Configuring the options](/documentation/cloudkit/cksharingparticipantpermissionoption/readwrite#Configuring-the-options)

[`static var any: CKSharingParticipantPermissionOption`](/documentation/cloudkit/cksharingparticipantpermissionoption/any)

The permission option the system uses to control whether a user can grant read-only or write access.

[`static var readOnly: CKSharingParticipantPermissionOption`](/documentation/cloudkit/cksharingparticipantpermissionoption/readonly)

The permission option the system uses to control whether a user can grant read-only access.

Current page is readWrite

--- 
instance properties

# allowsAccessRequests | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKAllowedSharingOptions](/documentation/cloudkit/ckallowedsharingoptions)
-   allowsAccessRequests

Instance Property

# allowsAccessRequests

Default value is `NO`. If set, the system sharing UI will allow the user to configure whether access requests are enabled on the share.

```
var allowsAccessRequests: Bool { get set }
```

Current page is allowsAccessRequests

# allowsParticipantsToInviteOthers | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKAllowedSharingOptions](/documentation/cloudkit/ckallowedsharingoptions)
-   allowsParticipantsToInviteOthers

Instance Property

# allowsParticipantsToInviteOthers

Default value is `NO`. If set, the system sharing UI will allow the user to choose whether added participants can invite others to the share. Shares with [`CKShare.ParticipantRole.administrator`](/documentation/cloudkit/ckshare/participantrole/administrator) participants will be returned as read-only to devices running OS versions prior to this role being introduced. Administrator participants on these read-only shares will be returned as [`CKShare.ParticipantRole.privateUser`](/documentation/cloudkit/ckshare/participantrole/privateuser).

```
var allowsParticipantsToInviteOthers: Bool { get set }
```

Current page is allowsParticipantsToInviteOthers


