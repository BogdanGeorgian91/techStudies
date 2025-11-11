# CKShareTransferRepresentation | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKShareTransferRepresentation

Structure

# CKShareTransferRepresentation

A transfer representation the system uses to share an item.

```
struct CKShareTransferRepresentation<Item> where Item : Transferable
```

## [Topics](/documentation/cloudkit/cksharetransferrepresentation#topics)

### [Creating a transfer representation](/documentation/cloudkit/cksharetransferrepresentation#Creating-a-transfer-representation)

[`init(exporter: (Item) throws -> CKShareTransferRepresentation<Item>.ExportedShare)`](/documentation/cloudkit/cksharetransferrepresentation/init\(exporter:\))

Creates and initializes a transfer representation.

### [Accessing transfer representation attributes](/documentation/cloudkit/cksharetransferrepresentation#Accessing-transfer-representation-attributes)

[`struct ExportedShare`](/documentation/cloudkit/cksharetransferrepresentation/exportedshare)

An intermediate structure that returns an existing share or prepares a new one if it doesn’t exist.

## [Relationships](/documentation/cloudkit/cksharetransferrepresentation#relationships)

### [Conforms To](/documentation/cloudkit/cksharetransferrepresentation#conforms-to)

-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)
-   [`TransferRepresentation`](/documentation/CoreTransferable/TransferRepresentation)

## [See Also](/documentation/cloudkit/cksharetransferrepresentation#see-also)

### [Collaboration](/documentation/cloudkit/cksharetransferrepresentation#Collaboration)

[

Sharing CloudKit Data with Other iCloud Users](/documentation/cloudkit/sharing-cloudkit-data-with-other-icloud-users)

Create and share private CloudKit data with other users by implementing the sharing UI.

[

Sharing Core Data objects between iCloud users](/documentation/CoreData/sharing-core-data-objects-between-icloud-users)

Use Core Data and CloudKit to synchronize data between devices of an iCloud user and share data between different iCloud users.

[`class CKShare`](/documentation/cloudkit/ckshare)

A specialized record type that manages a collection of shared records.

[`class CKAllowedSharingOptions`](documentation/cloudkit/CKAllowedSharingOptions.md)

An object that controls participant access and permission options.

[`class CKSystemSharingUIObserver`](/documentation/cloudkit/cksystemsharinguiobserver)

An object the system uses to monitor changes in sharing.

[`@MainActor class UICloudSharingController`](/documentation/UIKit/UICloudSharingController)

A view controller that presents standard screens for adding and removing people from a CloudKit share record.

[`CKSharingSupported`](/documentation/BundleResources/Information-Property-List/CKSharingSupported)

A Boolean value that indicates your app supports CloudKit Sharing.

Current page is CKShareTransferRepresentation

---
### Creating a transfer representation

# init(exporter:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShareTransferRepresentation](/documentation/cloudkit/cksharetransferrepresentation)
-   init(exporter:)

Initializer

# init(exporter:)

Creates and initializes a transfer representation.

```
init(exporter: @escaping (Item) throws -> CKShareTransferRepresentation<Item>.ExportedShare)
```

## [Parameters](/documentation/cloudkit/cksharetransferrepresentation/init\(exporter:\)#parameters)

`exporter`

A closure that provides a [`CKShareTransferRepresentation.ExportedShare`](/documentation/cloudkit/cksharetransferrepresentation/exportedshare) representation of the specified `Item`.

## [Return Value](/documentation/cloudkit/cksharetransferrepresentation/init\(exporter:\)#return-value)

A [`CKShareTransferRepresentation.ExportedShare`](/documentation/cloudkit/cksharetransferrepresentation/exportedshare).

Current page is init(exporter:)

---
### Accessing transfer representation attributes

# CKShareTransferRepresentation.ExportedShare | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShareTransferRepresentation](/documentation/cloudkit/cksharetransferrepresentation)
-   CKShareTransferRepresentation.ExportedShare

Structure

# CKShareTransferRepresentation.ExportedShare

An intermediate structure that returns an existing share or prepares a new one if it doesn’t exist.

```
struct ExportedShare
```

## [Topics](/documentation/cloudkit/cksharetransferrepresentation/exportedshare#topics)

### [Preparing an exported share](/documentation/cloudkit/cksharetransferrepresentation/exportedshare#Preparing-an-exported-share)

[`static func existing(CKShare, container: CKContainer, allowedSharingOptions: CKAllowedSharingOptions) -> CKShareTransferRepresentation<Item>.ExportedShare`](/documentation/cloudkit/cksharetransferrepresentation/exportedshare/existing\(_:container:allowedsharingoptions:\))

Allows the user to view or make modifications to the share settings.

[`static func prepareShare(container: CKContainer, allowedSharingOptions: CKAllowedSharingOptions, preparationHandler: () async throws -> CKShare) -> CKShareTransferRepresentation<Item>.ExportedShare`](/documentation/cloudkit/cksharetransferrepresentation/exportedshare/prepareshare\(container:allowedsharingoptions:preparationhandler:\))

Creates a share when the system calls the specified handler.

## [Relationships](/documentation/cloudkit/cksharetransferrepresentation/exportedshare#relationships)

### [Conforms To](/documentation/cloudkit/cksharetransferrepresentation/exportedshare#conforms-to)

-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)
-   [`Transferable`](/documentation/CoreTransferable/Transferable)

Current page is CKShareTransferRepresentation.ExportedShare

# existing(\_:container:allowedSharingOptions:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShareTransferRepresentation](/documentation/cloudkit/cksharetransferrepresentation)
-   [CKShareTransferRepresentation.ExportedShare](/documentation/cloudkit/cksharetransferrepresentation/exportedshare)
-   existing(\_:container:allowedSharingOptions:)

