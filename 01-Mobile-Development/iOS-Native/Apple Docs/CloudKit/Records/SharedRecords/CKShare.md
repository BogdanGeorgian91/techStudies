# CKShare | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKShare

Class

# CKShare

A specialized record type that manages a collection of shared records.

```
class CKShare
```

## [Overview](/documentation/cloudkit/ckshare#overview)

A share is a specialized record type that facilitates the sharing of one or more records with many participants. You store shareable records in a custom record zone in the user’s private database. As you create records in that zone, they become eligible for record zone sharing. If you want to share a specific hierarchy of related records, rather than the entire record zone, set each record’s [`parent`](/documentation/cloudkit/ckrecord/parent) property to define the relationship with its parent. CloudKit infers the shared hierarchy using only the [`parent`](/documentation/cloudkit/ckrecord/parent) property, and ignores any custom reference fields.

You create a share with either the ID of the record zone to share, or the root record, which defines the point in a record hierarchy where you want to start sharing. CloudKit shares all the records in the record zone, or every record in the hierarchy below the root. If you set the root record’s [`parent`](/documentation/cloudkit/ckrecord/parent) property, CloudKit ignores it. A record can take part in only a single share. This applies to every record in the shared record zone or hierarchy. If a record is participating in another share, any attempt to save the share fails, and CloudKit returns an [`alreadyShared`](/documentation/cloudkit/ckerror/alreadyshared) error.

Use [`CKModifyRecordsOperation`](/documentation/cloudkit/ckmodifyrecordsoperation) to save the share to the server. The initial set of records the share includes must exist on the server or be part of the same save operation to succeed. CloudKit then updates the share’s [`url`](/documentation/cloudkit/ckshare/url) property. Use [`UICloudSharingController`](/documentation/UIKit/UICloudSharingController) to present options to the user for sharing the URL. Otherwise, distribute the URL to any participants you add to the share. You can allow anyone with the URL to take part in the share by setting [`publicPermission`](/documentation/cloudkit/ckshare/publicpermission) to a value more permissive than [`CKShare.ParticipantPermission.none`](/documentation/cloudkit/ckshare/participantpermission/none).

After CloudKit saves the share, a participant can fetch its corresponding metadata, which includes a reference to the share, information about the user’s participation, and, for shared hierarchies, the root record’s record ID. Create an instance of [`CKFetchShareMetadataOperation`](/documentation/cloudkit/ckfetchsharemetadataoperation) using the share’s URL and add it to the container’s queue to execute it. The operation returns an instance of [`CKShare.Metadata`](/documentation/cloudkit/ckshare/metadata) for each URL you provide. This is only applicable if you manually process share acceptance. If a user receives the share URL and taps or clicks it, CloudKit automatically processes their participation.

To determine the configuration of a fetched share, inspect the [`recordName`](/documentation/cloudkit/ckrecord/id/recordname) property of its [`recordID`](/documentation/cloudkit/ckrecord/recordid). If the value is [`CKRecordNameZoneWideShare`](/documentation/cloudkit/ckrecordnamezonewideshare), the share is managing a shared record zone; otherwise, it’s managing a shared record hierarchy.

```
let isZoneWide = (metadata.share.recordID.recordName == CKRecordNameZoneWideShare)
```

CloudKit limits the number of participants in a share to 100, and each participant must have an active iCloud account. You don’t create participants. Instead, use [`UICloudSharingController`](/documentation/UIKit/UICloudSharingController) to manage a share’s participants and their permissions. Alternatively, create an instance of [`CKUserIdentity.LookupInfo`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class) for each user. Provide the user’s email address or phone number, and use [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) to fetch the corresponding participants. CloudKit queries iCloud for corresponding accounts as part of the operation. If it doesn’t find an account, the server updates the participant’s [`userIdentity`](/documentation/cloudkit/ckshare/participant/useridentity) to reflect that by setting the [`hasiCloudAccount`](/documentation/cloudkit/ckuseridentity/hasicloudaccount) property to [`false`](/documentation/swift/false). CloudKit associates the participant with their iCloud account when they accept the share if they launch the process by tapping or clicking the share URL.

Participants with write permissions can modify or delete any record that you include in the share. However, only the owner can delete a shared hierarchy’s root record. If a participant attempts to delete the share, CloudKit removes the participant. The share remains active for all other participants. If the owner deletes a share that manages a record hierarchy, CloudKit sets the root record’s [`share`](/documentation/cloudkit/ckrecord/share) property to `nil`. CloudKit deletes the share if the owner of the shared heirarchy deletes its root record.

You can customize the title and image the system displays when initiating a share or accepting an invitation to participate. You can also provide a custom UTI to indicate the content of the shared records. Use the keys that [`CKShare.SystemFieldKey`](/documentation/cloudkit/ckshare/systemfieldkey) defines, as the following example shows:

```
let share = CKShare(rootRecord: album)


// Configure the share so the system displays the album's 
// name and cover when the user initiates sharing or accepts 
// an invitation to participate.
share[CKShare.SystemFieldKey.title] = album["name"]
if let cover = album["cover"] as? UIImage, let data = cover.pngData() {
    share[CKShare.SystemFieldKey.thumbnailImageData] = data
}
// Include a custom UTI that describes the share's content.
share[CKShare.SystemFieldKey.shareType] = "com.example.app.album"
```

## [Topics](/documentation/cloudkit/ckshare#topics)

### [Creating a Share](/documentation/cloudkit/ckshare#Creating-a-Share)

[`init(coder: NSCoder)`](/documentation/cloudkit/ckshare/init\(coder:\))

Creates a share from a serialized instance.

[`convenience init(rootRecord: CKRecord)`](/documentation/cloudkit/ckshare/init\(rootrecord:\))

Creates a new share for the specified record.

[`init(rootRecord: CKRecord, shareID: CKRecord.ID)`](/documentation/cloudkit/ckshare/init\(rootrecord:shareid:\))

Creates a new share for the specified record and record ID.

