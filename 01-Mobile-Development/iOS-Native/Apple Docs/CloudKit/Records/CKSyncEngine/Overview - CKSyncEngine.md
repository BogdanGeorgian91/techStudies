# CKSyncEngine | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKSyncEngine

Class

# CKSyncEngine

An object that manages the synchronization of local and remote record data.

```
final class CKSyncEngine
```

## [Mentioned in](/documentation/cloudkit/cksyncengine-5sie5#mentions)

[

Deciding whether CloudKit is right for your app](/documentation/cloudkit/deciding-whether-cloudkit-is-right-for-your-app)

## [Overview](/documentation/cloudkit/cksyncengine-5sie5#overview)

Use [`CKSyncEngine`](/documentation/cloudkit/cksyncengine-5sie5) to handle your app’s CloudKit sync operations and benefit from the performance and reliability it provides. To use the class, create an instance early in your app’s launch process and specify a database to sync. Thereafter, and depending on good system conditions, the sync engine will periodically push and pull database and record zone changes on the app’s behalf. To participate in those sync operations and to provide the engine with the changes to send, create a type that conforms to [`CKSyncEngineDelegate`](/documentation/cloudkit/cksyncenginedelegate-1q7g8) and assign an instance of it to the engine’s configuration. You can have multiple instances of [`CKSyncEngine`](/documentation/cloudkit/cksyncengine-5sie5) in a single process, each targeting a different database. For example, you may have one syncing a person’s private database and another syncing their shared database.

Because periodic sync relies on good system conditions — adequate battery charge, an active network connection, a signed-in iCloud account, and so on — the engine’s sync schedule is indeterminate; if you need to sync immediately, like when you need to ensure your app has the most recent changes before continuing, use the [`fetchChanges(_:)`](/documentation/cloudkit/cksyncengine-5sie5/fetchchanges\(_:\)) and [`sendChanges(_:)`](/documentation/cloudkit/cksyncengine-5sie5/sendchanges\(_:\)) methods.

The sync engine uses an opaque type to track its internal state, and it’s your responsibility to persist that state to disk and make it available across app launches so the engine can function properly. For more information, see [`handleEvent(_:syncEngine:)`](/documentation/cloudkit/cksyncenginedelegate-1q7g8/handleevent\(_:syncengine:\)) and [`CKSyncEngine.Event.StateUpdate`](/documentation/cloudkit/cksyncengine-5sie5/event/stateupdate).

[`CKSyncEngine`](/documentation/cloudkit/cksyncengine-5sie5) requires the CloudKit and Remote notifications entitlements. For more information, see [Configuring iCloud services](/documentation/Xcode/configuring-icloud-services) and [Configuring background execution modes](/documentation/Xcode/configuring-background-execution-modes).

### [Send changes to iCloud](/documentation/cloudkit/cksyncengine-5sie5#Send-changes-to-iCloud)

A sync engine requires you to tell it about any changes to send, which you do by invoking the [`add(pendingDatabaseChanges:)`](/documentation/cloudkit/cksyncengine-5sie5/state-swift.class/add\(pendingdatabasechanges:\)) and [`add(pendingRecordZoneChanges:)`](/documentation/cloudkit/cksyncengine-5sie5/state-swift.class/add\(pendingrecordzonechanges:\)) methods on the engine’s [`state`](/documentation/cloudkit/cksyncengine-5sie5/state-swift.property) property. If there are no scheduled sync operations when you invoke these methods, the engine automatically schedules one. Database changes don’t require any additional input, but the sync engine does expect you to provide the individual record zone changes — in batches — and return them from your delegate’s implementation of [`nextRecordZoneChangeBatch(_:syncEngine:)`](/documentation/cloudkit/cksyncenginedelegate-1q7g8/nextrecordzonechangebatch\(_:syncengine:\)). After the engine sends the changes, it notifies your delegate about their success (or failure) by dispatching [`CKSyncEngine.Event.sentDatabaseChanges(_:)`](/documentation/cloudkit/cksyncengine-5sie5/event/sentdatabasechanges\(_:\)) and [`CKSyncEngine.Event.sentRecordZoneChanges(_:)`](/documentation/cloudkit/cksyncengine-5sie5/event/sentrecordzonechanges\(_:\)) events.

### [Fetch changes from iCloud](/documentation/cloudkit/cksyncengine-5sie5#Fetch-changes-from-iCloud)

By default, a sync engine attempts to discover an existing [`CKDatabaseSubscription`](/documentation/cloudkit/ckdatabasesubscription) for the associated database and uses that to receive silent notifications about remote record changes. If the engine doesn’t find a subscription, it automatically creates one to use. On receipt of a notification, the engine schedules a sync operation to fetch the related changes. When that operation runs, the engine dispatches a [`CKSyncEngine.Event.willFetchChanges(_:)`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges\(_:\)) event to your delegate. As it receives fetched changes, the engine dispatches [`CKSyncEngine.Event.fetchedDatabaseChanges(_:)`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges\(_:\)) and [`CKSyncEngine.Event.fetchedRecordZoneChanges(_:)`](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges\(_:\)), accordingly. After the operation finishes, the sync engine notifies your delegate by dispatching a [`CKSyncEngine.Event.didFetchChanges(_:)`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges\(_:\)) event. You handle all dispatched events in your delegate’s implementation of [`handleEvent(_:syncEngine:)`](/documentation/cloudkit/cksyncenginedelegate-1q7g8/handleevent\(_:syncengine:\)).

