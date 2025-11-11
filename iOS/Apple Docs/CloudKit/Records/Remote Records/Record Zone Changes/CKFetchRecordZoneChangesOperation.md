# CKFetchRecordZoneChangesOperation | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKFetchRecordZoneChangesOperation

Class

# CKFetchRecordZoneChangesOperation

An operation that fetches record zone changes.

```
class CKFetchRecordZoneChangesOperation
```

## [Mentioned in](/documentation/cloudkit/ckfetchrecordzonechangesoperation#mentions)

[

Encrypting User Data](/documentation/cloudkit/encrypting-user-data)

## [Overview](/documentation/cloudkit/ckfetchrecordzonechangesoperation#overview)

Use this operation to fetch record changes in one or more record zones, such as those that occur during record creation, modification, and deletion. You provide a configuration object for each record zone to query for changes. The configuration contains a server change token, which is an opaque pointer to a specific change in the zone’s history. CloudKit returns only the changes that occur after that point. For the first time you fetch a record zone’s changes, or to refetch all changes in a zone’s history, use `nil` instead.

CloudKit processes the record zones in succession, and returns the changes for each zone in batches. Each batch yields a new change token. If all batches return without error, the operation issues a final change token for that zone. The change tokens conform to [`NSSecureCoding`](/documentation/Foundation/NSSecureCoding) and are safe to cache on-disk. This operation’s tokens aren’t compatible with [`CKFetchDatabaseChangesOperation`](/documentation/cloudkit/ckfetchdatabasechangesoperation) so you should segregate them in your app’s cache. Don’t infer behavior or order from the tokens’ contents.

If you create record zones in the private database, fetch all changes the first time the app launches. Cache the results on-device and use [`CKRecordZoneSubscription`](/documentation/cloudkit/ckrecordzonesubscription) to subscribe to future changes. Fetch those changes on receipt of the push notifications the subscription generates. If you use the shared database, subscribe to changes with [`CKDatabaseSubscription`](/documentation/cloudkit/ckdatabasesubscription) instead. When a user participates in sharing, CloudKit adds and removes record zones. This means you don’t know in advance which zones exist in the shared database. Use [`CKFetchDatabaseChangesOperation`](/documentation/cloudkit/ckfetchdatabasechangesoperation) to fetch shared record zones on receipt of the subscription’s push notifications. Then fetch the changes in those zones using this operation. Regardless of which database you use, it’s not necessary to perform fetches each time your app launches, or to schedule fetches at regular intervals.

To run the operation, add it to the corresponding database’s operation queue. The operation executes its callbacks on a private serial queue.

The following example demonstrates how to create the operation, configure its callbacks, and execute it. For brevity, it omits the delete and operation completion callbacks.

```
// Create a dictionary that maps a record zone ID to its
// corresponding zone configuration. The configuration
// contains the server change token from the most recent 
// fetch of that record zone.
NSMutableDictionary *configurations = [NSMutableDictionary new];
for (CKRecordZoneID *recordZoneID in recordZoneIDs) {
    CKFetchRecordZoneChangesConfiguration *config = 
        [CKFetchRecordZoneChangesConfiguration new];
    config.previousServerChangeToken = [tokenCache objectForKey:recordZoneID];
    [configurations setObject:config forKey:recordZoneID];
}


// Create a fetch operation with an array of record zone IDs
// and the zone configuration mapping dictionary. 
CKFetchRecordZoneChangesOperation *operation = 
    [[CKFetchRecordZoneChangesOperation alloc] 
     initWithRecordZoneIDs:recordZoneIDs 
     configurationsByRecordZoneID:configurations];


// Process each changed record as CloudKit returns it.   
operation.recordChangedBlock = ^(CKRecord *record) {
    recordHandler(record);
};


// Cache the change tokens that CloudKit provides as
// the operation runs.
operation.recordZoneChangeTokensUpdatedBlock = ^(CKRecordZoneID *recordZoneID, 
                                                 CKServerChangeToken *token, 
                                                 NSData *data) {
    [tokenCache setObject:token forKey:recordZoneID];
};


// If the fetch for the current record zone completes
// successfully, cache the final change token.    
operation.recordZoneFetchCompletionBlock = ^(CKRecordZoneID *recordZoneID, 
                                             CKServerChangeToken *token, 
                                             NSData *data, BOOL more, 
                                             NSError *error) {
    if (error) {
        // Handle the error.
    } else {
        [tokenCache setObject:token forKey:recordZoneID];
    }
};


// Set an appropriate QoS and add the operation to the shared
// database's operation queue to execute it.
operation.qualityOfService = NSQualityOfServiceUtility;
[CKContainer.defaultContainer.sharedCloudDatabase addOperation:operation];
```

