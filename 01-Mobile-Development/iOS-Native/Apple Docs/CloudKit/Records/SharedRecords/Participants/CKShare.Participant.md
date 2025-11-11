# CKShare.Participant

An object that describes a user’s participation in a share.

```
class Participant
```

## [Overview](/documentation/cloudkit/ckshare/participant#overview)

Participants are a key element of sharing in CloudKit. A participant provides information about an iCloud user and their participation in a share, including their identity, acceptance status, permissions, and role.

The acceptance status determines the participant’s visibilty of the shared records. Statuses are: `pending`, `accepted`, `removed`, and `unknown`. If the status is `pending`, use [`CKAcceptSharesOperation`](/documentation/cloudkit/ckacceptsharesoperation) to accept the share. Upon acceptance, CloudKit makes the shared records available in the participant’s shared database. The records remain accessible for as long as the participant’s status is `accepted`.

You don’t create participants. Use the share’s [`participants`](/documentation/cloudkit/ckshare/participants) property to access its existing participants. Use [`UICloudSharingController`](/documentation/UIKit/UICloudSharingController) to manage the share’s participants and their permissions. Alternatively, you can generate participants using [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation). Participants must have an active iCloud account.

Anyone with the URL of a public share can become a participant in that share. Participants of a public share assume the `publicUser` role. For private shares, the owner manages the participants. An owner is any participant with the `owner` role. A participant of a private share can’t accept the share unless the owner adds them first. Private share participants assume the `privateUser` role. CloudKit removes any pending participants if the owner changes the share’s [`publicPermission`](/documentation/cloudkit/ckshare/publicpermission). CloudKit removes all participants if the new permission is `none`.

Participants with write permissions can modify or delete any record that you include in the share. However, only the owner can delete a shared hierarchy’s root record. If a participant attempts to delete the share, CloudKit removes the participant. The share remains active for all other participants.

## [Topics](/documentation/cloudkit/ckshare/participant#topics)

### [Accessing the Participant’s Status](/documentation/cloudkit/ckshare/participant#Accessing-the-Participants-Status)

[`var acceptanceStatus: CKShare.ParticipantAcceptanceStatus`](/documentation/cloudkit/ckshare/participant/acceptancestatus-swift.property)

The current state of the user’s acceptance of the share.

[`typealias AcceptanceStatus`](/documentation/cloudkit/ckshare/participant/acceptancestatus-swift.typealias)

A type that represents the acceptance status of the participant.

[`enum ParticipantAcceptanceStatus`](/documentation/cloudkit/ckshare/participantacceptancestatus)

Constants that represent the status of a participant.

### [Accessing the Participant’s Identity](/documentation/cloudkit/ckshare/participant#Accessing-the-Participants-Identity)

[`var userIdentity: CKUserIdentity`](/documentation/cloudkit/ckshare/participant/useridentity)

The identity of the participant.

### [Managing the Participant’s Capabilites](/documentation/cloudkit/ckshare/participant#Managing-the-Participants-Capabilites)

[`var permission: CKShare.ParticipantPermission`](/documentation/cloudkit/ckshare/participant/permission-swift.property)

The participant’s permission level for the share.

[`typealias Permission`](/documentation/cloudkit/ckshare/participant/permission-swift.typealias)

A type that represents the permissions for a participant.

[`enum ParticipantPermission`](/documentation/cloudkit/ckshare/participantpermission)

Constants that represent the permissions to grant to a share participant.

[`var role: CKShare.ParticipantRole`](/documentation/cloudkit/ckshare/participant/role-swift.property)

The participant’s role for the share.

[`typealias Role`](/documentation/cloudkit/ckshare/participant/role-swift.typealias)

A type that represents the role for a participant.

[`enum ParticipantRole`](/documentation/cloudkit/ckshare/participantrole)

Constants that represent the role of a share’s participant.

### [Deprecated](/documentation/cloudkit/ckshare/participant#Deprecated)

[

API Reference

Deprecated Symbols](/documentation/cloudkit/participant-deprecated-symbols)

Review unsupported symbols and their replacements.

### [Instance Properties](/documentation/cloudkit/ckshare/participant#Instance-Properties)

