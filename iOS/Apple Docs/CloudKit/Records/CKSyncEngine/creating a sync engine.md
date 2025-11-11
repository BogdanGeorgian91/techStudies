# init(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   init(\_:)

Initializer

# init(\_:)

Creates a sync engine with the specified configuration.

```
init(_ configuration: CKSyncEngine.Configuration)
```

## [Parameters](/documentation/cloudkit/cksyncengine-5sie5/init\(_:\)#parameters)

`configuration`

The attributes of the new sync engine, such as the associated database and the object to use as the engine’s delegate. For more information, see [`CKSyncEngine.Configuration`](/documentation/cloudkit/cksyncengine-5sie5/configuration).

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/init\(_:\)#see-also)

### [Creating a sync engine](/documentation/cloudkit/cksyncengine-5sie5/init\(_:\)#Creating-a-sync-engine)

[`struct Configuration`](/documentation/cloudkit/cksyncengine-5sie5/configuration)

A type that configures the attributes and behavior of a sync engine.

Current page is init(\_:)

---
# CKSyncEngine.Configuration

### Creating configurations

# init(database:stateSerialization:delegate:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Configuration](/documentation/cloudkit/cksyncengine-5sie5/configuration)
-   init(database:stateSerialization:delegate:)

Initializer

# init(database:stateSerialization:delegate:)

Creates a configuration for the specified database and serialized state.

```
init(
    database: CKDatabase,
    stateSerialization: CKSyncEngine.State.Serialization?,
    delegate: any CKSyncEngineDelegate
)
```

## [Parameters](/documentation/cloudkit/cksyncengine-5sie5/configuration/init\(database:stateserialization:delegate:\)#parameters)

`database`

The database to sync — either a person’s private database or their shared database.

`stateSerialization`

If this is the first initialization of the associated sync engine, specify `nil`; otherwise, specify the state from the most recent [`CKSyncEngine.Event.stateUpdate(_:)`](/documentation/cloudkit/cksyncengine-5sie5/event/stateupdate\(_:\)) event that your delegate handled.

`delegate`

The object that provides the records to sync and handles any related events.

Current page is init(database:stateSerialization:delegate:)

---
  
Handling record changes

# delegate | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Configuration](/documentation/cloudkit/cksyncengine-5sie5/configuration)
-   delegate

Instance Property

# delegate

The object that provides the records to sync and handles any related events.

```
var delegate: any CKSyncEngineDelegate
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/configuration/delegate#see-also)

### [Handling record changes](/documentation/cloudkit/cksyncengine-5sie5/configuration/delegate#Handling-record-changes)

[`protocol CKSyncEngineDelegate`](/documentation/cloudkit/cksyncenginedelegate-1q7g8)

An interface for providing record data to a sync engine and customizing that engine’s behavior.

Current page is delegate

---
# CKSyncEngineDelegate

  
Handling sync events

# handleEvent(\_:syncEngine:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngineDelegate](/documentation/cloudkit/cksyncenginedelegate-1q7g8)
-   handleEvent(\_:syncEngine:)

Instance Method

# handleEvent(\_:syncEngine:)

Tells the delegate to handle the specified sync event.

```
func handleEvent(
    _ event: CKSyncEngine.Event,
    syncEngine: CKSyncEngine
) async
```

**Required**

## [Parameters](/documentation/cloudkit/cksyncenginedelegate-1q7g8/handleevent\(_:syncengine:\)#parameters)

`event`

Information about the event. An event may occur for a number of reasons, such as when new data is available or when the device’s iCloud account changes. For more information, see [`CKSyncEngine.Event`](/documentation/cloudkit/cksyncengine-5sie5/event).

`syncEngine`

The sync engine that generates the event.

## [Discussion](/documentation/cloudkit/cksyncenginedelegate-1q7g8/handleevent\(_:syncengine:\)#Discussion)

The sync engines provides events serially; your delegate won’t receive the subsequent event until it finishes processing the current one and returns from this method.

## [See Also](/documentation/cloudkit/cksyncenginedelegate-1q7g8/handleevent\(_:syncengine:\)#see-also)

### [Handling sync events](/documentation/cloudkit/cksyncenginedelegate-1q7g8/handleevent\(_:syncengine:\)#Handling-sync-events)

[`enum Event`](/documentation/cloudkit/cksyncengine-5sie5/event)

Describes an event that occurs during a sync operation.

[`enum CKSyncEngineEventType`](/documentation/cloudkit/cksyncengineeventtype)

Describes an event that occurs during a sync operation.

Current page is handleEvent(\_:syncEngine:)

# CKSyncEngine.Event

# CKSyncEngine.Event.accountChange(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.accountChange(\_:)

Case

# CKSyncEngine.Event.accountChange(\_:)

An event indicating a change to the device’s iCloud account.

```
case accountChange(CKSyncEngine.Event.AccountChange)
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange\(_:\)#see-also)

### [Account changes](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange\(_:\)#Account-changes)

[`struct AccountChange`](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange)

A type that provides information about a change to the device’s iCloud account.

Current page is CKSyncEngine.Event.accountChange(\_:)

# CKSyncEngine.Event.AccountChange

### Understanding the change

# changeType | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.AccountChange](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange)
-   changeType

Instance Property

# changeType

The iCloud account’s change type.