Type Method

# existing(\_:container:allowedSharingOptions:)

Allows the user to view or make modifications to the share settings.

```
static func existing(
    _ share: CKShare,
    container: CKContainer,
    allowedSharingOptions: CKAllowedSharingOptions = CKAllowedSharingOptions.standard
) -> CKShareTransferRepresentation<Item>.ExportedShare
```

## [Parameters](/documentation/cloudkit/cksharetransferrepresentation/exportedshare/existing\(_:container:allowedsharingoptions:\)#parameters)

`share`

The existing `CKShare` object.

`container`

The [`CKContainer`](/documentation/cloudkit/ckcontainer) for the share.

`allowedSharingOptions`

The [`CKAllowedSharingOptions`](documentation/cloudkit/CKAllowedSharingOptions.md). The [`standard`](/documentation/cloudkit/ckallowedsharingoptions/standard) option is the default.

## [Return Value](/documentation/cloudkit/cksharetransferrepresentation/exportedshare/existing\(_:container:allowedsharingoptions:\)#return-value)

The [`CKShareTransferRepresentation.ExportedShare`](/documentation/cloudkit/cksharetransferrepresentation/exportedshare) with updated share settings.

## [Discussion](/documentation/cloudkit/cksharetransferrepresentation/exportedshare/existing\(_:container:allowedsharingoptions:\)#Discussion)

Use this method when you have a share that’s already saved to the server.

When the system invokes the share sheet with a [`CKShare`](/documentation/cloudkit/ckshare) registered with this method, the system allows the owner to make modifications to the share settings, or allows a participant to view the share settings.

## [See Also](/documentation/cloudkit/cksharetransferrepresentation/exportedshare/existing\(_:container:allowedsharingoptions:\)#see-also)

### [Preparing an exported share](/documentation/cloudkit/cksharetransferrepresentation/exportedshare/existing\(_:container:allowedsharingoptions:\)#Preparing-an-exported-share)

[`static func prepareShare(container: CKContainer, allowedSharingOptions: CKAllowedSharingOptions, preparationHandler: () async throws -> CKShare) -> CKShareTransferRepresentation<Item>.ExportedShare`](/documentation/cloudkit/cksharetransferrepresentation/exportedshare/prepareshare\(container:allowedsharingoptions:preparationhandler:\))

Creates a share when the system calls the specified handler.

Current page is existing(\_:container:allowedSharingOptions:)

# prepareShare(container:allowedSharingOptions:preparationHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKShareTransferRepresentation](/documentation/cloudkit/cksharetransferrepresentation)
-   [CKShareTransferRepresentation.ExportedShare](/documentation/cloudkit/cksharetransferrepresentation/exportedshare)
-   prepareShare(container:allowedSharingOptions:preparationHandler:)

Type Method

# prepareShare(container:allowedSharingOptions:preparationHandler:)

Creates a share when the system calls the specified handler.

```
static func prepareShare(
    container: CKContainer,
    allowedSharingOptions: CKAllowedSharingOptions = CKAllowedSharingOptions.standard,
    preparationHandler: @escaping () async throws -> CKShare
) -> CKShareTransferRepresentation<Item>.ExportedShare
```

## [Parameters](/documentation/cloudkit/cksharetransferrepresentation/exportedshare/prepareshare\(container:allowedsharingoptions:preparationhandler:\)#parameters)

`container`

The [`CKContainer`](/documentation/cloudkit/ckcontainer) for the share.

`allowedSharingOptions`

The [`CKAllowedSharingOptions`](documentation/cloudkit/CKAllowedSharingOptions.md). The [`standard`](/documentation/cloudkit/ckallowedsharingoptions/standard) option is the default.

`preparationHandler`

The handler the system calls in your app to create a new [`CKShare`](/documentation/cloudkit/ckshare).

## [Return Value](/documentation/cloudkit/cksharetransferrepresentation/exportedshare/prepareshare\(container:allowedsharingoptions:preparationhandler:\)#return-value)

The [`CKShareTransferRepresentation.ExportedShare`](/documentation/cloudkit/cksharetransferrepresentation/exportedshare) with the new `CKShare`.

## [Discussion](/documentation/cloudkit/cksharetransferrepresentation/exportedshare/prepareshare\(container:allowedsharingoptions:preparationhandler:\)#Discussion)

Use this method when you want to share a collection of [`CKRecord`](/documentation/cloudkit/ckrecord) objects, but don’t currently have a [`CKShare`](/documentation/cloudkit/ckshare).

When the system calls the `preparationHandler`, create a new `CKShare` with the appropriate root `CKRecord` or [`CKRecordZone.ID`](/documentation/cloudkit/ckrecordzone/id).

After saving the share and all records to the server, return the resulting `CKShare` or throw an error if saving fails. When your app invokes the share sheet with a `CKShare`, the system prompts the user to start sharing.

## [See Also](/documentation/cloudkit/cksharetransferrepresentation/exportedshare/prepareshare\(container:allowedsharingoptions:preparationhandler:\)#see-also)

### [Preparing an exported share](/documentation/cloudkit/cksharetransferrepresentation/exportedshare/prepareshare\(container:allowedsharingoptions:preparationhandler:\)#Preparing-an-exported-share)

[`static func existing(CKShare, container: CKContainer, allowedSharingOptions: CKAllowedSharingOptions) -> CKShareTransferRepresentation<Item>.ExportedShare`](/documentation/cloudkit/cksharetransferrepresentation/exportedshare/existing\(_:container:allowedsharingoptions:\))

Allows the user to view or make modifications to the share settings.

Current page is prepareShare(container:allowedSharingOptions:preparationHandler:)