[`init(recordZoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckshare/init\(recordzoneid:\))

Creates a new share for the specified record zone.

### [Accessing the Share’s Attributes](/documentation/cloudkit/ckshare#Accessing-the-Shares-Attributes)

[`var owner: CKShare.Participant`](/documentation/cloudkit/ckshare/owner)

The participant that represents the share’s owner.

[`var currentUserParticipant: CKShare.Participant?`](/documentation/cloudkit/ckshare/currentuserparticipant)

The participant that represents the current user.

[`var participants: [CKShare.Participant]`](/documentation/cloudkit/ckshare/participants)

An array that contains the share’s participants.

[`var url: URL?`](/documentation/cloudkit/ckshare/url)

The URL for inviting participants to the share.

### [Configuring the Share](/documentation/cloudkit/ckshare#Configuring-the-Share)

[`var publicPermission: CKShare.ParticipantPermission`](/documentation/cloudkit/ckshare/publicpermission)

The permission for anyone with access to the share’s URL.

[`func addParticipant(CKShare.Participant)`](/documentation/cloudkit/ckshare/addparticipant\(_:\))

Adds a participant to the share.

[`func removeParticipant(CKShare.Participant)`](/documentation/cloudkit/ckshare/removeparticipant\(_:\))

Removes a participant from the share.

[`class Participant`](/documentation/cloudkit/ckshare/participant)

An object that describes a user’s participation in a share.

### [Accessing Metadata](/documentation/cloudkit/ckshare#Accessing-Metadata)

[`class Metadata`](/documentation/cloudkit/ckshare/metadata)

An object that describes a shared record’s metadata.

### [Subscripting](/documentation/cloudkit/ckshare#Subscripting)

[`enum SystemFieldKey`](/documentation/cloudkit/ckshare/systemfieldkey)

Constants that represent the system fields of a share.

### [Classes](/documentation/cloudkit/ckshare#Classes)

[`class AccessRequester`](/documentation/cloudkit/ckshare/accessrequester)

[`class BlockedIdentity`](/documentation/cloudkit/ckshare/blockedidentity)

### [Instance Properties](/documentation/cloudkit/ckshare#Instance-Properties)

[`var allowsAccessRequests: Bool`](/documentation/cloudkit/ckshare/allowsaccessrequests)

Indicates whether uninvited users can request access to this share.

[`var blockedIdentities: [CKShare.BlockedIdentity]`](/documentation/cloudkit/ckshare/blockedidentities)

A list of users blocked from requesting access to this share.

[`var requesters: [CKShare.AccessRequester]`](/documentation/cloudkit/ckshare/requesters)

A list of all uninvited users who have requested access to this share.

### [Instance Methods](/documentation/cloudkit/ckshare#Instance-Methods)

[`func blockRequesters([CKShare.AccessRequester])`](/documentation/cloudkit/ckshare/blockrequesters\(_:\))

Blocks specified users from requesting access to this share.

[`func denyRequesters([CKShare.AccessRequester])`](/documentation/cloudkit/ckshare/denyrequesters\(_:\))

Denies access requests from specified users.

[`func oneTimeURL(for: CKShare.Participant.ID) -> URL?`](/documentation/cloudkit/ckshare/onetimeurl\(for:\))

[`func unblockIdentities([CKShare.BlockedIdentity])`](/documentation/cloudkit/ckshare/unblockidentities\(_:\))

Unblocks previously blocked users, allowing them to request access again.

## [Relationships](/documentation/cloudkit/ckshare#relationships)

### [Inherits From](/documentation/cloudkit/ckshare#inherits-from)

-   [`CKRecord`](/documentation/cloudkit/ckrecord)

### [Conforms To](/documentation/cloudkit/ckshare#conforms-to)

-   [`CKRecordKeyValueSetting`](/documentation/cloudkit/ckrecordkeyvaluesetting)
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
-   [`Sequence`](/documentation/Swift/Sequence)

## [See Also](/documentation/cloudkit/ckshare#see-also)

### [Collaboration](/documentation/cloudkit/ckshare#Collaboration)

[

Sharing CloudKit Data with Other iCloud Users](/documentation/cloudkit/sharing-cloudkit-data-with-other-icloud-users)

Create and share private CloudKit data with other users by implementing the sharing UI.

[

Sharing Core Data objects between iCloud users](/documentation/CoreData/sharing-core-data-objects-between-icloud-users)

Use Core Data and CloudKit to synchronize data between devices of an iCloud user and share data between different iCloud users.

[`struct CKShareTransferRepresentation`](/documentation/cloudkit/cksharetransferrepresentation)

A transfer representation the system uses to share an item.

[`class CKAllowedSharingOptions`](documentation/cloudkit/CKAllowedSharingOptions.md)

An object that controls participant access and permission options.

[`class CKSystemSharingUIObserver`](/documentation/cloudkit/cksystemsharinguiobserver)

An object the system uses to monitor changes in sharing.

[`@MainActor class UICloudSharingController`](/documentation/UIKit/UICloudSharingController)

A view controller that presents standard screens for adding and removing people from a CloudKit share record.

[`CKSharingSupported`](/documentation/BundleResources/Information-Property-List/CKSharingSupported)

A Boolean value that indicates your app supports CloudKit Sharing.

Current page is CKShare

---
### Creating a Share

# init(coder:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   init(coder:)

Initializer

# init(coder:)

Creates a share from a serialized instance.

```
init(coder aDecoder: NSCoder)
```

## [Parameters](/documentation/cloudkit/ckshare/init\(coder:\)#parameters)

`aDecoder`

The coder to use when deserializing the share.

## [Discussion](/documentation/cloudkit/ckshare/init\(coder:\)#Discussion)

When saving a newly created [`CKShare`](/documentation/cloudkit/ckshare), you must save the share and its [`rootRecord`](/documentation/cloudkit/ckshare/metadata/rootrecord) in the same [`CKModifyRecordsOperation`](/documentation/cloudkit/ckmodifyrecordsoperation) batch.

## [See Also](/documentation/cloudkit/ckshare/init\(coder:\)#see-also)

### [Creating a Share](/documentation/cloudkit/ckshare/init\(coder:\)#Creating-a-Share)

[`convenience init(rootRecord: CKRecord)`](/documentation/cloudkit/ckshare/init\(rootrecord:\))

Creates a new share for the specified record.

[`init(rootRecord: CKRecord, shareID: CKRecord.ID)`](/documentation/cloudkit/ckshare/init\(rootrecord:shareid:\))

Creates a new share for the specified record and record ID.

[`init(recordZoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckshare/init\(recordzoneid:\))

Creates a new share for the specified record zone.

Current page is init(coder:)

# init(rootRecord:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   init(rootRecord:)

Initializer

# init(rootRecord:)

Creates a new share for the specified record.

```
convenience init(rootRecord: CKRecord)
```

## [Parameters](/documentation/cloudkit/ckshare/init\(rootrecord:\)#parameters)

`rootRecord`

The record to share.

## [Discussion](/documentation/cloudkit/ckshare/init\(rootrecord:\)#Discussion)

When saving a newly created [`CKShare`](/documentation/cloudkit/ckshare), you must save the share and its [`rootRecord`](/documentation/cloudkit/ckshare/metadata/rootrecord) in the same [`CKModifyRecordsOperation`](/documentation/cloudkit/ckmodifyrecordsoperation) batch.

## [See Also](/documentation/cloudkit/ckshare/init\(rootrecord:\)#see-also)

### [Creating a Share](/documentation/cloudkit/ckshare/init\(rootrecord:\)#Creating-a-Share)

[`init(coder: NSCoder)`](/documentation/cloudkit/ckshare/init\(coder:\))

Creates a share from a serialized instance.

[`init(rootRecord: CKRecord, shareID: CKRecord.ID)`](/documentation/cloudkit/ckshare/init\(rootrecord:shareid:\))

Creates a new share for the specified record and record ID.

[`init(recordZoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckshare/init\(recordzoneid:\))

Creates a new share for the specified record zone.

Current page is init(rootRecord:)

# init(rootRecord:shareID:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   init(rootRecord:shareID:)

Initializer

# init(rootRecord:shareID:)

Creates a new share for the specified record and record ID.

```
init(
    rootRecord: CKRecord,
    shareID: CKRecord.ID
)
```

## [Parameters](/documentation/cloudkit/ckshare/init\(rootrecord:shareid:\)#parameters)

`rootRecord`

The record to share.

`shareID`

The [`CKRecord.ID`](/documentation/cloudkit/ckrecord/id) for the share.

## [Discussion](/documentation/cloudkit/ckshare/init\(rootrecord:shareid:\)#Discussion)

When saving a newly created [`CKShare`](/documentation/cloudkit/ckshare), save the share and its [`rootRecord`](/documentation/cloudkit/ckshare/metadata/rootrecord) in the same [`CKModifyRecordsOperation`](/documentation/cloudkit/ckmodifyrecordsoperation) batch.

## [See Also](/documentation/cloudkit/ckshare/init\(rootrecord:shareid:\)#see-also)

### [Creating a Share](/documentation/cloudkit/ckshare/init\(rootrecord:shareid:\)#Creating-a-Share)

[`init(coder: NSCoder)`](/documentation/cloudkit/ckshare/init\(coder:\))

Creates a share from a serialized instance.

[`convenience init(rootRecord: CKRecord)`](/documentation/cloudkit/ckshare/init\(rootrecord:\))

Creates a new share for the specified record.

[`init(recordZoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckshare/init\(recordzoneid:\))

Creates a new share for the specified record zone.

Current page is init(rootRecord:shareID:)

# init(recordZoneID:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   init(recordZoneID:)

Initializer

# init(recordZoneID:)

Creates a new share for the specified record zone.

```
init(recordZoneID: CKRecordZone.ID)
```

## [Parameters](/documentation/cloudkit/ckshare/init\(recordzoneid:\)#parameters)

`recordZoneID`

The ID of the record zone to share.

## [Discussion](/documentation/cloudkit/ckshare/init\(recordzoneid:\)#Discussion)

A shared record zone must have the [`zoneWideSharing`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/zonewidesharing) capability. Custom record zones that you create in the user’s private database have this capability by default. A record zone, and the records it contains, can take part in only a single share.

After accepting a share invite, CloudKit adds the records of the shared record zone to a new zone in the participant’s shared database. Use [`CKFetchDatabaseChangesOperation`](/documentation/cloudkit/ckfetchdatabasechangesoperation) to fetch the ID of the new record zone. Then configure [`CKFetchRecordZoneChangesOperation`](/documentation/cloudkit/ckfetchrecordzonechangesoperation) with that record zone ID and execute the operation to fetch the records.

If you use [`CKFetchShareMetadataOperation`](/documentation/cloudkit/ckfetchsharemetadataoperation) to fetch the metadata for a shared record zone, the operation ignores the [`shouldFetchRootRecord`](/documentation/cloudkit/ckfetchsharemetadataoperation/shouldfetchrootrecord) and [`rootRecordDesiredKeys`](/documentation/cloudkit/ckfetchsharemetadataoperation/rootrecorddesiredkeys-3xrex) properties because, unlike a shared record hierarchy, a record zone doesn’t have a nominated root record.

## [See Also](/documentation/cloudkit/ckshare/init\(recordzoneid:\)#see-also)

### [Creating a Share](/documentation/cloudkit/ckshare/init\(recordzoneid:\)#Creating-a-Share)

[`init(coder: NSCoder)`](/documentation/cloudkit/ckshare/init\(coder:\))

Creates a share from a serialized instance.

[`convenience init(rootRecord: CKRecord)`](/documentation/cloudkit/ckshare/init\(rootrecord:\))

Creates a new share for the specified record.

[`init(rootRecord: CKRecord, shareID: CKRecord.ID)`](/documentation/cloudkit/ckshare/init\(rootrecord:shareid:\))

Creates a new share for the specified record and record ID.

Current page is init(recordZoneID:)

---

### Accessing the Share’s Attributes
# owner | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   owner

Instance Property

# owner

The participant that represents the share’s owner.

```
@NSCopying
var owner: CKShare.Participant { get }
```

## [See Also](/documentation/cloudkit/ckshare/owner#see-also)

### [Accessing the Share’s Attributes](/documentation/cloudkit/ckshare/owner#Accessing-the-Shares-Attributes)

[`var currentUserParticipant: CKShare.Participant?`](/documentation/cloudkit/ckshare/currentuserparticipant)

The participant that represents the current user.

[`var participants: [CKShare.Participant]`](/documentation/cloudkit/ckshare/participants)

An array that contains the share’s participants.

[`var url: URL?`](/documentation/cloudkit/ckshare/url)

The URL for inviting participants to the share.

Current page is owner

# currentUserParticipant | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   currentUserParticipant

Instance Property

# currentUserParticipant

The participant that represents the current user.

```
@NSCopying
var currentUserParticipant: CKShare.Participant? { get }
```

## [See Also](/documentation/cloudkit/ckshare/currentuserparticipant#see-also)

### [Accessing the Share’s Attributes](/documentation/cloudkit/ckshare/currentuserparticipant#Accessing-the-Shares-Attributes)

[`var owner: CKShare.Participant`](/documentation/cloudkit/ckshare/owner)

The participant that represents the share’s owner.

[`var participants: [CKShare.Participant]`](/documentation/cloudkit/ckshare/participants)

An array that contains the share’s participants.

[`var url: URL?`](/documentation/cloudkit/ckshare/url)

The URL for inviting participants to the share.

Current page is currentUserParticipant

# participants | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   participants

Instance Property

# participants

An array that contains the share’s participants.

```
var participants: [CKShare.Participant] { get }
```

## [Discussion](/documentation/cloudkit/ckshare/participants#Discussion)

The property’s value contains all of the share’s participants that the current user has permissions to see. At a minimum, it includes the share’s owner and the current user.

## [See Also](/documentation/cloudkit/ckshare/participants#see-also)

### [Accessing the Share’s Attributes](/documentation/cloudkit/ckshare/participants#Accessing-the-Shares-Attributes)

[`var owner: CKShare.Participant`](/documentation/cloudkit/ckshare/owner)

The participant that represents the share’s owner.

[`var currentUserParticipant: CKShare.Participant?`](/documentation/cloudkit/ckshare/currentuserparticipant)

The participant that represents the current user.

[`var url: URL?`](/documentation/cloudkit/ckshare/url)

The URL for inviting participants to the share.

Current page is participants

# url | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   url

Instance Property

# url

The URL for inviting participants to the share.

```
var url: URL? { get }
```

## [Discussion](/documentation/cloudkit/ckshare/url#Discussion)

This property is only available after saving a share record to the server. This URL is stable and persists across shares and reshares of the same root record.

## [See Also](/documentation/cloudkit/ckshare/url#see-also)

### [Accessing the Share’s Attributes](/documentation/cloudkit/ckshare/url#Accessing-the-Shares-Attributes)

[`var owner: CKShare.Participant`](/documentation/cloudkit/ckshare/owner)

The participant that represents the share’s owner.

[`var currentUserParticipant: CKShare.Participant?`](/documentation/cloudkit/ckshare/currentuserparticipant)

The participant that represents the current user.

[`var participants: [CKShare.Participant]`](/documentation/cloudkit/ckshare/participants)

An array that contains the share’s participants.

Current page is url

---
  
Configuring the Share

# publicPermission | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   publicPermission

Instance Property

# publicPermission

The permission for anyone with access to the share’s URL.

```
var publicPermission: CKShare.ParticipantPermission { get set }
```

## [Discussion](/documentation/cloudkit/ckshare/publicpermission#Discussion)

Setting this property’s value to be more permissive than [`CKShare.ParticipantPermission.none`](/documentation/cloudkit/ckshare/participantpermission/none) allows any user with the share’s URL to join. CloudKit removes all public participants when you save the share if you set the property’s value to [`CKShare.ParticipantPermission.none`](/documentation/cloudkit/ckshare/participantpermission/none).

The default value is [`CKShare.ParticipantPermission.none`](/documentation/cloudkit/ckshare/participantpermission/none)

## [See Also](/documentation/cloudkit/ckshare/publicpermission#see-also)

### [Configuring the Share](/documentation/cloudkit/ckshare/publicpermission#Configuring-the-Share)

[`func addParticipant(CKShare.Participant)`](/documentation/cloudkit/ckshare/addparticipant\(_:\))

Adds a participant to the share.

[`func removeParticipant(CKShare.Participant)`](/documentation/cloudkit/ckshare/removeparticipant\(_:\))

Removes a participant from the share.

[`class Participant`](/documentation/cloudkit/ckshare/participant)

An object that describes a user’s participation in a share.

Current page is publicPermission

# addParticipant(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   addParticipant(\_:)

Instance Method

# addParticipant(\_:)

Adds a participant to the share.

```
func addParticipant(_ participant: CKShare.Participant)
```

## [Parameters](/documentation/cloudkit/ckshare/addparticipant\(_:\)#parameters)

`participant`

The participant to add to the share.

## [Discussion](/documentation/cloudkit/ckshare/addparticipant\(_:\)#Discussion)

If a participant with a matching [`userIdentity`](/documentation/cloudkit/ckshare/participant/useridentity) already exists in the share, the system updates the existing participant’s properties and doesn’t add a new participant.

To modify the list of participants, a share’s [`publicPermission`](/documentation/cloudkit/ckshare/publicpermission) must be [`CKShare.ParticipantPermission.none`](/documentation/cloudkit/ckshare/participantpermission/none). You can’t mix and match public and private users in the same share. You can only add certain participant types with this API. See [`CKShare.Participant`](/documentation/cloudkit/ckshare/participant) for more information.

## [See Also](/documentation/cloudkit/ckshare/addparticipant\(_:\)#see-also)

### [Configuring the Share](/documentation/cloudkit/ckshare/addparticipant\(_:\)#Configuring-the-Share)

[`var publicPermission: CKShare.ParticipantPermission`](/documentation/cloudkit/ckshare/publicpermission)

The permission for anyone with access to the share’s URL.

[`func removeParticipant(CKShare.Participant)`](/documentation/cloudkit/ckshare/removeparticipant\(_:\))

Removes a participant from the share.

[`class Participant`](/documentation/cloudkit/ckshare/participant)

An object that describes a user’s participation in a share.

Current page is addParticipant(\_:)

# removeParticipant(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   removeParticipant(\_:)

Instance Method

# removeParticipant(\_:)

Removes a participant from the share.

```
func removeParticipant(_ participant: CKShare.Participant)
```

## [Parameters](/documentation/cloudkit/ckshare/removeparticipant\(_:\)#parameters)

`participant`

The participant to remove from the share.

## [Discussion](/documentation/cloudkit/ckshare/removeparticipant\(_:\)#Discussion)

To modify the list of participants, a share’s [`publicPermission`](/documentation/cloudkit/ckshare/publicpermission) must be [`CKShare.ParticipantPermission.none`](/documentation/cloudkit/ckshare/participantpermission/none). You can’t mix and match public and private users in the same share. You can only add certain participant types with this API. See [`CKShare.Participant`](/documentation/cloudkit/ckshare/participant) for more information.

## [See Also](/documentation/cloudkit/ckshare/removeparticipant\(_:\)#see-also)

### [Configuring the Share](/documentation/cloudkit/ckshare/removeparticipant\(_:\)#Configuring-the-Share)

[`var publicPermission: CKShare.ParticipantPermission`](/documentation/cloudkit/ckshare/publicpermission)

The permission for anyone with access to the share’s URL.

[`func addParticipant(CKShare.Participant)`](/documentation/cloudkit/ckshare/addparticipant\(_:\))

Adds a participant to the share.

[`class Participant`](/documentation/cloudkit/ckshare/participant)

An object that describes a user’s participation in a share.

Current page is removeParticipant(\_:)

---
# CKShare.Participant | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   CKShare.Participant

Class

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

## [See Also](/documentation/cloudkit/ckshare/participant#see-also)

### [Participants](/documentation/cloudkit/ckshare/participant#Participants)

[`class CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation)

An operation that converts user identities into share participants.

Current page is CKShare.Participant

### Accessing the Participant’s Status

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

## [See Also](/documentation/cloudkit/ckshare/participant/acceptancestatus-swift.typealias#see-also)

### [Accessing the Participant’s Status](/documentation/cloudkit/ckshare/participant/acceptancestatus-swift.typealias#Accessing-the-Participants-Status)

[`var acceptanceStatus: CKShare.ParticipantAcceptanceStatus`](/documentation/cloudkit/ckshare/participant/acceptancestatus-swift.property)

The current state of the user’s acceptance of the share.

[`enum ParticipantAcceptanceStatus`](/documentation/cloudkit/ckshare/participantacceptancestatus)

Constants that represent the status of a participant.

Current page is CKShare.Participant.AcceptanceStatus

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

## [See Also](/documentation/cloudkit/ckshare/participantacceptancestatus#see-also)

### [Accessing the Participant’s Status](/documentation/cloudkit/ckshare/participantacceptancestatus#Accessing-the-Participants-Status)

[`var acceptanceStatus: CKShare.ParticipantAcceptanceStatus`](/documentation/cloudkit/ckshare/participant/acceptancestatus-swift.property)

The current state of the user’s acceptance of the share.

[`typealias AcceptanceStatus`](/documentation/cloudkit/ckshare/participant/acceptancestatus-swift.typealias)

A type that represents the acceptance status of the participant.

Current page is CKShare.ParticipantAcceptanceStatus

---
### Accessing the Participant’s Identity

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
  
Managing the Participant’s Capabilites

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

## [See Also](/documentation/cloudkit/ckshare/participant/permission-swift.property#see-also)

### [Managing the Participant’s Capabilites](/documentation/cloudkit/ckshare/participant/permission-swift.property#Managing-the-Participants-Capabilites)

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

Current page is permission

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

## [See Also](/documentation/cloudkit/ckshare/participantpermission#see-also)

### [Managing the Participant’s Capabilites](/documentation/cloudkit/ckshare/participantpermission#Managing-the-Participants-Capabilites)

[`var permission: CKShare.ParticipantPermission`](/documentation/cloudkit/ckshare/participant/permission-swift.property)

The participant’s permission level for the share.

[`typealias Permission`](/documentation/cloudkit/ckshare/participant/permission-swift.typealias)

A type that represents the permissions for a participant.

[`var role: CKShare.ParticipantRole`](/documentation/cloudkit/ckshare/participant/role-swift.property)

The participant’s role for the share.

[`typealias Role`](/documentation/cloudkit/ckshare/participant/role-swift.typealias)

A type that represents the role for a participant.

[`enum ParticipantRole`](/documentation/cloudkit/ckshare/participantrole)

Constants that represent the role of a share’s participant.

Current page is CKShare.ParticipantPermission

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

## [See Also](/documentation/cloudkit/ckshare/participant/role-swift.property#see-also)

### [Managing the Participant’s Capabilites](/documentation/cloudkit/ckshare/participant/role-swift.property#Managing-the-Participants-Capabilites)

[`var permission: CKShare.ParticipantPermission`](/documentation/cloudkit/ckshare/participant/permission-swift.property)

The participant’s permission level for the share.

[`typealias Permission`](/documentation/cloudkit/ckshare/participant/permission-swift.typealias)

A type that represents the permissions for a participant.

[`enum ParticipantPermission`](/documentation/cloudkit/ckshare/participantpermission)

Constants that represent the permissions to grant to a share participant.

[`typealias Role`](/documentation/cloudkit/ckshare/participant/role-swift.typealias)

A type that represents the role for a participant.

[`enum ParticipantRole`](/documentation/cloudkit/ckshare/participantrole)

Constants that represent the role of a share’s participant.

Current page is role

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

## [See Also](/documentation/cloudkit/ckshare/participant/role-swift.typealias#see-also)

### [Managing the Participant’s Capabilites](/documentation/cloudkit/ckshare/participant/role-swift.typealias#Managing-the-Participants-Capabilites)

[`var permission: CKShare.ParticipantPermission`](/documentation/cloudkit/ckshare/participant/permission-swift.property)

The participant’s permission level for the share.

[`typealias Permission`](/documentation/cloudkit/ckshare/participant/permission-swift.typealias)

A type that represents the permissions for a participant.

[`enum ParticipantPermission`](/documentation/cloudkit/ckshare/participantpermission)

Constants that represent the permissions to grant to a share participant.

[`var role: CKShare.ParticipantRole`](/documentation/cloudkit/ckshare/participant/role-swift.property)

The participant’s role for the share.

[`enum ParticipantRole`](/documentation/cloudkit/ckshare/participantrole)

Constants that represent the role of a share’s participant.

Current page is CKShare.Participant.Role

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

## [See Also](/documentation/cloudkit/ckshare/participantrole#see-also)

### [Managing the Participant’s Capabilites](/documentation/cloudkit/ckshare/participantrole#Managing-the-Participants-Capabilites)

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

Current page is CKShare.ParticipantRole

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

---
### Accessing Metadata
  
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

## [See Also](/documentation/cloudkit/ckshare/metadata/containeridentifier#see-also)

### [Accessing the Share](/documentation/cloudkit/ckshare/metadata/containeridentifier#Accessing-the-Share)

[`var share: CKShare`](/documentation/cloudkit/ckshare/metadata/share)

The share that owns the metadata.

[`var ownerIdentity: CKUserIdentity`](/documentation/cloudkit/ckshare/metadata/owneridentity)

The identity of the share’s owner.

Current page is containerIdentifier

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

## [See Also](/documentation/cloudkit/ckshare/metadata/owneridentity#see-also)

### [Accessing the Share](/documentation/cloudkit/ckshare/metadata/owneridentity#Accessing-the-Share)

[`var share: CKShare`](/documentation/cloudkit/ckshare/metadata/share)

The share that owns the metadata.

[`var containerIdentifier: String`](/documentation/cloudkit/ckshare/metadata/containeridentifier)

The ID of the share’s container.

Current page is ownerIdentity

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

## [See Also](/documentation/cloudkit/ckshare/metadata/hierarchicalrootrecordid#see-also)

### [Accessing the Root Record](/documentation/cloudkit/ckshare/metadata/hierarchicalrootrecordid#Accessing-the-Root-Record)

[`var rootRecord: CKRecord?`](/documentation/cloudkit/ckshare/metadata/rootrecord)

The share’s root record.

[`var rootRecordID: CKRecord.ID`](/documentation/cloudkit/ckshare/metadata/rootrecordid)

The record ID of the share’s root record.

Deprecated

Current page is hierarchicalRootRecordID

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

## [See Also](/documentation/cloudkit/ckshare/metadata/rootrecord#see-also)

### [Accessing the Root Record](/documentation/cloudkit/ckshare/metadata/rootrecord#Accessing-the-Root-Record)

[`var hierarchicalRootRecordID: CKRecord.ID?`](/documentation/cloudkit/ckshare/metadata/hierarchicalrootrecordid)

The record ID of the shared hierarchy’s root record.

[`var rootRecordID: CKRecord.ID`](/documentation/cloudkit/ckshare/metadata/rootrecordid)

The record ID of the share’s root record.

Deprecated

Current page is rootRecord

---
### Accessing the Participant’s Capabilities

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

## [See Also](/documentation/cloudkit/ckshare/metadata/participantrole#see-also)

### [Accessing the Participant’s Capabilities](/documentation/cloudkit/ckshare/metadata/participantrole#Accessing-the-Participants-Capabilities)

[`var participantPermission: CKShare.ParticipantPermission`](/documentation/cloudkit/ckshare/metadata/participantpermission)

The share’s permissions for the user who retrieves the metadata.

[`var participantStatus: CKShare.ParticipantAcceptanceStatus`](/documentation/cloudkit/ckshare/metadata/participantstatus)

The share’s participation status for the user who retrieves the metadata.

[`var participantType: CKShare.ParticipantType`](/documentation/cloudkit/ckshare/metadata/participanttype)

The share’s participation type for the user who retrieves the metadata.

Deprecated

Current page is participantRole

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

## [See Also](/documentation/cloudkit/ckshare/metadata/participantpermission#see-also)

### [Accessing the Participant’s Capabilities](/documentation/cloudkit/ckshare/metadata/participantpermission#Accessing-the-Participants-Capabilities)

[`var participantRole: CKShare.ParticipantRole`](/documentation/cloudkit/ckshare/metadata/participantrole)

The share’s participant role for the user who retrieves the metadata.

[`var participantStatus: CKShare.ParticipantAcceptanceStatus`](/documentation/cloudkit/ckshare/metadata/participantstatus)

The share’s participation status for the user who retrieves the metadata.

[`var participantType: CKShare.ParticipantType`](/documentation/cloudkit/ckshare/metadata/participanttype)

The share’s participation type for the user who retrieves the metadata.

Deprecated

Current page is participantPermission

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

## [See Also](/documentation/cloudkit/ckshare/metadata/participantstatus#see-also)

### [Accessing the Participant’s Capabilities](/documentation/cloudkit/ckshare/metadata/participantstatus#Accessing-the-Participants-Capabilities)

[`var participantRole: CKShare.ParticipantRole`](/documentation/cloudkit/ckshare/metadata/participantrole)

The share’s participant role for the user who retrieves the metadata.

[`var participantPermission: CKShare.ParticipantPermission`](/documentation/cloudkit/ckshare/metadata/participantpermission)

The share’s permissions for the user who retrieves the metadata.

[`var participantType: CKShare.ParticipantType`](/documentation/cloudkit/ckshare/metadata/participanttype)

The share’s participation type for the user who retrieves the metadata.

Deprecated

Current page is participantStatus

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

## [See Also](/documentation/cloudkit/ckshare/metadata/participanttype#see-also)

### [Accessing the Participant’s Capabilities](/documentation/cloudkit/ckshare/metadata/participanttype#Accessing-the-Participants-Capabilities)

[`var participantRole: CKShare.ParticipantRole`](/documentation/cloudkit/ckshare/metadata/participantrole)

The share’s participant role for the user who retrieves the metadata.

[`var participantPermission: CKShare.ParticipantPermission`](/documentation/cloudkit/ckshare/metadata/participantpermission)

The share’s permissions for the user who retrieves the metadata.

[`var participantStatus: CKShare.ParticipantAcceptanceStatus`](/documentation/cloudkit/ckshare/metadata/participantstatus)

The share’s participation status for the user who retrieves the metadata.

Current page is participantType

---
### Subscripting
# CKShare.SystemFieldKey | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   CKShare.SystemFieldKey

Enumeration

# CKShare.SystemFieldKey

Constants that represent the system fields of a share.

```
enum SystemFieldKey
```

## [Topics](/documentation/cloudkit/ckshare/systemfieldkey#topics)

### [Constants](/documentation/cloudkit/ckshare/systemfieldkey#Constants)

[`static let shareType: CKRecord.FieldKey`](/documentation/cloudkit/ckshare/systemfieldkey/sharetype)

The system field key for the share’s type.

[`static let title: CKRecord.FieldKey`](/documentation/cloudkit/ckshare/systemfieldkey/title)

The system field key for the share’s title.

[`static let thumbnailImageData: CKRecord.FieldKey`](/documentation/cloudkit/ckshare/systemfieldkey/thumbnailimagedata)

The system field key for the share’s thumbnail image data.

Current page is CKShare.SystemFieldKey

# CKShare.SystemFieldKey | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   CKShare.SystemFieldKey

Enumeration

# CKShare.SystemFieldKey

Constants that represent the system fields of a share.

```
enum SystemFieldKey
```

## [Topics](/documentation/cloudkit/ckshare/systemfieldkey#topics)

### [Constants](/documentation/cloudkit/ckshare/systemfieldkey#Constants)

[`static let shareType: CKRecord.FieldKey`](/documentation/cloudkit/ckshare/systemfieldkey/sharetype)

The system field key for the share’s type.

[`static let title: CKRecord.FieldKey`](/documentation/cloudkit/ckshare/systemfieldkey/title)

The system field key for the share’s title.

[`static let thumbnailImageData: CKRecord.FieldKey`](/documentation/cloudkit/ckshare/systemfieldkey/thumbnailimagedata)

The system field key for the share’s thumbnail image data.

Current page is CKShare.SystemFieldKey

# title | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.SystemFieldKey](/documentation/cloudkit/ckshare/systemfieldkey)
-   title

Type Property

# title

The system field key for the share’s title.

```
static let title: CKRecord.FieldKey
```

## [See Also](/documentation/cloudkit/ckshare/systemfieldkey/title#see-also)

### [Constants](/documentation/cloudkit/ckshare/systemfieldkey/title#Constants)

[`static let shareType: CKRecord.FieldKey`](/documentation/cloudkit/ckshare/systemfieldkey/sharetype)

The system field key for the share’s type.

[`static let thumbnailImageData: CKRecord.FieldKey`](/documentation/cloudkit/ckshare/systemfieldkey/thumbnailimagedata)

The system field key for the share’s thumbnail image data.

Current page is title

# thumbnailImageData | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.SystemFieldKey](/documentation/cloudkit/ckshare/systemfieldkey)
-   thumbnailImageData

Type Property

# thumbnailImageData

The system field key for the share’s thumbnail image data.

```
static let thumbnailImageData: CKRecord.FieldKey
```

## [See Also](/documentation/cloudkit/ckshare/systemfieldkey/thumbnailimagedata#see-also)

### [Constants](/documentation/cloudkit/ckshare/systemfieldkey/thumbnailimagedata#Constants)

[`static let shareType: CKRecord.FieldKey`](/documentation/cloudkit/ckshare/systemfieldkey/sharetype)

The system field key for the share’s type.

[`static let title: CKRecord.FieldKey`](/documentation/cloudkit/ckshare/systemfieldkey/title)

The system field key for the share’s title.

Current page is thumbnailImageData

---
  
### Classes

# CKShare.AccessRequester | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   CKShare.AccessRequester

Class

# CKShare.AccessRequester

```
class AccessRequester
```

## [Topics](/documentation/cloudkit/ckshare/accessrequester#topics)

### [Instance Properties](/documentation/cloudkit/ckshare/accessrequester#Instance-Properties)

[`var contact: CNContact`](/documentation/cloudkit/ckshare/accessrequester/contact)

A displayable `CNContact` representing the requester.

[`var participantLookupInfo: CKUserIdentity.LookupInfo`](/documentation/cloudkit/ckshare/accessrequester/participantlookupinfo)

Lookup information for the requester.

[`var userIdentity: CKUserIdentity`](/documentation/cloudkit/ckshare/accessrequester/useridentity)

The identity of the user requesting access to the share.

## [Relationships](/documentation/cloudkit/ckshare/accessrequester#relationships)

### [Inherits From](/documentation/cloudkit/ckshare/accessrequester#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckshare/accessrequester#conforms-to)

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

Current page is CKShare.AccessRequester

# contact | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.AccessRequester](/documentation/cloudkit/ckshare/accessrequester)
-   contact

Instance Property

# contact

A displayable `CNContact` representing the requester.

```
@NSCopying
var contact: CNContact { get }
```

## [Discussion](/documentation/cloudkit/ckshare/accessrequester/contact#discussion)

If the requester doesn’t exist in the user’s contacts or is not accessible, returns a newly created `CNContact`. This provides formatted requester information suitable for display in the application’s UI.

Current page is contact

# participantLookupInfo | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.AccessRequester](/documentation/cloudkit/ckshare/accessrequester)
-   participantLookupInfo

Instance Property

# participantLookupInfo

Lookup information for the requester.

```
@NSCopying
var participantLookupInfo: CKUserIdentity.LookupInfo { get }
```

## [Discussion](/documentation/cloudkit/ckshare/accessrequester/participantlookupinfo#discussion)

Use this lookup info with [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) to fetch the corresponding participant. Once fetched, add the participant to the share to approve the requester.

Current page is participantLookupInfo

# userIdentity | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.AccessRequester](/documentation/cloudkit/ckshare/accessrequester)
-   userIdentity

Instance Property

# userIdentity

The identity of the user requesting access to the share.

```
@NSCopying
var userIdentity: CKUserIdentity { get }
```

Current page is userIdentity

# CKShare.BlockedIdentity | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   CKShare.BlockedIdentity

Class

# CKShare.BlockedIdentity

```
class BlockedIdentity
```

## [Topics](/documentation/cloudkit/ckshare/blockedidentity#topics)

### [Instance Properties](/documentation/cloudkit/ckshare/blockedidentity#Instance-Properties)

[`var contact: CNContact`](/documentation/cloudkit/ckshare/blockedidentity/contact)

A displayable `CNContact` representing the blocked user.

[`var userIdentity: CKUserIdentity`](/documentation/cloudkit/ckshare/blockedidentity/useridentity)

The identity of the user who has been blocked from requesting access to the share.

## [Relationships](/documentation/cloudkit/ckshare/blockedidentity#relationships)

### [Inherits From](/documentation/cloudkit/ckshare/blockedidentity#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckshare/blockedidentity#conforms-to)

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

Current page is CKShare.BlockedIdentity

# contact | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.BlockedIdentity](/documentation/cloudkit/ckshare/blockedidentity)
-   contact

Instance Property

# contact

A displayable `CNContact` representing the blocked user.

```
@NSCopying
var contact: CNContact { get }
```

## [Discussion](/documentation/cloudkit/ckshare/blockedidentity/contact#discussion)

If the blocked identity does not exist in the user’s contacts or is not accessible, returns a newly created `CNContact`. This provides formatted blocked identity information suitable for display in the application’s UI.

Current page is contact

# userIdentity | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   [CKShare.BlockedIdentity](/documentation/cloudkit/ckshare/blockedidentity)
-   userIdentity

Instance Property

# userIdentity

The identity of the user who has been blocked from requesting access to the share.

```
@NSCopying
var userIdentity: CKUserIdentity { get }
```

Current page is userIdentity

---
  
Instance Properties

# allowsAccessRequests | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   allowsAccessRequests

Instance Property

# allowsAccessRequests

Indicates whether uninvited users can request access to this share.

```
var allowsAccessRequests: Bool { get set }
```

## [Discussion](/documentation/cloudkit/ckshare/allowsaccessrequests#discussion)

By default, this property is set to `NO`. When set to `YES`, uninvited users can request access to the share if they discover the share URL. When set to `NO`, the server prevents uninvited users from requesting access and does not indicate whether the share exists.

Only the share owner or an administrator can modify this property. Attempts by other participants to modify this property result in an exception.

Current page is allowsAccessRequests

# blockedIdentities | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   blockedIdentities

Instance Property

# blockedIdentities

A list of users blocked from requesting access to this share.

```
var blockedIdentities: [CKShare.BlockedIdentity] { get }
```

## [Discussion](/documentation/cloudkit/ckshare/blockedidentities#discussion)

Identities remain in this list until an owner or administrator calls [`unblockIdentities(_:)`](/documentation/cloudkit/ckshare/unblockidentities\(_:\)).

Current page is blockedIdentities

# requesters | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   requesters

Instance Property

# requesters

A list of all uninvited users who have requested access to this share.

```
var requesters: [CKShare.AccessRequester] { get }
```

## [Discussion](/documentation/cloudkit/ckshare/requesters#discussion)

When share access requests are allowed, uninvited users can request to join the share. All pending access requests appear in this array. Each requester is returned with name components and either an email or phone number.

Either share owners or administrators can respond to these access requests.

### [Responding to Access Requests:](/documentation/cloudkit/ckshare/requesters#Responding-to-Access-Requests)

-   **Approve Requesters:**
    
    -   Fetch the participant information by running [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) with the requester’s [`participantLookupInfo`](/documentation/cloudkit/ckshare/accessrequester/participantlookupinfo).
        
    -   Add the resulting participant to the share.
        
-   **Deny Requesters:**
    
    -   Use [`denyRequesters(_:)`](/documentation/cloudkit/ckshare/denyrequesters\(_:\)) to remove the requester from the requesters list.
        
-   **Block Requesters:**
    
    -   Use [`blockRequesters(_:)`](/documentation/cloudkit/ckshare/blockrequesters\(_:\)) to block requesters.
        
    -   Blocking a requester prevents them from sending future access requests to the share.
        

Current page is requesters

---
instance methods

# blockRequesters(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   blockRequesters(\_:)

Instance Method

# blockRequesters(\_:)

Blocks specified users from requesting access to this share.

```
func blockRequesters(_ requesters: [CKShare.AccessRequester])
```

## [Parameters](/documentation/cloudkit/ckshare/blockrequesters\(_:\)#parameters)

`requesters`

An array of [`CKShare.AccessRequester`](/documentation/cloudkit/ckshare/accessrequester) objects to block.

## [Discussion](/documentation/cloudkit/ckshare/blockrequesters\(_:\)#discussion)

Blocking prevents users from submitting future access requests and removes existing participants from the share. Blocked requesters appear in the [`blockedIdentities`](/documentation/cloudkit/ckshare/blockedidentities) array.

To persist this change, save the share to the server after calling this method.

Only the share owner or an administrator can invoke this method. Attempts by other participants result in an exception.

Current page is blockRequesters(\_:)

# denyRequesters(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   denyRequesters(\_:)

Instance Method

# denyRequesters(\_:)

Denies access requests from specified users.

```
func denyRequesters(_ requesters: [CKShare.AccessRequester])
```

## [Parameters](/documentation/cloudkit/ckshare/denyrequesters\(_:\)#parameters)

`requesters`

An array of [`CKShare.AccessRequester`](/documentation/cloudkit/ckshare/accessrequester) objects to deny.

## [Discussion](/documentation/cloudkit/ckshare/denyrequesters\(_:\)#discussion)

Use this method to deny pending access requests from uninvited users. Denied requesters are removed from the [`requesters`](/documentation/cloudkit/ckshare/requesters) array. To persist the changes, save the share to the server after calling this method.

After denial, requesters can still submit new access requests unless explicitly blocked using [`blockRequesters(_:)`](/documentation/cloudkit/ckshare/blockrequesters\(_:\)).

Only the share owner or an administrator can invoke this method. Attempts by other participants result in an exception.

Current page is denyRequesters(\_:)

# oneTimeURL(for:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   oneTimeURL(for:)

Instance Method

# oneTimeURL(for:)

```
func oneTimeURL(for participantID: CKShare.Participant.ID) -> URL?
```

Current page is oneTimeURL(for:)

# unblockIdentities(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShare](/documentation/cloudkit/ckshare)
-   unblockIdentities(\_:)

Instance Method

# unblockIdentities(\_:)

Unblocks previously blocked users, allowing them to request access again.

```
func unblockIdentities(_ blockedIdentities: [CKShare.BlockedIdentity])
```

## [Parameters](/documentation/cloudkit/ckshare/unblockidentities\(_:\)#parameters)

`blockedIdentities`

An array of [`CKShare.BlockedIdentity`](/documentation/cloudkit/ckshare/blockedidentity) objects to unblock.

## [Discussion](/documentation/cloudkit/ckshare/unblockidentities\(_:\)#discussion)

Use this method to remove specified identities from the [`blockedIdentities`](/documentation/cloudkit/ckshare/blockedidentities) array. Unblocked identities can request access again if access requests are enabled.

To persist this change, save the share to the server after calling this method.

Only the share owner or an administrator can invoke this method. Attempts by other participants result in an exception.

Current page is unblockIdentities(\_:)