```
let changeType: CKSyncEngine.Event.AccountChange.ChangeType
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.property#see-also)

### [Understanding the change](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.property#Understanding-the-change)

[`enum ChangeType`](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum)

Describes a change to the device’s iCloud account.

[`enum CKSyncEngineAccountChangeType`](/documentation/cloudkit/cksyncengineaccountchangetype)

Describes a change to the device’s iCloud account.

Current page is changeType

# CKSyncEngine.Event.AccountChange.ChangeType | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.AccountChange](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange)
-   CKSyncEngine.Event.AccountChange.ChangeType

Enumeration

# CKSyncEngine.Event.AccountChange.ChangeType

Describes a change to the device’s iCloud account.

```
enum ChangeType
```

## [Topics](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum#topics)

### [Account change types](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum#Account-change-types)

[`case signIn(currentUser: CKRecord.ID)`](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/signin\(currentuser:\))

A change indicating a sign-in to an iCloud account.

[`case signOut(previousUser: CKRecord.ID)`](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/signout\(previoususer:\))

A change indicating a sign-out of an iCloud account.

[`case switchAccounts(previousUser: CKRecord.ID, currentUser: CKRecord.ID)`](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/switchaccounts\(previoususer:currentuser:\))

A change indicating a switch between two iCloud accounts.

## [Relationships](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum#relationships)

### [Conforms To](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum#conforms-to)

-   [`Copyable`](/documentation/Swift/Copyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum#see-also)

### [Understanding the change](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum#Understanding-the-change)

[`let changeType: CKSyncEngine.Event.AccountChange.ChangeType`](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.property)

The iCloud account’s change type.

[`enum CKSyncEngineAccountChangeType`](/documentation/cloudkit/cksyncengineaccountchangetype)

Describes a change to the device’s iCloud account.

Current page is CKSyncEngine.Event.AccountChange.ChangeType

  
Account change types

# CKSyncEngine.Event.AccountChange.ChangeType.signIn(currentUser:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   -   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.AccountChange](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange)
-   [CKSyncEngine.Event.AccountChange.ChangeType](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum)
-   CKSyncEngine.Event.AccountChange.ChangeType.signIn(currentUser:)

Case

# CKSyncEngine.Event.AccountChange.ChangeType.signIn(currentUser:)

A change indicating a sign-in to an iCloud account.

```
case signIn(currentUser: CKRecord.ID)
```

## [Discussion](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/signin\(currentuser:\)#Discussion)

If your app has locally stored data when [`CKSyncEngine`](/documentation/cloudkit/cksyncengine-5sie5) notifies it about the device signing in to an iCloud account, perform one of the following actions:

-   Keep the local data separate from any remote data
    
-   Merge the local data with the account’s remote data
    
-   Delete the local data
    
-   Prompt the account’s owner to make the decision
    

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/signin\(currentuser:\)#see-also)

### [Account change types](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/signin\(currentuser:\)#Account-change-types)

[`case signOut(previousUser: CKRecord.ID)`](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/signout\(previoususer:\))

A change indicating a sign-out of an iCloud account.

[`case switchAccounts(previousUser: CKRecord.ID, currentUser: CKRecord.ID)`](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/switchaccounts\(previoususer:currentuser:\))

A change indicating a switch between two iCloud accounts.

Current page is CKSyncEngine.Event.AccountChange.ChangeType.signIn(currentUser:)

# CKSyncEngine.Event.AccountChange.ChangeType.signOut(previousUser:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   -   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.AccountChange](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange)
-   [CKSyncEngine.Event.AccountChange.ChangeType](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum)
-   CKSyncEngine.Event.AccountChange.ChangeType.signOut(previousUser:)

Case

# CKSyncEngine.Event.AccountChange.ChangeType.signOut(previousUser:)

A change indicating a sign-out of an iCloud account.

```
case signOut(previousUser: CKRecord.ID)
```

## [Discussion](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/signout\(previoususer:\)#Discussion)

After the device signs out of the iCloud account, delete any locally stored data that belongs to that account.

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/signout\(previoususer:\)#see-also)

### [Account change types](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/signout\(previoususer:\)#Account-change-types)

[`case signIn(currentUser: CKRecord.ID)`](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/signin\(currentuser:\))

A change indicating a sign-in to an iCloud account.

[`case switchAccounts(previousUser: CKRecord.ID, currentUser: CKRecord.ID)`](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/switchaccounts\(previoususer:currentuser:\))

A change indicating a switch between two iCloud accounts.

Current page is CKSyncEngine.Event.AccountChange.ChangeType.signOut(previousUser:)

# CKSyncEngine.Event.AccountChange.ChangeType.switchAccounts(previousUser:currentUser:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   -   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.AccountChange](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange)
-   [CKSyncEngine.Event.AccountChange.ChangeType](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum)
-   CKSyncEngine.Event.AccountChange.ChangeType.switchAccounts(previousUser:currentUser:)

Case

# CKSyncEngine.Event.AccountChange.ChangeType.switchAccounts(previousUser:currentUser:)

A change indicating a switch between two iCloud accounts.

```
case switchAccounts(
    previousUser: CKRecord.ID,
    currentUser: CKRecord.ID
)
```

## [Discussion](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/switchaccounts\(previoususer:currentuser:\)#Discussion)

If the device changes iCloud accounts while your app isn’t running, [`CKSyncEngine`](/documentation/cloudkit/cksyncengine-5sie5) notifies it about that change when the app next launches. Make sure to delete any locally stored data belonging to the `previousUser` account.

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/switchaccounts\(previoususer:currentuser:\)#see-also)

### [Account change types](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/switchaccounts\(previoususer:currentuser:\)#Account-change-types)

[`case signIn(currentUser: CKRecord.ID)`](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/signin\(currentuser:\))

A change indicating a sign-in to an iCloud account.

[`case signOut(previousUser: CKRecord.ID)`](/documentation/cloudkit/cksyncengine-5sie5/event/accountchange/changetype-swift.enum/signout\(previoususer:\))

A change indicating a sign-out of an iCloud account.

Current page is CKSyncEngine.Event.AccountChange.ChangeType.switchAccounts(previousUser:currentUser:)

---
# CKSyncEngineAccountChangeType

  
Account change types

# CKSyncEngineAccountChangeType.signIn | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngineAccountChangeType](/documentation/cloudkit/cksyncengineaccountchangetype)
-   CKSyncEngineAccountChangeType.signIn

Case

# CKSyncEngineAccountChangeType.signIn

A change indicating a sign-in to an iCloud account.

```
case signIn
```

## [See Also](/documentation/cloudkit/cksyncengineaccountchangetype/signin#see-also)

### [Account change types](/documentation/cloudkit/cksyncengineaccountchangetype/signin#Account-change-types)

[`case signOut`](/documentation/cloudkit/cksyncengineaccountchangetype/signout)

A change indicating a sign-out of an iCloud account.

[`case switchAccounts`](/documentation/cloudkit/cksyncengineaccountchangetype/switchaccounts)

A change indicating a switch between two iCloud accounts.

Current page is CKSyncEngineAccountChangeType.signIn

# CKSyncEngineAccountChangeType.signOut | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngineAccountChangeType](/documentation/cloudkit/cksyncengineaccountchangetype)
-   CKSyncEngineAccountChangeType.signOut

Case

# CKSyncEngineAccountChangeType.signOut

A change indicating a sign-out of an iCloud account.

```
case signOut
```

## [See Also](/documentation/cloudkit/cksyncengineaccountchangetype/signout#see-also)

### [Account change types](/documentation/cloudkit/cksyncengineaccountchangetype/signout#Account-change-types)

[`case signIn`](/documentation/cloudkit/cksyncengineaccountchangetype/signin)

A change indicating a sign-in to an iCloud account.

[`case switchAccounts`](/documentation/cloudkit/cksyncengineaccountchangetype/switchaccounts)

A change indicating a switch between two iCloud accounts.

Current page is CKSyncEngineAccountChangeType.signOut

# CKSyncEngineAccountChangeType.switchAccounts | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngineAccountChangeType](/documentation/cloudkit/cksyncengineaccountchangetype)
-   CKSyncEngineAccountChangeType.switchAccounts

Case

# CKSyncEngineAccountChangeType.switchAccounts

A change indicating a switch between two iCloud accounts.

```
case switchAccounts
```

## [See Also](/documentation/cloudkit/cksyncengineaccountchangetype/switchaccounts#see-also)

### [Account change types](/documentation/cloudkit/cksyncengineaccountchangetype/switchaccounts#Account-change-types)

[`case signIn`](/documentation/cloudkit/cksyncengineaccountchangetype/signin)

A change indicating a sign-in to an iCloud account.

[`case signOut`](/documentation/cloudkit/cksyncengineaccountchangetype/signout)

A change indicating a sign-out of an iCloud account.

Current page is CKSyncEngineAccountChangeType.switchAccounts

# init(rawValue:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngineAccountChangeType](/documentation/cloudkit/cksyncengineaccountchangetype)
-   init(rawValue:)

Initializer

# init(rawValue:)

```
init?(rawValue: Int)
```

Current page is init(rawValue:)

---
### Remote database changes

# CKSyncEngine.Event.willFetchChanges(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.willFetchChanges(\_:)

Case

# CKSyncEngine.Event.willFetchChanges(\_:)

An event indicating an imminent database fetch.

```
case willFetchChanges(CKSyncEngine.Event.WillFetchChanges)
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges\(_:\)#see-also)

