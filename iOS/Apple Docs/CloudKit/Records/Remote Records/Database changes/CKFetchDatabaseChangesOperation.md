# CKFetchDatabaseChangesOperation | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKFetchDatabaseChangesOperation

Class

# CKFetchDatabaseChangesOperation

An operation that fetches database changes.

```
class CKFetchDatabaseChangesOperation
```

## [Overview](/documentation/cloudkit/ckfetchdatabasechangesoperation#overview)

Use this operation to fetch all record zone changes in a database. This includes new record zones, changed zones — including deleted or purged zones — and zones that contain record changes. When you create the operation, you provide a server change token, which is an opaque token that represents a specific point in the database’s history. CloudKit returns only the changes that occur after that point. For your app’s first fetch, or to refetch every change in the database’s history, use `nil` instead.

The operation yields new change tokens during its execution, and issues a final change token when it completes without error. The change tokens conform to [`NSSecureCoding`](/documentation/Foundation/NSSecureCoding) and are safe to cache on-disk. This operation’s tokens aren’t compatible with [`CKFetchRecordZoneChangesOperation`](/documentation/cloudkit/ckfetchrecordzonechangesoperation) so you should segregate them in your cache. Don’t infer any behavior or order from the tokens’ contents.

When your app launches for the first time, use this operation to fetch all the database’s changes. Cache the results on-device and use [`CKDatabaseSubscription`](/documentation/cloudkit/ckdatabasesubscription) to subscribe to future changes. Fetch those changes on receipt of the push notifications the subscription generates. It’s not necessary to perform a fetch each time your app launches, or to schedule fetches at regular intervals.

The operation calls [`recordZoneWithIDChangedBlock`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidchangedblock) for each zone that contains record changes. It also calls it for new and modified record zones. Store the IDs that CloudKit provides to this callback. Use those IDs with [`CKFetchRecordZoneChangesOperation`](/documentation/cloudkit/ckfetchrecordzonechangesoperation) to fetch the corresponding changes. There are similar callbacks for deleted and purged record zones.

To run the operation, add it to the corresponding database’s operation queue. The operation executes its callbacks on a private serial queue.

The following example shows how to create the operation, configure its callbacks, and execute it. For brevity, it omits the delete and purge callbacks.

```
// Create a fetch operation using the server change
// token from the previous fetch.
CKFetchDatabaseChangesOperation *operation =
    [[CKFetchDatabaseChangesOperation alloc]
     initWithPreviousServerChangeToken:token];
    
// Collect the IDs of the modified record zones.
operation.recordZoneWithIDChangedBlock = ^(CKRecordZoneID *recordZoneID) {
    [recordZoneIDs addObject:recordZoneID];
};
    
// Process any change tokens that CloudKit provides
// as the operation runs.
operation.changeTokenUpdatedBlock = ^(CKServerChangeToken *newToken) {
    tokenHandler(newToken);
};
    
// Store the final change token and pass the IDs of the
// modified record zones for further processing.
operation.fetchDatabaseChangesCompletionBlock =
    ^(CKServerChangeToken *newToken, BOOL more, NSError *error) {
    if (error) {
        // Handle the error.
    } else {
        tokenHandler(newToken);
        recordZonesHandler(recordZoneIDs);
    }
};
    
// Set an appropriate QoS and add the operation to the shared
// database's operation queue to execute it.
operation.qualityOfService = NSQualityOfServiceUtility;
[CKContainer.defaultContainer.sharedCloudDatabase addOperation:operation];
```

## [Topics](/documentation/cloudkit/ckfetchdatabasechangesoperation#topics)

### [Creating an Operation](/documentation/cloudkit/ckfetchdatabasechangesoperation#Creating-an-Operation)

[`convenience init(previousServerChangeToken: CKServerChangeToken?)`](/documentation/cloudkit/ckfetchdatabasechangesoperation/init\(previousserverchangetoken:\))

Creates an operation for fetching database changes.

[`init()`](/documentation/cloudkit/ckfetchdatabasechangesoperation/init\(\))

Creates an empty fetch database changes operation.

### [Configuring the Operation](/documentation/cloudkit/ckfetchdatabasechangesoperation#Configuring-the-Operation)

[`var fetchAllChanges: Bool`](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchallchanges)