[`var dateAddedToShare: Date?`](/documentation/cloudkit/ckshare/participant/dateaddedtoshare)

The date and time when the participant was added to the share.

[`var isApprovedRequester: Bool`](/documentation/cloudkit/ckshare/participant/isapprovedrequester)

Indicates whether the participant was originally a requester who was approved to join the share.

[`var participantID: CKShare.Participant.ID`](/documentation/cloudkit/ckshare/participant/participantid)

### [Type Aliases](/documentation/cloudkit/ckshare/participant#Type-Aliases)

[`typealias ID`](/documentation/cloudkit/ckshare/participant/id)

### [Type Methods](/documentation/cloudkit/ckshare/participant#Type-Methods)

[`class func oneTimeURLParticipant() -> Self`](/documentation/cloudkit/ckshare/participant/onetimeurlparticipant\(\))

Generate a unique URL for inviting a participant without knowing their handle

## [Relationships](/documentation/cloudkit/ckshare/participant#relationships)

### [Inherits From](/documentation/cloudkit/ckshare/participant#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckshare/participant#conforms-to)

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

---
  
Accessing the Participant’s Status

# acceptanceStatus | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Participant](/documentation/cloudkit/ckshare/participant)
-   acceptanceStatus

Instance Property

# acceptanceStatus

The current state of the user’s acceptance of the share.

```
var acceptanceStatus: CKShare.ParticipantAcceptanceStatus { get }
```

## [Discussion](/documentation/cloudkit/ckshare/participant/acceptancestatus-swift.property#Discussion)

This property contains the current state of the participant’s acceptance of the share. For a list of possible values, see [`CKShare.ParticipantAcceptanceStatus`](/documentation/cloudkit/ckshare/participantacceptancestatus).

## [See Also](/documentation/cloudkit/ckshare/participant/acceptancestatus-swift.property#see-also)

### [Accessing the Participant’s Status](/documentation/cloudkit/ckshare/participant/acceptancestatus-swift.property#Accessing-the-Participants-Status)

[`typealias AcceptanceStatus`](/documentation/cloudkit/ckshare/participant/acceptancestatus-swift.typealias)

A type that represents the acceptance status of the participant.

[`enum ParticipantAcceptanceStatus`](/documentation/cloudkit/ckshare/participantacceptancestatus)

Constants that represent the status of a participant.

Current page is acceptanceStatus

# CKShare.Participant.AcceptanceStatus | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Participant](/documentation/cloudkit/ckshare/participant)
-   CKShare.Participant.AcceptanceStatus

Type Alias

# CKShare.Participant.AcceptanceStatus

A type that represents the acceptance status of the participant.

```
typealias AcceptanceStatus = CKShare.ParticipantAcceptanceStatus
```

# CKShare.ParticipantAcceptanceStatus | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   CKShare.ParticipantAcceptanceStatus

Enumeration

# CKShare.ParticipantAcceptanceStatus

Constants that represent the status of a participant.

```
enum ParticipantAcceptanceStatus
```

## [Topics](/documentation/cloudkit/ckshare/participantacceptancestatus#topics)

### [Constants](/documentation/cloudkit/ckshare/participantacceptancestatus#Constants)

[`case unknown`](/documentation/cloudkit/ckshare/participantacceptancestatus/unknown)

The participant’s status is unknown.

[`case pending`](/documentation/cloudkit/ckshare/participantacceptancestatus/pending)

The participant’s acceptance of the share request is pending.

[`case accepted`](/documentation/cloudkit/ckshare/participantacceptancestatus/accepted)

The participant accepted the share request.

[`case removed`](/documentation/cloudkit/ckshare/participantacceptancestatus/removed)

The system removed the participant from the share.

### [Initializers](/documentation/cloudkit/ckshare/participantacceptancestatus#Initializers)

[`init?(rawValue: Int)`](/documentation/cloudkit/ckshare/participantacceptancestatus/init\(rawvalue:\))

## [Relationships](/documentation/cloudkit/ckshare/participantacceptancestatus#relationships)

### [Conforms To](/documentation/cloudkit/ckshare/participantacceptancestatus#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

