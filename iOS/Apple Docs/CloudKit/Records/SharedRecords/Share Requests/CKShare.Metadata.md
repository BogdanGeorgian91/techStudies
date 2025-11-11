# CKShare.Metadata

An object that describes a shared record’s metadata.

```
class Metadata
```

## [Overview](/documentation/cloudkit/ckshare/metadata#overview)

A share’s metadata is an intermediary object that provides access to the share, its owner, and, for a shared record hierarchy, its root record. Metadata also includes details about the current user’s participation in the share.

You don’t create metadata. CloudKit provides it to your app when the user taps or clicks a share’s [`url`](/documentation/cloudkit/ckshare/url), such as in an email or a message. The method CloudKit calls varies by platform and app configuration, and includes the following:

-   For a scene-based iOS app in a running or suspended state, CloudKit calls the [`windowScene(_:userDidAcceptCloudKitShareWith:)`](/documentation/UIKit/UIWindowSceneDelegate/windowScene\(_:userDidAcceptCloudKitShareWith:\)) method on your window scene delegate.
    
-   For a scene-based iOS app that’s not running, the system launches your app in response to the tap or click, and calls the [`scene(_:willConnectTo:options:)`](/documentation/UIKit/UISceneDelegate/scene\(_:willConnectTo:options:\)) method on your scene delegate. The `connectionOptions` parameter contains the metadata. Use its [`cloudKitShareMetadata`](/documentation/UIKit/UIScene/ConnectionOptions/cloudKitShareMetadata) property to access it.
    
-   For an iOS app that doesn’t use scenes, CloudKit calls your app delegate’s [`application(_:userDidAcceptCloudKitShareWith:)`](/documentation/UIKit/UIApplicationDelegate/application\(_:userDidAcceptCloudKitShareWith:\)) method.
    
-   For a macOS app, CloudKit calls your app delegate’s [`application(_:userDidAcceptCloudKitShareWith:)`](/documentation/AppKit/NSApplicationDelegate/application\(_:userDidAcceptCloudKitShareWith:\)) method.
    
-   For a watchOS app, CloudKit calls the [`userDidAcceptCloudKitShare(with:)`](/documentation/WatchKit/WKExtensionDelegate/userDidAcceptCloudKitShare\(with:\)) method on your watch extension delegate.
    

Respond by checking the [`participantStatus`](/documentation/cloudkit/ckshare/metadata/participantstatus) of the provided metadata. If the status is `pending`, use [`CKAcceptSharesOperation`](/documentation/cloudkit/ckacceptsharesoperation) to accept participation in the share. You can also fetch metadata independent of this flow using [`CKFetchShareMetadataOperation`](/documentation/cloudkit/ckfetchsharemetadataoperation).

For a shared record hierarchy, the [`hierarchicalRootRecordID`](/documentation/cloudkit/ckshare/metadata/hierarchicalrootrecordid) property contains the ID of the share’s root record. When using [`CKFetchShareMetadataOperation`](/documentation/cloudkit/ckfetchsharemetadataoperation) to fetch metadata, you can include the entire root record by setting the operation’s [`shouldFetchRootRecord`](/documentation/cloudkit/ckfetchsharemetadataoperation/shouldfetchrootrecord) property to [`true`](/documentation/swift/true). CloudKit then populates the [`rootRecord`](/documentation/cloudkit/ckshare/metadata/rootrecord) property before it returns the metadata. You can further customize this behavior using the operation’s [`rootRecordDesiredKeys`](/documentation/cloudkit/ckfetchsharemetadataoperation/rootrecorddesiredkeys-3xrex) property to specify which fields to return. This functionality isn’t applicable for a shared record zone because, unlike a shared record hierarchy, it doesn’t have a nominated root record.

The participant properties provide the current user’s acceptance status, permissions, and role. Use these values to determine what functionality to provide to the user. For example, only display editing controls for accepted participants with `readWrite` permissions.

## [Topics](/documentation/cloudkit/ckshare/metadata#topics)

### [Accessing the Share](/documentation/cloudkit/ckshare/metadata#Accessing-the-Share)

[`var share: CKShare`](/documentation/cloudkit/ckshare/metadata/share)

The share that owns the metadata.

[`var containerIdentifier: String`](/documentation/cloudkit/ckshare/metadata/containeridentifier)

The ID of the share’s container.

[`var ownerIdentity: CKUserIdentity`](/documentation/cloudkit/ckshare/metadata/owneridentity)

The identity of the share’s owner.

### [Accessing the Root Record](/documentation/cloudkit/ckshare/metadata#Accessing-the-Root-Record)

[`var hierarchicalRootRecordID: CKRecord.ID?`](/documentation/cloudkit/ckshare/metadata/hierarchicalrootrecordid)

The record ID of the shared hierarchy’s root record.

[`var rootRecord: CKRecord?`](/documentation/cloudkit/ckshare/metadata/rootrecord)

The share’s root record.

[`var rootRecordID: CKRecord.ID`](/documentation/cloudkit/ckshare/metadata/rootrecordid)

The record ID of the share’s root record.

Deprecated

### [Accessing the Participant’s Capabilities](/documentation/cloudkit/ckshare/metadata#Accessing-the-Participants-Capabilities)