## [Topics](/documentation/cloudkit/cksyncengine-5sie5#topics)

### [Creating a sync engine](/documentation/cloudkit/cksyncengine-5sie5#Creating-a-sync-engine)

[`init(CKSyncEngine.Configuration)`](/documentation/cloudkit/cksyncengine-5sie5/init\(_:\))

Creates a sync engine with the specified configuration.

[`struct Configuration`](/documentation/cloudkit/cksyncengine-5sie5/configuration)

A type that configures the attributes and behavior of a sync engine.

### [Accessing the engine’s attributes](/documentation/cloudkit/cksyncengine-5sie5#Accessing-the-engines-attributes)

[`var database: CKDatabase`](/documentation/cloudkit/cksyncengine-5sie5/database)

The associated database.

[`var state: CKSyncEngine.State`](/documentation/cloudkit/cksyncengine-5sie5/state-swift.property)

The sync engine’s state.

[`class State`](/documentation/cloudkit/cksyncengine-5sie5/state-swift.class)

An object that manages the sync engine’s state.

### [Participating in scheduled sync operations](/documentation/cloudkit/cksyncengine-5sie5#Participating-in-scheduled-sync-operations)

[`protocol CKSyncEngineDelegate`](/documentation/cloudkit/cksyncenginedelegate-1q7g8)

An interface for providing record data to a sync engine and customizing that engine’s behavior.

### [Invoking manual sync operations](/documentation/cloudkit/cksyncengine-5sie5#Invoking-manual-sync-operations)

[`func fetchChanges(CKSyncEngine.FetchChangesOptions) async throws`](/documentation/cloudkit/cksyncengine-5sie5/fetchchanges\(_:\))

Fetches pending remote changes from the server.

[`struct FetchChangesOptions`](/documentation/cloudkit/cksyncengine-5sie5/fetchchangesoptions)

A set of options to use with a fetch operation.

[`func sendChanges(CKSyncEngine.SendChangesOptions) async throws`](/documentation/cloudkit/cksyncengine-5sie5/sendchanges\(_:\))

Sends pending local changes to the server.

[`struct SendChangesOptions`](/documentation/cloudkit/cksyncengine-5sie5/sendchangesoptions)

A set of options to use with a send operation.

### [Canceling operations](/documentation/cloudkit/cksyncengine-5sie5#Canceling-operations)

[`func cancelOperations() async`](/documentation/cloudkit/cksyncengine-5sie5/canceloperations\(\))

Cancels any in-progress or pending sync operations.

### [Structures](/documentation/cloudkit/cksyncengine-5sie5#Structures)

[`struct FetchChangesContext`](/documentation/cloudkit/cksyncengine-5sie5/fetchchangescontext)

[`struct RecordZoneChangeBatch`](/documentation/cloudkit/cksyncengine-5sie5/recordzonechangebatch)

A type that contains the record changes for a single send operation.

[`struct SendChangesContext`](/documentation/cloudkit/cksyncengine-5sie5/sendchangescontext)

A type that describes a single attempt to send changes to the iCloud servers.

### [Enumerations](/documentation/cloudkit/cksyncengine-5sie5#Enumerations)

[`enum Event`](/documentation/cloudkit/cksyncengine-5sie5/event)

Describes an event that occurs during a sync operation.

[`enum PendingDatabaseChange`](/documentation/cloudkit/cksyncengine-5sie5/pendingdatabasechange)

Describes an unsent database modification.

[`enum PendingRecordZoneChange`](/documentation/cloudkit/cksyncengine-5sie5/pendingrecordzonechange)

Describes an unsent record modification.

[`enum SyncReason`](/documentation/cloudkit/cksyncengine-5sie5/syncreason)

Describes the reason for a sync operation.

## [Relationships](/documentation/cloudkit/cksyncengine-5sie5#relationships)

### [Conforms To](/documentation/cloudkit/cksyncengine-5sie5#conforms-to)

-   [`Copyable`](/documentation/Swift/Copyable)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/cksyncengine-5sie5#see-also)

### [Records](/documentation/cloudkit/cksyncengine-5sie5#Records)

[

API Reference

Local Records](/documentation/cloudkit/local-records)

Manipulate records on-device and save changes to the server.

[

API Reference

Remote Records](/documentation/cloudkit/remote-records)

Use subscriptions and change tokens to efficiently manage modifications to remote records.

[

API Reference

Shared Records](/documentation/cloudkit/shared-records)

Share one or more records with other iCloud users.

Current page is CKSyncEngine