## [Topics](/documentation/cloudkit/ckfetchrecordzonechangesoperation#topics)

### [Creating a Zone Change Operation](/documentation/cloudkit/ckfetchrecordzonechangesoperation#Creating-a-Zone-Change-Operation)

[`convenience init(recordZoneIDs: [CKRecordZone.ID]?, configurationsByRecordZoneID: [CKRecordZone.ID : CKFetchRecordZoneChangesOperation.ZoneConfiguration]?)`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/init\(recordzoneids:configurationsbyrecordzoneid:\))

Creates an operation for fetching record zone changes.

[`init()`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/init\(\))

Creates an empty fetch record zone changes operation.

### [Configuring the Zone Change Operation](/documentation/cloudkit/ckfetchrecordzonechangesoperation#Configuring-the-Zone-Change-Operation)

[`var configurationsByRecordZoneID: [CKRecordZone.ID : CKFetchRecordZoneChangesOperation.ZoneConfiguration]?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/configurationsbyrecordzoneid)

A dictionary of configurations for fetching change operations by zone identifier.

[`class ZoneConfiguration`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration)

A configuration object that describes the information to fetch from a record zone.

[`var fetchAllChanges: Bool`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/fetchallchanges)

A Boolean value that indicates whether to send repeated requests to the server.

[`var recordZoneIDs: [CKRecordZone.ID]?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzoneids)

The IDs of the record zones that contain the records to fetch.

### [Processing the Zone Change Operation Results](/documentation/cloudkit/ckfetchrecordzonechangesoperation#Processing-the-Zone-Change-Operation-Results)