---
###  Accessing the Participant’s Identity

# userIdentity | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Participant](/documentation/cloudkit/ckshare/participant)
-   userIdentity

Instance Property

# userIdentity

The identity of the participant.

```
@NSCopying
var userIdentity: CKUserIdentity { get }
```

## [Discussion](/documentation/cloudkit/ckshare/participant/useridentity#Discussion)

This property contains a reference to the user identity for the share participant.

Current page is userIdentity

---
### Managing the Participant’s Capabilites

# permission | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Participant](/documentation/cloudkit/ckshare/participant)
-   permission

Instance Property

# permission

The participant’s permission level for the share.

```
var permission: CKShare.ParticipantPermission { get set }
```

## [Discussion](/documentation/cloudkit/ckshare/participant/permission-swift.property#Discussion)

This property controls the permissions that the participant has for the share. For a list of possible values, see [`CKShare.ParticipantPermission`](/documentation/cloudkit/ckshare/participantpermission).
# CKShare.Participant.Permission | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Participant](/documentation/cloudkit/ckshare/participant)
-   CKShare.Participant.Permission

Type Alias

# CKShare.Participant.Permission

A type that represents the permissions for a participant.

```
typealias Permission = CKShare.ParticipantPermission
```

## [See Also](/documentation/cloudkit/ckshare/participant/permission-swift.typealias#see-also)

### [Managing the Participant’s Capabilites](/documentation/cloudkit/ckshare/participant/permission-swift.typealias#Managing-the-Participants-Capabilites)

[`var permission: CKShare.ParticipantPermission`](/documentation/cloudkit/ckshare/participant/permission-swift.property)

The participant’s permission level for the share.

[`enum ParticipantPermission`](/documentation/cloudkit/ckshare/participantpermission)

Constants that represent the permissions to grant to a share participant.

[`var role: CKShare.ParticipantRole`](/documentation/cloudkit/ckshare/participant/role-swift.property)

The participant’s role for the share.

[`typealias Role`](/documentation/cloudkit/ckshare/participant/role-swift.typealias)

A type that represents the role for a participant.

[`enum ParticipantRole`](/documentation/cloudkit/ckshare/participantrole)

Constants that represent the role of a share’s participant.

Current page is CKShare.Participant.Permission

# CKShare.ParticipantPermission | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   CKShare.ParticipantPermission

Enumeration

# CKShare.ParticipantPermission

Constants that represent the permissions to grant to a share participant.

```
enum ParticipantPermission
```

## [Topics](/documentation/cloudkit/ckshare/participantpermission#topics)

### [Permissions](/documentation/cloudkit/ckshare/participantpermission#Permissions)

[`case none`](/documentation/cloudkit/ckshare/participantpermission/none)

The participant doesn’t have any permissions for the share.

[`case readOnly`](/documentation/cloudkit/ckshare/participantpermission/readonly)

The participant has read-only permissions for the share.

[`case readWrite`](/documentation/cloudkit/ckshare/participantpermission/readwrite)

The participant has read-and-write permissions for the share.

[`case unknown`](/documentation/cloudkit/ckshare/participantpermission/unknown)

The participant’s permissions are unknown.

### [Initializers](/documentation/cloudkit/ckshare/participantpermission#Initializers)

[`init?(rawValue: Int)`](/documentation/cloudkit/ckshare/participantpermission/init\(rawvalue:\))

## [Relationships](/documentation/cloudkit/ckshare/participantpermission#relationships)

### [Conforms To](/documentation/cloudkit/ckshare/participantpermission#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)
# role | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Participant](/documentation/cloudkit/ckshare/participant)
-   role

Instance Property

# role

The participant’s role for the share.

```
var role: CKShare.ParticipantRole { get set }
```
# CKShare.Participant.Role | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Participant](/documentation/cloudkit/ckshare/participant)
-   CKShare.Participant.Role

Type Alias

# CKShare.Participant.Role

A type that represents the role for a participant.

```
typealias Role = CKShare.ParticipantRole
```

# CKShare.ParticipantRole | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   CKShare.ParticipantRole

Enumeration

# CKShare.ParticipantRole