[`var participantRole: CKShare.ParticipantRole`](/documentation/cloudkit/ckshare/metadata/participantrole)

The share’s participant role for the user who retrieves the metadata.

[`var participantPermission: CKShare.ParticipantPermission`](/documentation/cloudkit/ckshare/metadata/participantpermission)

The share’s permissions for the user who retrieves the metadata.

[`var participantStatus: CKShare.ParticipantAcceptanceStatus`](/documentation/cloudkit/ckshare/metadata/participantstatus)

The share’s participation status for the user who retrieves the metadata.

[`var participantType: CKShare.ParticipantType`](/documentation/cloudkit/ckshare/metadata/participanttype)

The share’s participation type for the user who retrieves the metadata.

Deprecated

## [Relationships](/documentation/cloudkit/ckshare/metadata#relationships)

### [Inherits From](/documentation/cloudkit/ckshare/metadata#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckshare/metadata#conforms-to)

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
  
Accessing the Share

# share | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Metadata](/documentation/cloudkit/ckshare/metadata)
-   share

Instance Property

# share

The share that owns the metadata.

```
@NSCopying
var share: CKShare { get }
```

## [See Also](/documentation/cloudkit/ckshare/metadata/share#see-also)

### [Accessing the Share](/documentation/cloudkit/ckshare/metadata/share#Accessing-the-Share)

[`var containerIdentifier: String`](/documentation/cloudkit/ckshare/metadata/containeridentifier)

The ID of the share’s container.

[`var ownerIdentity: CKUserIdentity`](/documentation/cloudkit/ckshare/metadata/owneridentity)

The identity of the share’s owner.

Current page is share

# containerIdentifier | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Metadata](/documentation/cloudkit/ckshare/metadata)
-   containerIdentifier

Instance Property

# containerIdentifier

The ID of the share’s container.

```
var containerIdentifier: String { get }
```

# ownerIdentity | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Metadata](/documentation/cloudkit/ckshare/metadata)
-   ownerIdentity

Instance Property

# ownerIdentity

The identity of the share’s owner.

```
@NSCopying
var ownerIdentity: CKUserIdentity { get }
```

---
  
Accessing the Root Record

# hierarchicalRootRecordID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Metadata](/documentation/cloudkit/ckshare/metadata)
-   hierarchicalRootRecordID

Instance Property

# hierarchicalRootRecordID

The record ID of the shared hierarchy’s root record.

```
@NSCopying
var hierarchicalRootRecordID: CKRecord.ID? { get }
```

## [Discussion](/documentation/cloudkit/ckshare/metadata/hierarchicalrootrecordid#Discussion)

CloudKit populates this property only for metadata that belongs to a shared record hierarchy. If the metadata is part of a shared record zone, the property is `nil`. This is because, unlike a shared record hierarchy, a shared record zone doesn’t have a nominated root record.

# rootRecord | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Metadata](/documentation/cloudkit/ckshare/metadata)
-   rootRecord

Instance Property

# rootRecord

The share’s root record.

```
@NSCopying
var rootRecord: CKRecord? { get }
```

## [Discussion](/documentation/cloudkit/ckshare/metadata/rootrecord#Discussion)

This property contains the root record of the shared record hierarchy if you set the [`shouldFetchRootRecord`](/documentation/cloudkit/ckfetchsharemetadataoperation/shouldfetchrootrecord) property of the operation that fetches the metadata to [`true`](/documentation/swift/true). You can specify which fields CloudKit returns by setting the same operation’s [`rootRecordDesiredKeys`](/documentation/cloudkit/ckfetchsharemetadataoperation/rootrecorddesiredkeys-3xrex) property.

The operation ignores the [`shouldFetchRootRecord`](/documentation/cloudkit/ckfetchsharemetadataoperation/shouldfetchrootrecord) and [`rootRecordDesiredKeys`](/documentation/cloudkit/ckfetchsharemetadataoperation/rootrecorddesiredkeys-3xrex) properties when fetching a shared record zone’s metadata because, unlike a shared record hierarchy, a record zone doesn’t have a nominated root record.

---
  
Accessing the Participant’s Capabilities

# participantRole | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Metadata](/documentation/cloudkit/ckshare/metadata)
-   participantRole

Instance Property

# participantRole

The share’s participant role for the user who retrieves the metadata.

```
var participantRole: CKShare.ParticipantRole { get }
```

# participantPermission | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Metadata](/documentation/cloudkit/ckshare/metadata)
-   participantPermission

Instance Property

# participantPermission

The share’s permissions for the user who retrieves the metadata.

```
var participantPermission: CKShare.ParticipantPermission { get }
```

# participantStatus | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Metadata](/documentation/cloudkit/ckshare/metadata)
-   participantStatus

Instance Property

# participantStatus

The share’s participation status for the user who retrieves the metadata.

```
var participantStatus: CKShare.ParticipantAcceptanceStatus { get }
```

# participantType | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.Metadata](/documentation/cloudkit/ckshare/metadata)
-   participantType

Instance Property

# participantType

The share’s participation type for the user who retrieves the metadata.

```
var participantType: CKShare.ParticipantType { get }
```