[`var recordChangedBlock: ((CKRecord) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordchangedblock)

The closure to execute with the contents of a changed record.

Deprecated

[`var recordWithIDWasDeletedBlock: ((CKRecord.ID, CKRecord.RecordType) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordwithidwasdeletedblock-3z14c)

The closure to execute when a record no longer exists.

[`var recordZoneChangeTokensUpdatedBlock: ((CKRecordZone.ID, CKServerChangeToken?, Data?) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzonechangetokensupdatedblock)

The closure to execute when the change token updates.

[`var recordZoneFetchCompletionBlock: ((CKRecordZone.ID, CKServerChangeToken?, Data?, Bool, (any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzonefetchcompletionblock)

The closure to execute when a record zone’s fetch finishes.

Deprecated

[`var fetchRecordZoneChangesCompletionBlock: (((any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/fetchrecordzonechangescompletionblock)

The closure to execute when the operation finishes.

Deprecated

### [Deprecated](/documentation/cloudkit/ckfetchrecordzonechangesoperation#Deprecated)

[

API Reference

Deprecated Symbols](/documentation/cloudkit/ckfetchrecordzonechangesoperation-deprecated-symbols)

Review unsupported symbols and their replacements.

### [Instance Properties](/documentation/cloudkit/ckfetchrecordzonechangesoperation#Instance-Properties)

[`var fetchRecordZoneChangesResultBlock: ((Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/fetchrecordzonechangesresultblock)

[`var recordWasChangedBlock: ((CKRecord.ID, Result<CKRecord, any Error>) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordwaschangedblock-x5bw)

[`var recordZoneFetchResultBlock: ((CKRecordZone.ID, Result<(serverChangeToken: CKServerChangeToken, clientChangeTokenData: Data?, moreComing: Bool), any Error>) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzonefetchresultblock)

## [Relationships](/documentation/cloudkit/ckfetchrecordzonechangesoperation#relationships)

### [Inherits From](/documentation/cloudkit/ckfetchrecordzonechangesoperation#inherits-from)

-   [`CKDatabaseOperation`](/documentation/cloudkit/ckdatabaseoperation)

### [Conforms To](/documentation/cloudkit/ckfetchrecordzonechangesoperation#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/ckfetchrecordzonechangesoperation#see-also)

### [Record Zone Changes](/documentation/cloudkit/ckfetchrecordzonechangesoperation#Record-Zone-Changes)

[`class CKRecordZoneSubscription`](/documentation/cloudkit/ckrecordzonesubscription)

A subscription that generates push notifications when CloudKit modifies records in a specific record zone.

[`class CKRecordZoneNotification`](/documentation/cloudkit/ckrecordzonenotification)

A notification that triggers when the contents of a record zone change.

Current page is CKFetchRecordZoneChangesOperation

---
  
Creating a Zone Change Operation

# init(recordZoneIDs:configurationsByRecordZoneID:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZoneChangesOperation](/documentation/cloudkit/ckfetchrecordzonechangesoperation)
-   init(recordZoneIDs:configurationsByRecordZoneID:)

Initializer

# init(recordZoneIDs:configurationsByRecordZoneID:)

Creates an operation for fetching record zone changes.

```
convenience init(
    recordZoneIDs: [CKRecordZone.ID]? = nil,
    configurationsByRecordZoneID: [CKRecordZone.ID : CKFetchRecordZoneChangesOperation.ZoneConfiguration]? = nil
)
```

## [Parameters](/documentation/cloudkit/ckfetchrecordzonechangesoperation/init\(recordzoneids:configurationsbyrecordzoneid:\)#parameters)

`recordZoneIDs`

The IDs of the record zones that you want to query for changes. You can specify `nil` for this parameter.

`configurationsByRecordZoneID`

A dictionary that maps record zone IDs to their corresponding configurations. You can specify `nil` for this parameter.

## [Discussion](/documentation/cloudkit/ckfetchrecordzonechangesoperation/init\(recordzoneids:configurationsbyrecordzoneid:\)#Discussion)

CloudKit configures the operation for retrieving all of the record zones that you specify. If you want to reduce the amount of data that CloudKit returns, provide zone configurations for each record zone.

## [See Also](/documentation/cloudkit/ckfetchrecordzonechangesoperation/init\(recordzoneids:configurationsbyrecordzoneid:\)#see-also)

### [Creating a Zone Change Operation](/documentation/cloudkit/ckfetchrecordzonechangesoperation/init\(recordzoneids:configurationsbyrecordzoneid:\)#Creating-a-Zone-Change-Operation)

[`init()`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/init\(\))

Creates an empty fetch record zone changes operation.

Current page is init(recordZoneIDs:configurationsByRecordZoneID:)

# init() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZoneChangesOperation](/documentation/cloudkit/ckfetchrecordzonechangesoperation)
-   init()

Initializer

# init()

Creates an empty fetch record zone changes operation.

```
init()
```

## [See Also](/documentation/cloudkit/ckfetchrecordzonechangesoperation/init\(\)#see-also)

### [Creating a Zone Change Operation](/documentation/cloudkit/ckfetchrecordzonechangesoperation/init\(\)#Creating-a-Zone-Change-Operation)

[`convenience init(recordZoneIDs: [CKRecordZone.ID]?, configurationsByRecordZoneID: [CKRecordZone.ID : CKFetchRecordZoneChangesOperation.ZoneConfiguration]?)`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/init\(recordzoneids:configurationsbyrecordzoneid:\))

Creates an operation for fetching record zone changes.

Current page is init()

---
### Configuring the Zone Change Operation

# configurationsByRecordZoneID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZoneChangesOperation](/documentation/cloudkit/ckfetchrecordzonechangesoperation)
-   configurationsByRecordZoneID

Instance Property

# configurationsByRecordZoneID

A dictionary of configurations for fetching change operations by zone identifier.

```
var configurationsByRecordZoneID: [CKRecordZone.ID : CKFetchRecordZoneChangesOperation.ZoneConfiguration]? { get set }
```

## [See Also](/documentation/cloudkit/ckfetchrecordzonechangesoperation/configurationsbyrecordzoneid#see-also)

### [Configuring the Zone Change Operation](/documentation/cloudkit/ckfetchrecordzonechangesoperation/configurationsbyrecordzoneid#Configuring-the-Zone-Change-Operation)

[`class ZoneConfiguration`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration)

A configuration object that describes the information to fetch from a record zone.

[`var fetchAllChanges: Bool`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/fetchallchanges)

A Boolean value that indicates whether to send repeated requests to the server.

[`var recordZoneIDs: [CKRecordZone.ID]?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzoneids)

The IDs of the record zones that contain the records to fetch.

Current page is configurationsByRecordZoneID

----
# configurationsByRecordZoneID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZoneChangesOperation](/documentation/cloudkit/ckfetchrecordzonechangesoperation)
-   configurationsByRecordZoneID

Instance Property

# configurationsByRecordZoneID

A dictionary of configurations for fetching change operations by zone identifier.

```
var configurationsByRecordZoneID: [CKRecordZone.ID : CKFetchRecordZoneChangesOperation.ZoneConfiguration]? { get set }
```

## [See Also](/documentation/cloudkit/ckfetchrecordzonechangesoperation/configurationsbyrecordzoneid#see-also)

### [Configuring the Zone Change Operation](/documentation/cloudkit/ckfetchrecordzonechangesoperation/configurationsbyrecordzoneid#Configuring-the-Zone-Change-Operation)

[`class ZoneConfiguration`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration)

A configuration object that describes the information to fetch from a record zone.

[`var fetchAllChanges: Bool`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/fetchallchanges)

A Boolean value that indicates whether to send repeated requests to the server.

[`var recordZoneIDs: [CKRecordZone.ID]?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzoneids)

The IDs of the record zones that contain the records to fetch.

Current page is configurationsByRecordZoneID

  
Creating a Zone Change Configuration

# init(previousServerChangeToken:resultsLimit:desiredKeys:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZoneChangesOperation](/documentation/cloudkit/ckfetchrecordzonechangesoperation)
-   [CKFetchRecordZoneChangesOperation.ZoneConfiguration](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration)
-   init(previousServerChangeToken:resultsLimit:desiredKeys:)

Initializer

# init(previousServerChangeToken:resultsLimit:desiredKeys:)

Creates a zone configuration with the desired keys and a result limit for updates.

```
convenience init(
    previousServerChangeToken: CKServerChangeToken? = nil,
    resultsLimit: Int? = nil,
    desiredKeys: [CKRecord.FieldKey]? = nil
)
```

## [Parameters](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration/init\(previousserverchangetoken:resultslimit:desiredkeys:\)#parameters)

`previousServerChangeToken`

A CloudKit server change token.

`resultsLimit`

The maximum number of updated records that CloudKit retrieves with an update operation. The default is 0.

`desiredKeys`

An array of the desired record keys CloudKit retrieves with updates.

Current page is init(previousServerChangeToken:resultsLimit:desiredKeys:)

  
Accessing a Zone Change Configuration

# previousServerChangeToken | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZoneChangesOperation](/documentation/cloudkit/ckfetchrecordzonechangesoperation)
-   [CKFetchRecordZoneChangesOperation.ZoneConfiguration](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration)
-   previousServerChangeToken

Instance Property

# previousServerChangeToken

The server change token.

```
@NSCopying
var previousServerChangeToken: CKServerChangeToken? { get set }
```

## [See Also](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration/previousserverchangetoken#see-also)

### [Accessing a Zone Change Configuration](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration/previousserverchangetoken#Accessing-a-Zone-Change-Configuration)

[`var resultsLimit: Int`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration/resultslimit)

The maximum number of records that CloudKit retrieves when fetching zone changes.

[`var desiredKeys: [CKRecord.FieldKey]?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration/desiredkeys)

An array of the record keys to retrieve.

Current page is previousServerChangeToken

# resultsLimit | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZoneChangesOperation](/documentation/cloudkit/ckfetchrecordzonechangesoperation)
-   [CKFetchRecordZoneChangesOperation.ZoneConfiguration](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration)
-   resultsLimit

Instance Property

# resultsLimit

The maximum number of records that CloudKit retrieves when fetching zone changes.

```
var resultsLimit: Int { get set }
```

## [See Also](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration/resultslimit#see-also)

### [Accessing a Zone Change Configuration](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration/resultslimit#Accessing-a-Zone-Change-Configuration)

[`var previousServerChangeToken: CKServerChangeToken?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration/previousserverchangetoken)

The server change token.

[`var desiredKeys: [CKRecord.FieldKey]?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration/desiredkeys)

An array of the record keys to retrieve.

Current page is resultsLimit

# desiredKeys | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZoneChangesOperation](/documentation/cloudkit/ckfetchrecordzonechangesoperation)
-   [CKFetchRecordZoneChangesOperation.ZoneConfiguration](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration)
-   desiredKeys

Instance Property

# desiredKeys

An array of the record keys to retrieve.

```
var desiredKeys: [CKRecord.FieldKey]? { get set }
```

## [See Also](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration/desiredkeys#see-also)

### [Accessing a Zone Change Configuration](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration/desiredkeys#Accessing-a-Zone-Change-Configuration)

[`var previousServerChangeToken: CKServerChangeToken?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration/previousserverchangetoken)

The server change token.

[`var resultsLimit: Int`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration/resultslimit)

The maximum number of records that CloudKit retrieves when fetching zone changes.

Current page is desiredKeys

---
# fetchAllChanges | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZoneChangesOperation](/documentation/cloudkit/ckfetchrecordzonechangesoperation)
-   fetchAllChanges

Instance Property

# fetchAllChanges

A Boolean value that indicates whether to send repeated requests to the server.

```
var fetchAllChanges: Bool { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchrecordzonechangesoperation/fetchallchanges#Discussion)

If [`true`](/documentation/swift/true), the operation sends repeat requests to the server until it fetches all changes. CloudKit executes the handler you set on the [`recordZoneFetchCompletionBlock`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzonefetchcompletionblock) property with a change token after each request.

The default value is [`true`](/documentation/swift/true).

## [See Also](/documentation/cloudkit/ckfetchrecordzonechangesoperation/fetchallchanges#see-also)

### [Configuring the Zone Change Operation](/documentation/cloudkit/ckfetchrecordzonechangesoperation/fetchallchanges#Configuring-the-Zone-Change-Operation)

[`var configurationsByRecordZoneID: [CKRecordZone.ID : CKFetchRecordZoneChangesOperation.ZoneConfiguration]?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/configurationsbyrecordzoneid)

A dictionary of configurations for fetching change operations by zone identifier.

[`class ZoneConfiguration`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration)

A configuration object that describes the information to fetch from a record zone.

[`var recordZoneIDs: [CKRecordZone.ID]?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzoneids)

The IDs of the record zones that contain the records to fetch.

Current page is fetchAllChanges

# recordZoneIDs | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZoneChangesOperation](/documentation/cloudkit/ckfetchrecordzonechangesoperation)
-   recordZoneIDs

Instance Property

# recordZoneIDs

The IDs of the record zones that contain the records to fetch.

```
var recordZoneIDs: [CKRecordZone.ID]? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzoneids#Discussion)

Typically, you set the value of this property when you create the operation. If you intend to change the record zone IDs, update the value before you execute the operation or submit it to a queue.

## [See Also](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzoneids#see-also)

### [Configuring the Zone Change Operation](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzoneids#Configuring-the-Zone-Change-Operation)

[`var configurationsByRecordZoneID: [CKRecordZone.ID : CKFetchRecordZoneChangesOperation.ZoneConfiguration]?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/configurationsbyrecordzoneid)

A dictionary of configurations for fetching change operations by zone identifier.

[`class ZoneConfiguration`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/zoneconfiguration)

A configuration object that describes the information to fetch from a record zone.

[`var fetchAllChanges: Bool`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/fetchallchanges)

A Boolean value that indicates whether to send repeated requests to the server.

Current page is recordZoneIDs

---
  
Processing the Zone Change Operation Results

# recordWithIDWasDeletedBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZoneChangesOperation](/documentation/cloudkit/ckfetchrecordzonechangesoperation)
-   recordWithIDWasDeletedBlock

Instance Property

# recordWithIDWasDeletedBlock

The closure to execute when a record no longer exists.

```
var recordWithIDWasDeletedBlock: ((CKRecord.ID, CKRecord.RecordType) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordwithidwasdeletedblock-3z14c#Discussion)

The closure returns no value and takes the following parameters:

-   The deleted record’s ID.
    
-   The deleted record’s type.
    

The operation executes this closure once for each record the server deletes after the previous change token. Each time the closure executes, it executes serially with respect to the other closures of the operation. If there aren’t any record deletions, this closure doesn’t execute.

Set this property before you execute the operation or submit it to a queue.

## [See Also](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordwithidwasdeletedblock-3z14c#see-also)

### [Processing the Zone Change Operation Results](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordwithidwasdeletedblock-3z14c#Processing-the-Zone-Change-Operation-Results)

[`var recordChangedBlock: ((CKRecord) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordchangedblock)

The closure to execute with the contents of a changed record.

Deprecated

[`var recordZoneChangeTokensUpdatedBlock: ((CKRecordZone.ID, CKServerChangeToken?, Data?) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzonechangetokensupdatedblock)

The closure to execute when the change token updates.

[`var recordZoneFetchCompletionBlock: ((CKRecordZone.ID, CKServerChangeToken?, Data?, Bool, (any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzonefetchcompletionblock)

The closure to execute when a record zone’s fetch finishes.

Deprecated

[`var fetchRecordZoneChangesCompletionBlock: (((any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/fetchrecordzonechangescompletionblock)

The closure to execute when the operation finishes.

Deprecated

Current page is recordWithIDWasDeletedBlock

# recordZoneChangeTokensUpdatedBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZoneChangesOperation](/documentation/cloudkit/ckfetchrecordzonechangesoperation)
-   recordZoneChangeTokensUpdatedBlock

Instance Property

# recordZoneChangeTokensUpdatedBlock

The closure to execute when the change token updates.

```
var recordZoneChangeTokensUpdatedBlock: ((CKRecordZone.ID, CKServerChangeToken?, Data?) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzonechangetokensupdatedblock#Discussion)

The closure returns no value and takes the following parameters:

-   The record zone’s ID.
    
-   The new change token from the server. You can store this token locally and use it during subsequent fetch operations to limit the results to records that change after this operation executes.
    
-   The most recent client change token from the device. If the change token isn’t the most recent change token you provided, the server might not have received the associated changes.
    

The operation executes this closure once for each record zone. Each time the closure executes, it executes serially with respect to the other blocks of the operation.

Set this property before you execute the operation or submit it to a queue.

## [See Also](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzonechangetokensupdatedblock#see-also)

### [Processing the Zone Change Operation Results](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzonechangetokensupdatedblock#Processing-the-Zone-Change-Operation-Results)

[`var recordChangedBlock: ((CKRecord) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordchangedblock)

The closure to execute with the contents of a changed record.

Deprecated

[`var recordWithIDWasDeletedBlock: ((CKRecord.ID, CKRecord.RecordType) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordwithidwasdeletedblock-3z14c)

The closure to execute when a record no longer exists.

[`var recordZoneFetchCompletionBlock: ((CKRecordZone.ID, CKServerChangeToken?, Data?, Bool, (any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/recordzonefetchcompletionblock)

The closure to execute when a record zone’s fetch finishes.

Deprecated

[`var fetchRecordZoneChangesCompletionBlock: (((any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonechangesoperation/fetchrecordzonechangescompletionblock)

The closure to execute when the operation finishes.

Deprecated

Current page is recordZoneChangeTokensUpdatedBlock