A Boolean value that indicates whether to send repeated requests to the server.

[`var previousServerChangeToken: CKServerChangeToken?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/previousserverchangetoken)

The server change token.

[`var resultsLimit: Int`](/documentation/cloudkit/ckfetchdatabasechangesoperation/resultslimit)

The maximum number of results that the operation fetches.

### [Processing the Operation’s Results](/documentation/cloudkit/ckfetchdatabasechangesoperation#Processing-the-Operations-Results)

[`var recordZoneWithIDChangedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidchangedblock)

The closure to execute with a single record zone change.

[`var recordZoneWithIDWasDeletedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedblock)

The closure to execute when a record zone no longer exists.

[`var recordZoneWithIDWasDeletedDueToUserEncryptedDataResetBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedduetouserencrypteddataresetblock)

The closure to execute when a user-invoked account reset deletes a record zone.

[`var recordZoneWithIDWasPurgedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwaspurgedblock)

The closure to execute when CloudKit purges a record zone.

[`var changeTokenUpdatedBlock: ((CKServerChangeToken) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/changetokenupdatedblock)

The closure to execute when the change token updates.

[`var fetchDatabaseChangesCompletionBlock: ((CKServerChangeToken?, Bool, (any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchdatabasechangescompletionblock)

The closure to execute when the operation finishes.

Deprecated

### [Instance Properties](/documentation/cloudkit/ckfetchdatabasechangesoperation#Instance-Properties)

[`var fetchDatabaseChangesResultBlock: ((Result<(serverChangeToken: CKServerChangeToken, moreComing: Bool), any Error>) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchdatabasechangesresultblock)

## [Relationships](/documentation/cloudkit/ckfetchdatabasechangesoperation#relationships)

### [Inherits From](/documentation/cloudkit/ckfetchdatabasechangesoperation#inherits-from)

-   [`CKDatabaseOperation`](/documentation/cloudkit/ckdatabaseoperation)

### [Conforms To](/documentation/cloudkit/ckfetchdatabasechangesoperation#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/ckfetchdatabasechangesoperation#see-also)

### [Database Changes](/documentation/cloudkit/ckfetchdatabasechangesoperation#Database-Changes)

[`class CKDatabaseSubscription`](/documentation/cloudkit/ckdatabasesubscription)

A subscription that generates push notifications when CloudKit modifies records in a database.

[`class CKDatabaseNotification`](/documentation/cloudkit/ckdatabasenotification)

A notification that triggers when the contents of a database change.

Current page is CKFetchDatabaseChangesOperation

---
### Creating an Operation
# init(previousServerChangeToken:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchDatabaseChangesOperation](/documentation/cloudkit/ckfetchdatabasechangesoperation)
-   init(previousServerChangeToken:)

Initializer

# init(previousServerChangeToken:)

Creates an operation for fetching database changes.

```
convenience init(previousServerChangeToken: CKServerChangeToken?)
```

## [Parameters](/documentation/cloudkit/ckfetchdatabasechangesoperation/init\(previousserverchangetoken:\)#parameters)

`previousServerChangeToken`

The change token that CloudKit uses to determine which database changes to return.

## [Discussion](/documentation/cloudkit/ckfetchdatabasechangesoperation/init\(previousserverchangetoken:\)#Discussion)

After creating the operation, assign a handler to the [`fetchDatabaseChangesCompletionBlock`](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchdatabasechangescompletionblock) property so that you can process the operation’s results.

If this is your first fetch, or if you want to refetch all zones, pass `nil` for the change token. If you provide a change token from a previous [`CKFetchDatabaseChangesOperation`](/documentation/cloudkit/ckfetchdatabasechangesoperation), CloudKit returns only the zones with changes since that token. The per-database [`CKServerChangeToken`](/documentation/cloudkit/ckserverchangetoken) isn’t the same as the per-record zone [`CKServerChangeToken`](/documentation/cloudkit/ckserverchangetoken) from [`CKFetchRecordZoneChangesOperation`](/documentation/cloudkit/ckfetchrecordzonechangesoperation).

## [See Also](/documentation/cloudkit/ckfetchdatabasechangesoperation/init\(previousserverchangetoken:\)#see-also)

### [Creating an Operation](/documentation/cloudkit/ckfetchdatabasechangesoperation/init\(previousserverchangetoken:\)#Creating-an-Operation)

[`init()`](/documentation/cloudkit/ckfetchdatabasechangesoperation/init\(\))

Creates an empty fetch database changes operation.

Current page is init(previousServerChangeToken:)

# init() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchDatabaseChangesOperation](/documentation/cloudkit/ckfetchdatabasechangesoperation)
-   init()

Initializer

# init()

Creates an empty fetch database changes operation.

```
init()
```

## [See Also](/documentation/cloudkit/ckfetchdatabasechangesoperation/init\(\)#see-also)

### [Creating an Operation](/documentation/cloudkit/ckfetchdatabasechangesoperation/init\(\)#Creating-an-Operation)

[`convenience init(previousServerChangeToken: CKServerChangeToken?)`](/documentation/cloudkit/ckfetchdatabasechangesoperation/init\(previousserverchangetoken:\))

Creates an operation for fetching database changes.

Current page is init()

---
### Configuring the Operation

# fetchAllChanges | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchDatabaseChangesOperation](/documentation/cloudkit/ckfetchdatabasechangesoperation)
-   fetchAllChanges

Instance Property

# fetchAllChanges

A Boolean value that indicates whether to send repeated requests to the server.

```
var fetchAllChanges: Bool { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchallchanges#Discussion)

If [`true`](/documentation/swift/true), the operation sends repeat requests to the server until it fetches all changes. CloudKit executes the handler you set on the [`changeTokenUpdatedBlock`](/documentation/cloudkit/ckfetchdatabasechangesoperation/changetokenupdatedblock) property with a change token after each request.

The default value is [`true`](/documentation/swift/true).

## [See Also](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchallchanges#see-also)

### [Configuring the Operation](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchallchanges#Configuring-the-Operation)

[`var previousServerChangeToken: CKServerChangeToken?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/previousserverchangetoken)

The server change token.

[`var resultsLimit: Int`](/documentation/cloudkit/ckfetchdatabasechangesoperation/resultslimit)

The maximum number of results that the operation fetches.

Current page is fetchAllChanges

# previousServerChangeToken | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchDatabaseChangesOperation](/documentation/cloudkit/ckfetchdatabasechangesoperation)
-   previousServerChangeToken

Instance Property

# previousServerChangeToken

The server change token.

```
@NSCopying
var previousServerChangeToken: CKServerChangeToken? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchdatabasechangesoperation/previousserverchangetoken#Discussion)

Assign the token you receive from the [`fetchDatabaseChangesCompletionBlock`](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchdatabasechangescompletionblock) to this property. Doing so yields only the changes that occur after your most recent fetch operation. If you specify `nil` for this parameter, the operation fetches all changes.

## [See Also](/documentation/cloudkit/ckfetchdatabasechangesoperation/previousserverchangetoken#see-also)

### [Configuring the Operation](/documentation/cloudkit/ckfetchdatabasechangesoperation/previousserverchangetoken#Configuring-the-Operation)

[`var fetchAllChanges: Bool`](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchallchanges)

A Boolean value that indicates whether to send repeated requests to the server.

[`var resultsLimit: Int`](/documentation/cloudkit/ckfetchdatabasechangesoperation/resultslimit)

The maximum number of results that the operation fetches.

Current page is previousServerChangeToken

# resultsLimit | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchDatabaseChangesOperation](/documentation/cloudkit/ckfetchdatabasechangesoperation)
-   resultsLimit

Instance Property

# resultsLimit

The maximum number of results that the operation fetches.

```
var resultsLimit: Int { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchdatabasechangesoperation/resultslimit#Discussion)

Use this property to limit the number of changes this operation returns. When the operation reaches the limit, it updates the change token and returns it to indicate that more results are available.

## [See Also](/documentation/cloudkit/ckfetchdatabasechangesoperation/resultslimit#see-also)

### [Configuring the Operation](/documentation/cloudkit/ckfetchdatabasechangesoperation/resultslimit#Configuring-the-Operation)

[`var fetchAllChanges: Bool`](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchallchanges)

A Boolean value that indicates whether to send repeated requests to the server.

[`var previousServerChangeToken: CKServerChangeToken?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/previousserverchangetoken)

The server change token.

Current page is resultsLimit

---
  
Processing the Operation’s Results

# recordZoneWithIDChangedBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchDatabaseChangesOperation](/documentation/cloudkit/ckfetchdatabasechangesoperation)
-   recordZoneWithIDChangedBlock

Instance Property

# recordZoneWithIDChangedBlock

The closure to execute with a single record zone change.

```
var recordZoneWithIDChangedBlock: ((CKRecordZone.ID) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidchangedblock#Discussion)

The closure returns no value and takes the following parameter:

`zoneID`

The ID of the record zone that contains changes.

## [See Also](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidchangedblock#see-also)

### [Processing the Operation’s Results](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidchangedblock#Processing-the-Operations-Results)

[`var recordZoneWithIDWasDeletedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedblock)

The closure to execute when a record zone no longer exists.

[`var recordZoneWithIDWasDeletedDueToUserEncryptedDataResetBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedduetouserencrypteddataresetblock)

The closure to execute when a user-invoked account reset deletes a record zone.

[`var recordZoneWithIDWasPurgedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwaspurgedblock)

The closure to execute when CloudKit purges a record zone.

[`var changeTokenUpdatedBlock: ((CKServerChangeToken) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/changetokenupdatedblock)

The closure to execute when the change token updates.

[`var fetchDatabaseChangesCompletionBlock: ((CKServerChangeToken?, Bool, (any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchdatabasechangescompletionblock)

The closure to execute when the operation finishes.

Deprecated

Current page is recordZoneWithIDChangedBlock

# recordZoneWithIDWasDeletedBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchDatabaseChangesOperation](/documentation/cloudkit/ckfetchdatabasechangesoperation)
-   recordZoneWithIDWasDeletedBlock

Instance Property

# recordZoneWithIDWasDeletedBlock

The closure to execute when a record zone no longer exists.

```
var recordZoneWithIDWasDeletedBlock: ((CKRecordZone.ID) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedblock#Discussion)

The closure returns no value and takes the following parameter:

`zoneID`

The deleted record zone’s ID.

## [See Also](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedblock#see-also)

### [Processing the Operation’s Results](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedblock#Processing-the-Operations-Results)

[`var recordZoneWithIDChangedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidchangedblock)

The closure to execute with a single record zone change.

[`var recordZoneWithIDWasDeletedDueToUserEncryptedDataResetBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedduetouserencrypteddataresetblock)

The closure to execute when a user-invoked account reset deletes a record zone.

[`var recordZoneWithIDWasPurgedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwaspurgedblock)

The closure to execute when CloudKit purges a record zone.

[`var changeTokenUpdatedBlock: ((CKServerChangeToken) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/changetokenupdatedblock)

The closure to execute when the change token updates.

[`var fetchDatabaseChangesCompletionBlock: ((CKServerChangeToken?, Bool, (any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchdatabasechangescompletionblock)

The closure to execute when the operation finishes.

Deprecated

Current page is recordZoneWithIDWasDeletedBlock

# recordZoneWithIDWasDeletedDueToUserEncryptedDataResetBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchDatabaseChangesOperation](/documentation/cloudkit/ckfetchdatabasechangesoperation)
-   recordZoneWithIDWasDeletedDueToUserEncryptedDataResetBlock

Instance Property

# recordZoneWithIDWasDeletedDueToUserEncryptedDataResetBlock

The closure to execute when a user-invoked account reset deletes a record zone.

```
var recordZoneWithIDWasDeletedDueToUserEncryptedDataResetBlock: ((CKRecordZone.ID) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedduetouserencrypteddataresetblock#Discussion)

The closure returns no value and takes a single parameter: the deleted record zone’s ID.

The operation executes this closure, instead of [`recordZoneWithIDWasDeletedBlock`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedblock), after a user action causes CloudKit to delete the record zone. Reupload any locally cached data to iCloud to minimize data loss.

## [See Also](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedduetouserencrypteddataresetblock#see-also)

### [Processing the Operation’s Results](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedduetouserencrypteddataresetblock#Processing-the-Operations-Results)

[`var recordZoneWithIDChangedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidchangedblock)

The closure to execute with a single record zone change.

[`var recordZoneWithIDWasDeletedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedblock)

The closure to execute when a record zone no longer exists.

[`var recordZoneWithIDWasPurgedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwaspurgedblock)

The closure to execute when CloudKit purges a record zone.

[`var changeTokenUpdatedBlock: ((CKServerChangeToken) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/changetokenupdatedblock)

The closure to execute when the change token updates.

[`var fetchDatabaseChangesCompletionBlock: ((CKServerChangeToken?, Bool, (any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchdatabasechangescompletionblock)

The closure to execute when the operation finishes.

Deprecated

Current page is recordZoneWithIDWasDeletedDueToUserEncryptedDataResetBlock

# recordZoneWithIDWasPurgedBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchDatabaseChangesOperation](/documentation/cloudkit/ckfetchdatabasechangesoperation)
-   recordZoneWithIDWasPurgedBlock

Instance Property

# recordZoneWithIDWasPurgedBlock

The closure to execute when CloudKit purges a record zone.

```
var recordZoneWithIDWasPurgedBlock: ((CKRecordZone.ID) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwaspurgedblock#Discussion)

The closure returns no value and takes the following parameter:

`zoneID`

The purged record zone’s ID.

## [See Also](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwaspurgedblock#see-also)

### [Processing the Operation’s Results](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwaspurgedblock#Processing-the-Operations-Results)

[`var recordZoneWithIDChangedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidchangedblock)

The closure to execute with a single record zone change.

[`var recordZoneWithIDWasDeletedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedblock)

The closure to execute when a record zone no longer exists.

[`var recordZoneWithIDWasDeletedDueToUserEncryptedDataResetBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedduetouserencrypteddataresetblock)

The closure to execute when a user-invoked account reset deletes a record zone.

[`var changeTokenUpdatedBlock: ((CKServerChangeToken) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/changetokenupdatedblock)

The closure to execute when the change token updates.

[`var fetchDatabaseChangesCompletionBlock: ((CKServerChangeToken?, Bool, (any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchdatabasechangescompletionblock)

The closure to execute when the operation finishes.

Deprecated

Current page is recordZoneWithIDWasPurgedBlock

# changeTokenUpdatedBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchDatabaseChangesOperation](/documentation/cloudkit/ckfetchdatabasechangesoperation)
-   changeTokenUpdatedBlock

Instance Property

# changeTokenUpdatedBlock

The closure to execute when the change token updates.

```
var changeTokenUpdatedBlock: ((CKServerChangeToken) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchdatabasechangesoperation/changetokenupdatedblock#Discussion)

The closure executes periodically, and provides a new change token so that you don’t need to refetch previously fetched record zone changes in a subsequent operation.

## [See Also](/documentation/cloudkit/ckfetchdatabasechangesoperation/changetokenupdatedblock#see-also)

### [Processing the Operation’s Results](/documentation/cloudkit/ckfetchdatabasechangesoperation/changetokenupdatedblock#Processing-the-Operations-Results)

[`var recordZoneWithIDChangedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidchangedblock)

The closure to execute with a single record zone change.

[`var recordZoneWithIDWasDeletedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedblock)

The closure to execute when a record zone no longer exists.

[`var recordZoneWithIDWasDeletedDueToUserEncryptedDataResetBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwasdeletedduetouserencrypteddataresetblock)

The closure to execute when a user-invoked account reset deletes a record zone.

[`var recordZoneWithIDWasPurgedBlock: ((CKRecordZone.ID) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/recordzonewithidwaspurgedblock)

The closure to execute when CloudKit purges a record zone.

[`var fetchDatabaseChangesCompletionBlock: ((CKServerChangeToken?, Bool, (any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchdatabasechangesoperation/fetchdatabasechangescompletionblock)

The closure to execute when the operation finishes.

Deprecated

Current page is changeTokenUpdatedBlock

---
  
Instance Properties

# fetchDatabaseChangesResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchDatabaseChangesOperation](/documentation/cloudkit/ckfetchdatabasechangesoperation)
-   fetchDatabaseChangesResultBlock

Instance Property

# fetchDatabaseChangesResultBlock

```
var fetchDatabaseChangesResultBlock: ((Result<(serverChangeToken: CKServerChangeToken, moreComing: Bool), any Error>) -> Void)? { get set }
```

Current page is fetchDatabaseChangesResultBlock