### [Remote database changes](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges\(_:\)#Remote-database-changes)

[`struct WillFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges)

A type that provides information about an imminent database fetch.

[`case fetchedDatabaseChanges(CKSyncEngine.Event.FetchedDatabaseChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges\(_:\))

An event indicating there are fetched database changes to process.

[`struct FetchedDatabaseChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges)

A type that provides information about fetched database changes.

[`case didFetchChanges(CKSyncEngine.Event.DidFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges\(_:\))

An event that indicates the database fetch is done.

[`struct DidFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges)

A type that provides information about a finished database fetch.

Current page is CKSyncEngine.Event.willFetchChanges(\_:)

# CKSyncEngine.Event.WillFetchChanges
# context | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.WillFetchChanges](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges)
-   context

Instance Property

# context

```
let context: CKSyncEngine.FetchChangesContext
```

Current page is context

# CKSyncEngine.Event.fetchedDatabaseChanges(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.fetchedDatabaseChanges(\_:)

Case

# CKSyncEngine.Event.fetchedDatabaseChanges(\_:)

An event indicating there are fetched database changes to process.

```
case fetchedDatabaseChanges(CKSyncEngine.Event.FetchedDatabaseChanges)
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges\(_:\)#see-also)

### [Remote database changes](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges\(_:\)#Remote-database-changes)

[`case willFetchChanges(CKSyncEngine.Event.WillFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges\(_:\))

An event indicating an imminent database fetch.

[`struct WillFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges)

A type that provides information about an imminent database fetch.

[`struct FetchedDatabaseChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges)

A type that provides information about fetched database changes.

[`case didFetchChanges(CKSyncEngine.Event.DidFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges\(_:\))

An event that indicates the database fetch is done.

[`struct DidFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges)

A type that provides information about a finished database fetch.

Current page is CKSyncEngine.Event.fetchedDatabaseChanges(\_:)

# CKSyncEngine.Event.FetchedDatabaseChanges

# deletions | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.FetchedDatabaseChanges](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges)
-   deletions

Instance Property

# deletions

The fetched record deletions.

```
let deletions: [CKDatabase.DatabaseChange.Deletion]
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/deletions#see-also)

### [Accessing changes](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/deletions#Accessing-changes)

[`enum CKSyncEngineZoneDeletionReason`](/documentation/cloudkit/cksyncenginezonedeletionreason)

Describes the reason for a record zone deletion.

[`let modifications: [CKDatabase.DatabaseChange.Modification]`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/modifications)

The fetched record modifications.

Current page is deletions

# CKSyncEngineZoneDeletionReason

# CKSyncEngineZoneDeletionReason | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKSyncEngineZoneDeletionReason

Enumeration

# CKSyncEngineZoneDeletionReason

Describes the reason for a record zone deletion.

```
enum CKSyncEngineZoneDeletionReason
```

## [Topics](/documentation/cloudkit/cksyncenginezonedeletionreason#topics)

### [Deletion reasons](/documentation/cloudkit/cksyncenginezonedeletionreason#Deletion-reasons)

[`case deleted`](/documentation/cloudkit/cksyncenginezonedeletionreason/deleted)

Your app deleted the record zone.

[`case encryptedDataReset`](/documentation/cloudkit/cksyncenginezonedeletionreason/encrypteddatareset)

The owner of the iCloud account reset their encrypted data.

[`case purged`](/documentation/cloudkit/cksyncenginezonedeletionreason/purged)

The owner of the iCloud account purged your app’s data using the Settings app.

### [Initializers](/documentation/cloudkit/cksyncenginezonedeletionreason#Initializers)

[`init?(rawValue: Int)`](/documentation/cloudkit/cksyncenginezonedeletionreason/init\(rawvalue:\))

## [Relationships](/documentation/cloudkit/cksyncenginezonedeletionreason#relationships)

### [Conforms To](/documentation/cloudkit/cksyncenginezonedeletionreason#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/cksyncenginezonedeletionreason#see-also)

### [Accessing changes](/documentation/cloudkit/cksyncenginezonedeletionreason#Accessing-changes)

[`let deletions: [CKDatabase.DatabaseChange.Deletion]`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/deletions)

The fetched record deletions.

[`let modifications: [CKDatabase.DatabaseChange.Modification]`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/modifications)

The fetched record modifications.

Current page is CKSyncEngineZoneDeletionReason

# CKSyncEngineZoneDeletionReason.deleted | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngineZoneDeletionReason](/documentation/cloudkit/cksyncenginezonedeletionreason)
-   CKSyncEngineZoneDeletionReason.deleted

Case

# CKSyncEngineZoneDeletionReason.deleted

Your app deleted the record zone.

```
case deleted
```

## [See Also](/documentation/cloudkit/cksyncenginezonedeletionreason/deleted#see-also)

### [Deletion reasons](/documentation/cloudkit/cksyncenginezonedeletionreason/deleted#Deletion-reasons)

[`case encryptedDataReset`](/documentation/cloudkit/cksyncenginezonedeletionreason/encrypteddatareset)

The owner of the iCloud account reset their encrypted data.

[`case purged`](/documentation/cloudkit/cksyncenginezonedeletionreason/purged)

The owner of the iCloud account purged your app’s data using the Settings app.

Current page is CKSyncEngineZoneDeletionReason.deleted

# CKSyncEngineZoneDeletionReason.encryptedDataReset | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngineZoneDeletionReason](/documentation/cloudkit/cksyncenginezonedeletionreason)
-   CKSyncEngineZoneDeletionReason.encryptedDataReset

Case

# CKSyncEngineZoneDeletionReason.encryptedDataReset

The owner of the iCloud account reset their encrypted data.

```
case encryptedDataReset
```

## [Discussion](/documentation/cloudkit/cksyncenginezonedeletionreason/encrypteddatareset#Discussion)

## [See Also](/documentation/cloudkit/cksyncenginezonedeletionreason/encrypteddatareset#see-also)

### [Deletion reasons](/documentation/cloudkit/cksyncenginezonedeletionreason/encrypteddatareset#Deletion-reasons)

[`case deleted`](/documentation/cloudkit/cksyncenginezonedeletionreason/deleted)

Your app deleted the record zone.

[`case purged`](/documentation/cloudkit/cksyncenginezonedeletionreason/purged)

The owner of the iCloud account purged your app’s data using the Settings app.

Current page is CKSyncEngineZoneDeletionReason.encryptedDataReset

# CKSyncEngineZoneDeletionReason.purged | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngineZoneDeletionReason](/documentation/cloudkit/cksyncenginezonedeletionreason)
-   CKSyncEngineZoneDeletionReason.purged

Case

# CKSyncEngineZoneDeletionReason.purged

The owner of the iCloud account purged your app’s data using the Settings app.

```
case purged
```

## [Discussion](/documentation/cloudkit/cksyncenginezonedeletionreason/purged#Discussion)

## [See Also](/documentation/cloudkit/cksyncenginezonedeletionreason/purged#see-also)

### [Deletion reasons](/documentation/cloudkit/cksyncenginezonedeletionreason/purged#Deletion-reasons)

[`case deleted`](/documentation/cloudkit/cksyncenginezonedeletionreason/deleted)

Your app deleted the record zone.

[`case encryptedDataReset`](/documentation/cloudkit/cksyncenginezonedeletionreason/encrypteddatareset)

The owner of the iCloud account reset their encrypted data.

Current page is CKSyncEngineZoneDeletionReason.purged

# init(rawValue:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngineZoneDeletionReason](/documentation/cloudkit/cksyncenginezonedeletionreason)
-   init(rawValue:)

Initializer

# init(rawValue:)

```
init?(rawValue: Int)
```

Current page is init(rawValue:)

# modifications | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.FetchedDatabaseChanges](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges)
-   modifications

Instance Property

# modifications

The fetched record modifications.

```
let modifications: [CKDatabase.DatabaseChange.Modification]
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/modifications#see-also)

### [Accessing changes](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/modifications#Accessing-changes)

[`let deletions: [CKDatabase.DatabaseChange.Deletion]`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/deletions)

The fetched record deletions.

[`enum CKSyncEngineZoneDeletionReason`](/documentation/cloudkit/cksyncenginezonedeletionreason)

Describes the reason for a record zone deletion.

Current page is modifications

# CKSyncEngine.Event.didFetchChanges(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.didFetchChanges(\_:)

Case

# CKSyncEngine.Event.didFetchChanges(\_:)

An event that indicates the database fetch is done.

```
case didFetchChanges(CKSyncEngine.Event.DidFetchChanges)
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges\(_:\)#see-also)

### [Remote database changes](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges\(_:\)#Remote-database-changes)

[`case willFetchChanges(CKSyncEngine.Event.WillFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges\(_:\))

An event indicating an imminent database fetch.

[`struct WillFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges)

A type that provides information about an imminent database fetch.

[`case fetchedDatabaseChanges(CKSyncEngine.Event.FetchedDatabaseChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges\(_:\))

An event indicating there are fetched database changes to process.

[`struct FetchedDatabaseChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges)

A type that provides information about fetched database changes.

[`struct DidFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges)

A type that provides information about a finished database fetch.

Current page is CKSyncEngine.Event.didFetchChanges(\_:)

# CKSyncEngine.Event.DidFetchChanges

# CKSyncEngine.Event.DidFetchChanges | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.DidFetchChanges

Structure

# CKSyncEngine.Event.DidFetchChanges

A type that provides information about a finished database fetch.

```
struct DidFetchChanges
```

## [Topics](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges#topics)

### [Instance Properties](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges#Instance-Properties)

[`let context: CKSyncEngine.FetchChangesContext`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges/context)

## [Relationships](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges#relationships)

### [Conforms To](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges#conforms-to)

-   [`Copyable`](/documentation/Swift/Copyable)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges#see-also)

### [Remote database changes](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges#Remote-database-changes)

[`case willFetchChanges(CKSyncEngine.Event.WillFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges\(_:\))

An event indicating an imminent database fetch.

[`struct WillFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges)

A type that provides information about an imminent database fetch.

[`case fetchedDatabaseChanges(CKSyncEngine.Event.FetchedDatabaseChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges\(_:\))

An event indicating there are fetched database changes to process.

[`struct FetchedDatabaseChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges)

A type that provides information about fetched database changes.

[`case didFetchChanges(CKSyncEngine.Event.DidFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges\(_:\))

An event that indicates the database fetch is done.

Current page is CKSyncEngine.Event.DidFetchChanges

# context | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.DidFetchChanges](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges)
-   context

Instance Property

# context

```
let context: CKSyncEngine.FetchChangesContext
```

Current page is context

---
### Remote database changes
# CKSyncEngine.Event.willFetchChanges(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.willFetchChanges(\_:)

Case

# CKSyncEngine.Event.willFetchChanges(\_:)

An event indicating an imminent database fetch.

```
case willFetchChanges(CKSyncEngine.Event.WillFetchChanges)
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges\(_:\)#see-also)

### [Remote database changes](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges\(_:\)#Remote-database-changes)

[`struct WillFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges)

A type that provides information about an imminent database fetch.

[`case fetchedDatabaseChanges(CKSyncEngine.Event.FetchedDatabaseChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges\(_:\))

An event indicating there are fetched database changes to process.

[`struct FetchedDatabaseChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges)

A type that provides information about fetched database changes.

[`case didFetchChanges(CKSyncEngine.Event.DidFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges\(_:\))

An event that indicates the database fetch is done.

[`struct DidFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges)

A type that provides information about a finished database fetch.

Current page is CKSyncEngine.Event.willFetchChanges(\_:)

# CKSyncEngine.Event.WillFetchChanges | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.WillFetchChanges

Structure

# CKSyncEngine.Event.WillFetchChanges

A type that provides information about an imminent database fetch.

```
struct WillFetchChanges
```

## [Topics](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges#topics)

### [Instance Properties](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges#Instance-Properties)

[`let context: CKSyncEngine.FetchChangesContext`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges/context)

## [Relationships](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges#relationships)

### [Conforms To](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges#conforms-to)

-   [`Copyable`](/documentation/Swift/Copyable)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges#see-also)

### [Remote database changes](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges#Remote-database-changes)

[`case willFetchChanges(CKSyncEngine.Event.WillFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges\(_:\))

An event indicating an imminent database fetch.

[`case fetchedDatabaseChanges(CKSyncEngine.Event.FetchedDatabaseChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges\(_:\))

An event indicating there are fetched database changes to process.

[`struct FetchedDatabaseChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges)

A type that provides information about fetched database changes.

[`case didFetchChanges(CKSyncEngine.Event.DidFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges\(_:\))

An event that indicates the database fetch is done.

[`struct DidFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges)

A type that provides information about a finished database fetch.

Current page is CKSyncEngine.Event.WillFetchChanges

# context | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.WillFetchChanges](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges)
-   context

Instance Property

# context

```
let context: CKSyncEngine.FetchChangesContext
```

Current page is context

# CKSyncEngine.Event.fetchedDatabaseChanges(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.fetchedDatabaseChanges(\_:)

Case

# CKSyncEngine.Event.fetchedDatabaseChanges(\_:)

An event indicating there are fetched database changes to process.

```
case fetchedDatabaseChanges(CKSyncEngine.Event.FetchedDatabaseChanges)
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges\(_:\)#see-also)

### [Remote database changes](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges\(_:\)#Remote-database-changes)

[`case willFetchChanges(CKSyncEngine.Event.WillFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges\(_:\))

An event indicating an imminent database fetch.

[`struct WillFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges)

A type that provides information about an imminent database fetch.

[`struct FetchedDatabaseChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges)

A type that provides information about fetched database changes.

[`case didFetchChanges(CKSyncEngine.Event.DidFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges\(_:\))

An event that indicates the database fetch is done.

[`struct DidFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges)

A type that provides information about a finished database fetch.

Current page is CKSyncEngine.Event.fetchedDatabaseChanges(\_:)

# CKSyncEngine.Event.FetchedDatabaseChanges | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.FetchedDatabaseChanges

Structure

# CKSyncEngine.Event.FetchedDatabaseChanges

A type that provides information about fetched database changes.

```
struct FetchedDatabaseChanges
```

## [Overview](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges#overview)

## [Topics](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges#topics)

### [Accessing changes](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges#Accessing-changes)

[`let deletions: [CKDatabase.DatabaseChange.Deletion]`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/deletions)

The fetched record deletions.

[`enum CKSyncEngineZoneDeletionReason`](/documentation/cloudkit/cksyncenginezonedeletionreason)

Describes the reason for a record zone deletion.

[`let modifications: [CKDatabase.DatabaseChange.Modification]`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/modifications)

The fetched record modifications.

## [Relationships](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges#relationships)

### [Conforms To](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges#conforms-to)

-   [`Copyable`](/documentation/Swift/Copyable)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges#see-also)

### [Remote database changes](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges#Remote-database-changes)

[`case willFetchChanges(CKSyncEngine.Event.WillFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges\(_:\))

An event indicating an imminent database fetch.

[`struct WillFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges)

A type that provides information about an imminent database fetch.

[`case fetchedDatabaseChanges(CKSyncEngine.Event.FetchedDatabaseChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges\(_:\))

An event indicating there are fetched database changes to process.

[`case didFetchChanges(CKSyncEngine.Event.DidFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges\(_:\))

An event that indicates the database fetch is done.

[`struct DidFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges)

A type that provides information about a finished database fetch.

Current page is CKSyncEngine.Event.FetchedDatabaseChanges

# deletions | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.FetchedDatabaseChanges](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges)
-   deletions

Instance Property

# deletions

The fetched record deletions.

```
let deletions: [CKDatabase.DatabaseChange.Deletion]
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/deletions#see-also)

### [Accessing changes](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/deletions#Accessing-changes)

[`enum CKSyncEngineZoneDeletionReason`](/documentation/cloudkit/cksyncenginezonedeletionreason)

Describes the reason for a record zone deletion.

[`let modifications: [CKDatabase.DatabaseChange.Modification]`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/modifications)

The fetched record modifications.

Current page is deletions

# CKSyncEngineZoneDeletionReason | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKSyncEngineZoneDeletionReason

Enumeration

# CKSyncEngineZoneDeletionReason

Describes the reason for a record zone deletion.

```
enum CKSyncEngineZoneDeletionReason
```

## [Topics](/documentation/cloudkit/cksyncenginezonedeletionreason#topics)

### [Deletion reasons](/documentation/cloudkit/cksyncenginezonedeletionreason#Deletion-reasons)

[`case deleted`](/documentation/cloudkit/cksyncenginezonedeletionreason/deleted)

Your app deleted the record zone.

[`case encryptedDataReset`](/documentation/cloudkit/cksyncenginezonedeletionreason/encrypteddatareset)

The owner of the iCloud account reset their encrypted data.

[`case purged`](/documentation/cloudkit/cksyncenginezonedeletionreason/purged)

The owner of the iCloud account purged your app’s data using the Settings app.

### [Initializers](/documentation/cloudkit/cksyncenginezonedeletionreason#Initializers)

[`init?(rawValue: Int)`](/documentation/cloudkit/cksyncenginezonedeletionreason/init\(rawvalue:\))

## [Relationships](/documentation/cloudkit/cksyncenginezonedeletionreason#relationships)

### [Conforms To](/documentation/cloudkit/cksyncenginezonedeletionreason#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/cksyncenginezonedeletionreason#see-also)

### [Accessing changes](/documentation/cloudkit/cksyncenginezonedeletionreason#Accessing-changes)

[`let deletions: [CKDatabase.DatabaseChange.Deletion]`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/deletions)

The fetched record deletions.

[`let modifications: [CKDatabase.DatabaseChange.Modification]`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/modifications)

The fetched record modifications.

Current page is CKSyncEngineZoneDeletionReason

# CKSyncEngineZoneDeletionReason.deleted | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngineZoneDeletionReason](/documentation/cloudkit/cksyncenginezonedeletionreason)
-   CKSyncEngineZoneDeletionReason.deleted

Case

# CKSyncEngineZoneDeletionReason.deleted

Your app deleted the record zone.

```
case deleted
```

## [See Also](/documentation/cloudkit/cksyncenginezonedeletionreason/deleted#see-also)

### [Deletion reasons](/documentation/cloudkit/cksyncenginezonedeletionreason/deleted#Deletion-reasons)

[`case encryptedDataReset`](/documentation/cloudkit/cksyncenginezonedeletionreason/encrypteddatareset)

The owner of the iCloud account reset their encrypted data.

[`case purged`](/documentation/cloudkit/cksyncenginezonedeletionreason/purged)

The owner of the iCloud account purged your app’s data using the Settings app.

Current page is CKSyncEngineZoneDeletionReason.deleted

# CKSyncEngineZoneDeletionReason.encryptedDataReset | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngineZoneDeletionReason](/documentation/cloudkit/cksyncenginezonedeletionreason)
-   CKSyncEngineZoneDeletionReason.encryptedDataReset

Case

# CKSyncEngineZoneDeletionReason.encryptedDataReset

The owner of the iCloud account reset their encrypted data.

```
case encryptedDataReset
```

## [Discussion](/documentation/cloudkit/cksyncenginezonedeletionreason/encrypteddatareset#Discussion)

## [See Also](/documentation/cloudkit/cksyncenginezonedeletionreason/encrypteddatareset#see-also)

### [Deletion reasons](/documentation/cloudkit/cksyncenginezonedeletionreason/encrypteddatareset#Deletion-reasons)

[`case deleted`](/documentation/cloudkit/cksyncenginezonedeletionreason/deleted)

Your app deleted the record zone.

[`case purged`](/documentation/cloudkit/cksyncenginezonedeletionreason/purged)

The owner of the iCloud account purged your app’s data using the Settings app.

Current page is CKSyncEngineZoneDeletionReason.encryptedDataReset

# CKSyncEngineZoneDeletionReason.purged | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngineZoneDeletionReason](/documentation/cloudkit/cksyncenginezonedeletionreason)
-   CKSyncEngineZoneDeletionReason.purged

Case

# CKSyncEngineZoneDeletionReason.purged

The owner of the iCloud account purged your app’s data using the Settings app.

```
case purged
```

## [Discussion](/documentation/cloudkit/cksyncenginezonedeletionreason/purged#Discussion)

## [See Also](/documentation/cloudkit/cksyncenginezonedeletionreason/purged#see-also)

### [Deletion reasons](/documentation/cloudkit/cksyncenginezonedeletionreason/purged#Deletion-reasons)

[`case deleted`](/documentation/cloudkit/cksyncenginezonedeletionreason/deleted)

Your app deleted the record zone.

[`case encryptedDataReset`](/documentation/cloudkit/cksyncenginezonedeletionreason/encrypteddatareset)

The owner of the iCloud account reset their encrypted data.

Current page is CKSyncEngineZoneDeletionReason.purged

# modifications | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.FetchedDatabaseChanges](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges)
-   modifications

Instance Property

# modifications

The fetched record modifications.

```
let modifications: [CKDatabase.DatabaseChange.Modification]
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/modifications#see-also)

### [Accessing changes](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/modifications#Accessing-changes)

[`let deletions: [CKDatabase.DatabaseChange.Deletion]`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges/deletions)

The fetched record deletions.

[`enum CKSyncEngineZoneDeletionReason`](/documentation/cloudkit/cksyncenginezonedeletionreason)

Describes the reason for a record zone deletion.

Current page is modifications

# CKSyncEngine.Event.didFetchChanges(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.didFetchChanges(\_:)

Case

# CKSyncEngine.Event.didFetchChanges(\_:)

An event that indicates the database fetch is done.

```
case didFetchChanges(CKSyncEngine.Event.DidFetchChanges)
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges\(_:\)#see-also)

### [Remote database changes](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges\(_:\)#Remote-database-changes)

[`case willFetchChanges(CKSyncEngine.Event.WillFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges\(_:\))

An event indicating an imminent database fetch.

[`struct WillFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges)

A type that provides information about an imminent database fetch.

[`case fetchedDatabaseChanges(CKSyncEngine.Event.FetchedDatabaseChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges\(_:\))

An event indicating there are fetched database changes to process.

[`struct FetchedDatabaseChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges)

A type that provides information about fetched database changes.

[`struct DidFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges)

A type that provides information about a finished database fetch.

Current page is CKSyncEngine.Event.didFetchChanges(\_:)

# CKSyncEngine.Event.DidFetchChanges | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.DidFetchChanges

Structure

# CKSyncEngine.Event.DidFetchChanges

A type that provides information about a finished database fetch.

```
struct DidFetchChanges
```

## [Topics](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges#topics)

### [Instance Properties](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges#Instance-Properties)

[`let context: CKSyncEngine.FetchChangesContext`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges/context)

## [Relationships](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges#relationships)

### [Conforms To](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges#conforms-to)

-   [`Copyable`](/documentation/Swift/Copyable)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges#see-also)

### [Remote database changes](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges#Remote-database-changes)

[`case willFetchChanges(CKSyncEngine.Event.WillFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges\(_:\))

An event indicating an imminent database fetch.

[`struct WillFetchChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchchanges)

A type that provides information about an imminent database fetch.

[`case fetchedDatabaseChanges(CKSyncEngine.Event.FetchedDatabaseChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges\(_:\))

An event indicating there are fetched database changes to process.

[`struct FetchedDatabaseChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/fetcheddatabasechanges)

A type that provides information about fetched database changes.

[`case didFetchChanges(CKSyncEngine.Event.DidFetchChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges\(_:\))

An event that indicates the database fetch is done.

Current page is CKSyncEngine.Event.DidFetchChanges

# context | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.DidFetchChanges](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchchanges)
-   context

Instance Property

# context

```
let context: CKSyncEngine.FetchChangesContext
```

Current page is context

---
### Remote record zone changes

case willFetchRecordZoneChanges([`CKSyncEngine`](https://developer.apple.com/documentation/cloudkit/cksyncengine-5sie5).[`Event`](https://developer.apple.com/documentation/cloudkit/cksyncengine-5sie5/event).[`WillFetchRecordZoneChanges`](https://developer.apple.com/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges))

# CKSyncEngine.Event.WillFetchRecordZoneChanges | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.WillFetchRecordZoneChanges

Structure

# CKSyncEngine.Event.WillFetchRecordZoneChanges

A type that provides information about an imminent fetch of changes in a record zone.

```
struct WillFetchRecordZoneChanges
```

## [Topics](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges#topics)

### [Identifying the record zone](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges#Identifying-the-record-zone)

[`let zoneID: CKRecordZone.ID`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges/zoneid)

The associated record zone’s unique identifier.

## [Relationships](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges#relationships)

### [Conforms To](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges#conforms-to)

-   [`Copyable`](/documentation/Swift/Copyable)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges#see-also)

### [Remote record zone changes](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges#Remote-record-zone-changes)

[`case willFetchRecordZoneChanges(CKSyncEngine.Event.WillFetchRecordZoneChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges\(_:\))

An event indicating an imminent fetch of changes in a record zone.

[`case fetchedRecordZoneChanges(CKSyncEngine.Event.FetchedRecordZoneChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges\(_:\))

An event indicating there are fetched record zone changes to process.

[`struct FetchedRecordZoneChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges)

A type that provides information about fetched record zone changes.

[`case didFetchRecordZoneChanges(CKSyncEngine.Event.DidFetchRecordZoneChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges\(_:\))

An event that indicates the record zone fetch is done.

[`struct DidFetchRecordZoneChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges)

A type that provides information about a finished record zone fetch.

Current page is CKSyncEngine.Event.WillFetchRecordZoneChanges

# zoneID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.WillFetchRecordZoneChanges](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges)
-   zoneID

Instance Property

# zoneID

The associated record zone’s unique identifier.

```
let zoneID: CKRecordZone.ID
```

Current page is zoneID

# CKSyncEngine.Event.fetchedRecordZoneChanges(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.fetchedRecordZoneChanges(\_:)

Case

# CKSyncEngine.Event.fetchedRecordZoneChanges(\_:)

An event indicating there are fetched record zone changes to process.

```
case fetchedRecordZoneChanges(CKSyncEngine.Event.FetchedRecordZoneChanges)
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges\(_:\)#see-also)

### [Remote record zone changes](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges\(_:\)#Remote-record-zone-changes)

[`case willFetchRecordZoneChanges(CKSyncEngine.Event.WillFetchRecordZoneChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges\(_:\))

An event indicating an imminent fetch of changes in a record zone.

[`struct WillFetchRecordZoneChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges)

A type that provides information about an imminent fetch of changes in a record zone.

[`struct FetchedRecordZoneChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges)

A type that provides information about fetched record zone changes.

[`case didFetchRecordZoneChanges(CKSyncEngine.Event.DidFetchRecordZoneChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges\(_:\))

An event that indicates the record zone fetch is done.

[`struct DidFetchRecordZoneChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges)

A type that provides information about a finished record zone fetch.

Current page is CKSyncEngine.Event.fetchedRecordZoneChanges(\_:)

# CKSyncEngine.Event.FetchedRecordZoneChanges | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.FetchedRecordZoneChanges

Structure

# CKSyncEngine.Event.FetchedRecordZoneChanges

A type that provides information about fetched record zone changes.

```
struct FetchedRecordZoneChanges
```

## [Overview](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges#overview)

## [Topics](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges#topics)

### [Accessing changes](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges#Accessing-changes)

[`let deletions: [CKDatabase.RecordZoneChange.Deletion]`](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges/deletions)

The fetched record zone deletions.

[`let modifications: [CKDatabase.RecordZoneChange.Modification]`](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges/modifications)

The fetched record zone modifications.

## [Relationships](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges#relationships)

### [Conforms To](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges#conforms-to)

-   [`Copyable`](/documentation/Swift/Copyable)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges#see-also)

### [Remote record zone changes](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges#Remote-record-zone-changes)

[`case willFetchRecordZoneChanges(CKSyncEngine.Event.WillFetchRecordZoneChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges\(_:\))

An event indicating an imminent fetch of changes in a record zone.

[`struct WillFetchRecordZoneChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges)

A type that provides information about an imminent fetch of changes in a record zone.

[`case fetchedRecordZoneChanges(CKSyncEngine.Event.FetchedRecordZoneChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges\(_:\))

An event indicating there are fetched record zone changes to process.

[`case didFetchRecordZoneChanges(CKSyncEngine.Event.DidFetchRecordZoneChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges\(_:\))

An event that indicates the record zone fetch is done.

[`struct DidFetchRecordZoneChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges)

A type that provides information about a finished record zone fetch.

Current page is CKSyncEngine.Event.FetchedRecordZoneChanges

# deletions | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.FetchedRecordZoneChanges](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges)
-   deletions

Instance Property

# deletions

The fetched record zone deletions.

```
let deletions: [CKDatabase.RecordZoneChange.Deletion]
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges/deletions#see-also)

### [Accessing changes](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges/deletions#Accessing-changes)

[`let modifications: [CKDatabase.RecordZoneChange.Modification]`](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges/modifications)

The fetched record zone modifications.

Current page is deletions

# modifications | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   [CKSyncEngine.Event.FetchedRecordZoneChanges](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges)
-   modifications

Instance Property

# modifications

The fetched record zone modifications.

```
let modifications: [CKDatabase.RecordZoneChange.Modification]
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges/modifications#see-also)

### [Accessing changes](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges/modifications#Accessing-changes)

[`let deletions: [CKDatabase.RecordZoneChange.Deletion]`](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges/deletions)

The fetched record zone deletions.

Current page is modifications

# CKSyncEngine.Event.didFetchRecordZoneChanges(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.didFetchRecordZoneChanges(\_:)

Case

# CKSyncEngine.Event.didFetchRecordZoneChanges(\_:)

An event that indicates the record zone fetch is done.

```
case didFetchRecordZoneChanges(CKSyncEngine.Event.DidFetchRecordZoneChanges)
```

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges\(_:\)#see-also)

### [Remote record zone changes](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges\(_:\)#Remote-record-zone-changes)

[`case willFetchRecordZoneChanges(CKSyncEngine.Event.WillFetchRecordZoneChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges\(_:\))

An event indicating an imminent fetch of changes in a record zone.

[`struct WillFetchRecordZoneChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges)

A type that provides information about an imminent fetch of changes in a record zone.

[`case fetchedRecordZoneChanges(CKSyncEngine.Event.FetchedRecordZoneChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges\(_:\))

An event indicating there are fetched record zone changes to process.

[`struct FetchedRecordZoneChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges)

A type that provides information about fetched record zone changes.

[`struct DidFetchRecordZoneChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges)

A type that provides information about a finished record zone fetch.

Current page is CKSyncEngine.Event.didFetchRecordZoneChanges(\_:)

# CKSyncEngine.Event.DidFetchRecordZoneChanges | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKSyncEngine](/documentation/cloudkit/cksyncengine-5sie5)
-   [CKSyncEngine.Event](/documentation/cloudkit/cksyncengine-5sie5/event)
-   CKSyncEngine.Event.DidFetchRecordZoneChanges

Structure

# CKSyncEngine.Event.DidFetchRecordZoneChanges

A type that provides information about a finished record zone fetch.

```
struct DidFetchRecordZoneChanges
```

## [Topics](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges#topics)

### [Identifying the record zone](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges#Identifying-the-record-zone)

[`let zoneID: CKRecordZone.ID`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges/zoneid)

The associated record zone’s unique identifier.

### [Handling errors](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges#Handling-errors)

[`let error: CKError?`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges/error)

An error that describes the cause of a failed fetch operation.

## [Relationships](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges#relationships)

### [Conforms To](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges#conforms-to)

-   [`Copyable`](/documentation/Swift/Copyable)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges#see-also)

### [Remote record zone changes](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges#Remote-record-zone-changes)

[`case willFetchRecordZoneChanges(CKSyncEngine.Event.WillFetchRecordZoneChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges\(_:\))

An event indicating an imminent fetch of changes in a record zone.

[`struct WillFetchRecordZoneChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/willfetchrecordzonechanges)

A type that provides information about an imminent fetch of changes in a record zone.

[`case fetchedRecordZoneChanges(CKSyncEngine.Event.FetchedRecordZoneChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges\(_:\))

An event indicating there are fetched record zone changes to process.

[`struct FetchedRecordZoneChanges`](/documentation/cloudkit/cksyncengine-5sie5/event/fetchedrecordzonechanges)

A type that provides information about fetched record zone changes.

[`case didFetchRecordZoneChanges(CKSyncEngine.Event.DidFetchRecordZoneChanges)`](/documentation/cloudkit/cksyncengine-5sie5/event/didfetchrecordzonechanges\(_:\))

An event that indicates the record zone fetch is done.

Current page is CKSyncEngine.Event.DidFetchRecordZoneChanges

let zoneID: [`CKRecordZone`](https://developer.apple.com/documentation/cloudkit/ckrecordzone).[`ID`](https://developer.apple.com/documentation/cloudkit/ckrecordzone/id)

let error: [`CKError`](https://developer.apple.com/documentation/cloudkit/ckerror/code)?

---
  
Pending local changes

