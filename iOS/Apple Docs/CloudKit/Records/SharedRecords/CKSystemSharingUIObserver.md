# CKSystemSharingUIObserver | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKSystemSharingUIObserver

Class

# CKSystemSharingUIObserver

An object the system uses to monitor changes in sharing.

```
class CKSystemSharingUIObserver
```

## [Overview](/documentation/cloudkit/cksystemsharinguiobserver#overview)

Initialize a `CKSystemSharingUIObserver` instance with your [`CKContainer`](/documentation/cloudkit/ckcontainer) when preparing to share an item using the share sheet. Use your implementation to update the local state of a shared item when your app receives a [`CKShare`](/documentation/cloudkit/ckshare), or to delete a locally cached share when the system notifies your app about a share deletion.

The system only propagates changes on the local device using `CKSystemSharingUIObserver`. The system doesn’t notify your app about any remote changes on the server. For more information about how to keep your local cache in sync with remote changes, see [Remote Records](/documentation/cloudkit/remote-records).

## [Topics](/documentation/cloudkit/cksystemsharinguiobserver#topics)

### [Creating a sharing observer](/documentation/cloudkit/cksystemsharinguiobserver#Creating-a-sharing-observer)

[`init(container: CKContainer)`](/documentation/cloudkit/cksystemsharinguiobserver/init\(container:\))

Creates and initializes an observer using the provided container.

### [Accessing sharing blocks](/documentation/cloudkit/cksystemsharinguiobserver#Accessing-sharing-blocks)

[`var systemSharingUIDidSaveShareBlock: ((CKRecord.ID, Result<CKShare, any Error>) -> Void)?`](/documentation/cloudkit/cksystemsharinguiobserver/systemsharinguididsaveshareblock-8c9vi)

A callback block the system invokes after the success or failure of a system sharing UI save.

[`var systemSharingUIDidStopSharingBlock: ((CKRecord.ID, Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/cksystemsharinguiobserver/systemsharinguididstopsharingblock-7nmiw)

A callback block the system invokes after the success or failure of a system sharing UI delete.

## [Relationships](/documentation/cloudkit/cksystemsharinguiobserver#relationships)

### [Inherits From](/documentation/cloudkit/cksystemsharinguiobserver#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/cksystemsharinguiobserver#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/cksystemsharinguiobserver#see-also)

### [Collaboration](/documentation/cloudkit/cksystemsharinguiobserver#Collaboration)

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

[`class CKAllowedSharingOptions`](/documentation/cloudkit/ckallowedsharingoptions)

An object that controls participant access and permission options.

[`@MainActor class UICloudSharingController`](/documentation/UIKit/UICloudSharingController)

A view controller that presents standard screens for adding and removing people from a CloudKit share record.

[`CKSharingSupported`](/documentation/BundleResources/Information-Property-List/CKSharingSupported)

A Boolean value that indicates your app supports CloudKit Sharing.

Current page is CKSystemSharingUIObserver

---
  
Creating a sharing observer

# init(container:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSystemSharingUIObserver](/documentation/cloudkit/cksystemsharinguiobserver)
-   init(container:)

Initializer

# init(container:)

Creates and initializes an observer using the provided container.

```
init(container: CKContainer)
```

## [Parameters](/documentation/cloudkit/cksystemsharinguiobserver/init\(container:\)#parameters)

`container`

The [`CKContainer`](/documentation/cloudkit/ckcontainer) for the sharing observer.

Current page is init(container:)

---

  
Accessing sharing blocks

# systemSharingUIDidSaveShareBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSystemSharingUIObserver](/documentation/cloudkit/cksystemsharinguiobserver)
-   systemSharingUIDidSaveShareBlock

Instance Property

# systemSharingUIDidSaveShareBlock

A callback block the system invokes after the success or failure of a system sharing UI save.

```
@preconcurrency
var systemSharingUIDidSaveShareBlock: ((CKRecord.ID, Result<CKShare, any Error>) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/cksystemsharinguiobserver/systemsharinguididsaveshareblock-8c9vi#Discussion)

Following a successful share save by the system sharing UI in the provided [`CKContainer`](/documentation/cloudkit/ckcontainer), the system invokes this callback with a `nonnull` [`CKRecord.ID`](/documentation/cloudkit/ckrecord/id), a `nonnull` share, and a `nil` error.

If a save failure occurs due to a per-item error like [`CKError.Code.serverRecordChanged`](/documentation/cloudkit/ckerror/code/serverrecordchanged), the system invokes this callback with a `nonnull` `CKRecord.ID`, a `nil` share, and a `nonnull` error.

Each [`CKSystemSharingUIObserver`](/documentation/cloudkit/cksystemsharinguiobserver) instance has a private serial queue. The system uses this queue for all callback block invocations.

## [See Also](/documentation/cloudkit/cksystemsharinguiobserver/systemsharinguididsaveshareblock-8c9vi#see-also)

### [Accessing sharing blocks](/documentation/cloudkit/cksystemsharinguiobserver/systemsharinguididsaveshareblock-8c9vi#Accessing-sharing-blocks)

[`var systemSharingUIDidStopSharingBlock: ((CKRecord.ID, Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/cksystemsharinguiobserver/systemsharinguididstopsharingblock-7nmiw)

A callback block the system invokes after the success or failure of a system sharing UI delete.

Current page is systemSharingUIDidSaveShareBlock

# systemSharingUIDidStopSharingBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSystemSharingUIObserver](/documentation/cloudkit/cksystemsharinguiobserver)
-   systemSharingUIDidStopSharingBlock

Instance Property

# systemSharingUIDidStopSharingBlock

A callback block the system invokes after the success or failure of a system sharing UI delete.

```
@preconcurrency
var systemSharingUIDidStopSharingBlock: ((CKRecord.ID, Result<Void, any Error>) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/cksystemsharinguiobserver/systemsharinguididstopsharingblock-7nmiw#Discussion)

The system invokes this block on the success or failure of a [`CKShare`](/documentation/cloudkit/ckshare) delete when the user decides to stop sharing through the system sharing UI.

Each [`CKSystemSharingUIObserver`](/documentation/cloudkit/cksystemsharinguiobserver) instance has a private serial queue. The system uses this queue for all callback block invocations.

## [See Also](/documentation/cloudkit/cksystemsharinguiobserver/systemsharinguididstopsharingblock-7nmiw#see-also)

### [Accessing sharing blocks](/documentation/cloudkit/cksystemsharinguiobserver/systemsharinguididstopsharingblock-7nmiw#Accessing-sharing-blocks)

[`var systemSharingUIDidSaveShareBlock: ((CKRecord.ID, Result<CKShare, any Error>) -> Void)?`](/documentation/cloudkit/cksystemsharinguiobserver/systemsharinguididsaveshareblock-8c9vi)

A callback block the system invokes after the success or failure of a system sharing UI save.

Current page is systemSharingUIDidStopSharingBlock