Constants that represent the role of a share’s participant.

```
enum ParticipantRole
```

## [Topics](/documentation/cloudkit/ckshare/participantrole#topics)

### [Roles](/documentation/cloudkit/ckshare/participantrole#Roles)

[`case owner`](/documentation/cloudkit/ckshare/participantrole/owner)

The participant is the share’s owner.

[`case privateUser`](/documentation/cloudkit/ckshare/participantrole/privateuser)

The participant has the private role.

[`case publicUser`](/documentation/cloudkit/ckshare/participantrole/publicuser)

The participant has the public role.

[`case unknown`](/documentation/cloudkit/ckshare/participantrole/unknown)

The participant’s role is unknown.

### [Enumeration Cases](/documentation/cloudkit/ckshare/participantrole#Enumeration-Cases)

[`case administrator`](/documentation/cloudkit/ckshare/participantrole/administrator)

### [Initializers](/documentation/cloudkit/ckshare/participantrole#Initializers)

[`init?(rawValue: Int)`](/documentation/cloudkit/ckshare/participantrole/init\(rawvalue:\))

## [Relationships](/documentation/cloudkit/ckshare/participantrole#relationships)

### [Conforms To](/documentation/cloudkit/ckshare/participantrole#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)
# CKShare.ParticipantRole.administrator | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.ParticipantRole](/documentation/cloudkit/ckshare/participantrole)
-   CKShare.ParticipantRole.administrator

Case

# CKShare.ParticipantRole.administrator

```
case administrator
```

Current page is CKShare.ParticipantRole.administrator

---
instance properties

# dateAddedToShare | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Participant](/documentation/cloudkit/ckshare/participant)
-   dateAddedToShare

Instance Property

# dateAddedToShare

The date and time when the participant was added to the share.

```
var dateAddedToShare: Date? { get }
```

## [Discussion](/documentation/cloudkit/ckshare/participant/dateaddedtoshare#discussion)

This timestamp is set when the share is successfully saved to the server.

Current page is dateAddedToShare

# isApprovedRequester | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Participant](/documentation/cloudkit/ckshare/participant)
-   isApprovedRequester

Instance Property

# isApprovedRequester

Indicates whether the participant was originally a requester who was approved to join the share.

```
var isApprovedRequester: Bool { get }
```

Current page is isApprovedRequester

# participantID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Participant](/documentation/cloudkit/ckshare/participant)
-   participantID

Instance Property

# participantID

```
@backDeployed(before: macOS 15.0, iOS 18.0, tvOS 18.0, watchOS 11.0, visionOS 2.0)
final var participantID: CKShare.Participant.ID { get }
```

Current page is participantID

# CKShare.Participant.ID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Participant](/documentation/cloudkit/ckshare/participant)
-   CKShare.Participant.ID

Type Alias

# CKShare.Participant.ID

```
typealias ID = String
```

Current page is CKShare.Participant.ID

# oneTimeURLParticipant() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Participant](/documentation/cloudkit/ckshare/participant)
-   oneTimeURLParticipant()

Type Method

# oneTimeURLParticipant()

Generate a unique URL for inviting a participant without knowing their handle

```
class func oneTimeURLParticipant() -> Self
```

## [Discussion](/documentation/cloudkit/ckshare/participant/onetimeurlparticipant\(\)#discussion)

When a participant’s email address / phone number / userRecordID isn’t known up-front, a [`oneTimeURLParticipant()`](/documentation/cloudkit/ckshare/participant/onetimeurlparticipant\(\)) can be added to the share. Once the share is saved, a custom invitation link or one-time URL is available for the added participant via [`oneTimeURLForParticipantID:`](/documentation/cloudkit/ckshare/onetimeurlforparticipantid:). This custom link can be used by any recipient user to fetch share metadata and accept the share.

Note that a one-time URL participant in the [`CKShare.ParticipantAcceptanceStatus.pending`](/documentation/cloudkit/ckshare/participantacceptancestatus/pending) state has empty [`nameComponents`](/documentation/cloudkit/ckuseridentity/namecomponents) and a nil [`lookupInfo`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.property).

Current page is oneTimeURLParticipant()