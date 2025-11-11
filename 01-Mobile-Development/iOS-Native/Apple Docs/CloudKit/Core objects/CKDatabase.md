# CKDatabase

An object that represents a collection of record zones and subscriptions.

```
class CKDatabase
```
## [Overview](/documentation/cloudkit/ckdatabase#overview)

A database takes requests and operations and applies them to the objects it contains, whether that’s record zones, records, or subscriptions. Each of your app’s users has access to the three separate databases:

-   A public database that’s accessible to all users of your app.
    
-   A private database that’s accessible only to the user of the current device.
    
-   A shared database that’s accessible only to the user of the current device, which contains records that other iCloud users share with them.
    

The public database is always available, even when the device doesn’t have an active iCloud account. In this scenario, your app can fetch specific records and perform searches, but it can’t create or modify records. CloudKit requires an iCloud account for writing to the public database so it can identify the authors of any changes. All access to the private and shared databases requires an iCloud account.

You don’t create instances of [`CKDatabase`](/documentation/cloudkit/ckdatabase), nor do you subclass it. Instead, you access the required database using one of your app’s containers. For more information, see [`CKContainer`](/documentation/cloudkit/ckcontainer).

By default, CloudKit executes the methods in this class with a low-priority quality of service (QoS). To use a higher-priority QoS, perform the following:

1.  Create an instance of [`CKOperation.Configuration`](/documentation/cloudkit/ckoperation/configuration-swift.class) and set its [`qualityOfService`](/documentation/cloudkit/ckoperation/configuration-swift.class/qualityofservice) property to the preferred value.
    
2.  Call the databaseʼs [`configuredWith(configuration:group:body:)`](/documentation/cloudkit/ckdatabase/configuredwith\(configuration:group:body:\)-637p1) method and provide the configuration and a trailing closure.
    
3.  In the closure, use the provided database to execute the relevant methods at the preferred QoS.
    

```
func fetchRecords(with ids: [CKRecord.ID]) async throws
    -> [CKRecord.ID: Result<CKRecord, Error>] {


    // Get a reference to the user's private database.
    let database = CKContainer.default().privateCloudDatabase


    // Create a configuration with a higher-priority quality of service.
    let config = CKOperation.Configuration()
    config.qualityOfService = .userInitiated


    // Configure the database and execute the fetch.
    return try await database.configuredWith(configuration: config) { db in
        try await db.records(for: ids)
    }
}
```

## [Topics](/documentation/cloudkit/ckdatabase#topics)

### [Configuring Database Requests](/documentation/cloudkit/ckdatabase#Configuring-Database-Requests)

[`func configuredWith<R>(configuration: CKOperation.Configuration?, group: CKOperationGroup?, body: (CKDatabase) async throws -> R) async rethrows -> R`](/documentation/cloudkit/ckdatabase/configuredwith\(configuration:group:body:\)-637p1)

Applies a temporary configuration to the database within the scope of a closure that supports concurrency.

[`func configuredWith<R>(configuration: CKOperation.Configuration?, group: CKOperationGroup?, body: (CKDatabase) throws -> R) rethrows -> R`](/documentation/cloudkit/ckdatabase/configuredwith\(configuration:group:body:\)-12vrs)

Applies a temporary configuration to the database within the scope of a closure.

### [Fetching Records](/documentation/cloudkit/ckdatabase#Fetching-Records)

[`func records(for: [CKRecord.ID], desiredKeys: [CKRecord.FieldKey]?) async throws -> [CKRecord.ID : Result<CKRecord, any Error>]`](/documentation/cloudkit/ckdatabase/records\(for:desiredkeys:\))

Fetches the specified records and returns them to an awaiting caller.

[`func fetch(withRecordIDs: [CKRecord.ID], desiredKeys: [CKRecord.FieldKey]?, completionHandler: (Result<[CKRecord.ID : Result<CKRecord, any Error>], any Error>) -> Void)`](/documentation/cloudkit/ckdatabase/fetch\(withrecordids:desiredkeys:completionhandler:\))

Fetches the specified records and delivers them to a completion handler.

[`func fetch(withRecordID: CKRecord.ID, completionHandler: (CKRecord?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/fetch\(withrecordid:completionhandler:\))

Fetches a specific record.

### [Querying Records](/documentation/cloudkit/ckdatabase#Querying-Records)

[`func records(matching: CKQuery, inZoneWith: CKRecordZone.ID?, desiredKeys: [CKRecord.FieldKey]?, resultsLimit: Int) async throws -> (matchResults: [(CKRecord.ID, Result<CKRecord, any Error>)], queryCursor: CKQueryOperation.Cursor?)`](/documentation/cloudkit/ckdatabase/records\(matching:inzonewith:desiredkeys:resultslimit:\))

Searches for records that match a predicate and returns them to an awaiting caller.

[`func records(continuingMatchFrom: CKQueryOperation.Cursor, desiredKeys: [CKRecord.FieldKey]?, resultsLimit: Int) async throws -> (matchResults: [(CKRecord.ID, Result<CKRecord, any Error>)], queryCursor: CKQueryOperation.Cursor?)`](/documentation/cloudkit/ckdatabase/records\(continuingmatchfrom:desiredkeys:resultslimit:\))

Retrieves the next batch of records from an existing search and returns them to an awaiting caller.

[`func fetch(withQuery: CKQuery, inZoneWith: CKRecordZone.ID?, desiredKeys: [CKRecord.FieldKey]?, resultsLimit: Int, completionHandler: (Result<(matchResults: [(CKRecord.ID, Result<CKRecord, any Error>)], queryCursor: CKQueryOperation.Cursor?), any Error>) -> Void)`](/documentation/cloudkit/ckdatabase/fetch\(withquery:inzonewith:desiredkeys:resultslimit:completionhandler:\))

Searches for records that match a predicate and delivers them to a completion handler.

[`func fetch(withCursor: CKQueryOperation.Cursor, desiredKeys: [CKRecord.FieldKey]?, resultsLimit: Int, completionHandler: (Result<(matchResults: [(CKRecord.ID, Result<CKRecord, any Error>)], queryCursor: CKQueryOperation.Cursor?), any Error>) -> Void)`](/documentation/cloudkit/ckdatabase/fetch\(withcursor:desiredkeys:resultslimit:completionhandler:\))

Retrieves the next batch of records from an existing search and delivers them to a completion handler.

[`func perform(CKQuery, inZoneWith: CKRecordZone.ID?, completionHandler: ([CKRecord]?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/perform\(_:inzonewith:completionhandler:\))

Searches for records matching a predicate in the specified record zone.

Deprecated

[`func records(matching: CKQuery, inZoneWith: CKRecordZone.ID?) async throws -> [CKRecord]`](/documentation/cloudkit/ckdatabase/records\(matching:inzonewith:\))

Searches for records in the specified record zone and returns them to an awaiting caller.

Deprecated

### [Modifying Records](/documentation/cloudkit/ckdatabase#Modifying-Records)

[`func modifyRecords(saving: [CKRecord], deleting: [CKRecord.ID], savePolicy: CKModifyRecordsOperation.RecordSavePolicy, atomically: Bool) async throws -> (saveResults: [CKRecord.ID : Result<CKRecord, any Error>], deleteResults: [CKRecord.ID : Result<Void, any Error>])`](/documentation/cloudkit/ckdatabase/modifyrecords\(saving:deleting:savepolicy:atomically:\))

Modifies the specified records and returns the results to an awaiting caller.

[`func modifyRecords(saving: [CKRecord], deleting: [CKRecord.ID], savePolicy: CKModifyRecordsOperation.RecordSavePolicy, atomically: Bool, completionHandler: (Result<(saveResults: [CKRecord.ID : Result<CKRecord, any Error>], deleteResults: [CKRecord.ID : Result<Void, any Error>]), any Error>) -> Void)`](/documentation/cloudkit/ckdatabase/modifyrecords\(saving:deleting:savepolicy:atomically:completionhandler:\))

Modifies the specified records and delivers the results to a completion hander.

[`enum RecordSavePolicy`](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy)

Constants that indicate which policy to apply when saving records.

[`func save(CKRecord, completionHandler: (CKRecord?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-3tatz)

Saves a specific record.

[`func delete(withRecordID: CKRecord.ID, completionHandler: (CKRecord.ID?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/delete\(withrecordid:completionhandler:\))

Deletes a specific record.

### [Fetching Record Zones](/documentation/cloudkit/ckdatabase#Fetching-Record-Zones)

[`func recordZones(for: [CKRecordZone.ID]) async throws -> [CKRecordZone.ID : Result<CKRecordZone, any Error>]`](/documentation/cloudkit/ckdatabase/recordzones\(for:\))

Fetches the specified record zones and returns them to an awaiting caller.

[`func fetch(withRecordZoneIDs: [CKRecordZone.ID], completionHandler: (Result<[CKRecordZone.ID : Result<CKRecordZone, any Error>], any Error>) -> Void)`](/documentation/cloudkit/ckdatabase/fetch\(withrecordzoneids:completionhandler:\))

Fetches the specified record zones and delivers them to a completion handler.

[`func fetchAllRecordZones(completionHandler: ([CKRecordZone]?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/fetchallrecordzones\(completionhandler:\))

Fetches all record zones from the current database.

[`func fetch(withRecordZoneID: CKRecordZone.ID, completionHandler: (CKRecordZone?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/fetch\(withrecordzoneid:completionhandler:\))

Fetches a specific record zone.

### [Modifying Record Zones](/documentation/cloudkit/ckdatabase#Modifying-Record-Zones)

[`func modifyRecordZones(saving: [CKRecordZone], deleting: [CKRecordZone.ID]) async throws -> (saveResults: [CKRecordZone.ID : Result<CKRecordZone, any Error>], deleteResults: [CKRecordZone.ID : Result<Void, any Error>])`](/documentation/cloudkit/ckdatabase/modifyrecordzones\(saving:deleting:\))

Modifies the specified record zones and returns the results to an awaiting caller.

[`func modifyRecordZones(saving: [CKRecordZone], deleting: [CKRecordZone.ID], completionHandler: (Result<(saveResults: [CKRecordZone.ID : Result<CKRecordZone, any Error>], deleteResults: [CKRecordZone.ID : Result<Void, any Error>]), any Error>) -> Void)`](/documentation/cloudkit/ckdatabase/modifyrecordzones\(saving:deleting:completionhandler:\))

Modifies the specified record zones and delivers the results to a completion handler.

[`func save(CKRecordZone, completionHandler: (CKRecordZone?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-32ffr)

Saves a specific record zone.

[`func delete(withRecordZoneID: CKRecordZone.ID, completionHandler: (CKRecordZone.ID?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/delete\(withrecordzoneid:completionhandler:\))

Deletes a specific record zone.

### [Fetching Subscriptions](/documentation/cloudkit/ckdatabase#Fetching-Subscriptions)

[`func subscriptions(for: [CKSubscription.ID]) async throws -> [CKSubscription.ID : Result<CKSubscription, any Error>]`](/documentation/cloudkit/ckdatabase/subscriptions\(for:\))

Fetches the specified subscriptions and returns them to an awaiting caller.

[`func fetch(withSubscriptionIDs: [CKSubscription.ID], completionHandler: (Result<[CKSubscription.ID : Result<CKSubscription, any Error>], any Error>) -> Void)`](/documentation/cloudkit/ckdatabase/fetch\(withsubscriptionids:completionhandler:\))

Fetches the specified subscriptions and delivers them to a completion handler.

[`func subscription(for: CKSubscription.ID) async throws -> CKSubscription`](/documentation/cloudkit/ckdatabase/subscription\(for:\))

Fetches a specific subscription and returns it to an awaiting caller.

[`func fetch(withSubscriptionID: CKSubscription.ID, completionHandler: (CKSubscription?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/fetch\(withsubscriptionid:completionhandler:\))

Fetches a specific subscription and delivers it to a completion handler.

[`func fetchAllSubscriptions(completionHandler: ([CKSubscription]?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/fetchallsubscriptions\(completionhandler:\))

Fetches all subscriptions from the current database.

### [Modifying Subscriptions](/documentation/cloudkit/ckdatabase#Modifying-Subscriptions)

[`func modifySubscriptions(saving: [CKSubscription], deleting: [CKSubscription.ID]) async throws -> (saveResults: [CKSubscription.ID : Result<CKSubscription, any Error>], deleteResults: [CKSubscription.ID : Result<Void, any Error>])`](/documentation/cloudkit/ckdatabase/modifysubscriptions\(saving:deleting:\))

Modifies the specified subscriptions and returns the results to an awaiting caller.

[`func modifySubscriptions(saving: [CKSubscription], deleting: [CKSubscription.ID], completionHandler: (Result<(saveResults: [CKSubscription.ID : Result<CKSubscription, any Error>], deleteResults: [CKSubscription.ID : Result<Void, any Error>]), any Error>) -> Void)`](/documentation/cloudkit/ckdatabase/modifysubscriptions\(saving:deleting:completionhandler:\))

Modifies the specified subscriptions and delivers the results to a completion handler.

[`func save(CKSubscription, completionHandler: (CKSubscription?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-9pona)

Saves a specific subscription.

[`func deleteSubscription(withID: CKSubscription.ID) async throws -> CKSubscription.ID`](/documentation/cloudkit/ckdatabase/deletesubscription\(withid:\))

Deletes a specific subscription and returns the deleted subscription’s identifier to an awaiting caller.

[`func delete(withSubscriptionID: CKSubscription.ID, completionHandler: (String?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/delete\(withsubscriptionid:completionhandler:\))

Deletes a specific subscription and delivers the deleted subscription’s identifier to a completion handler.

### [Fetching Changes](/documentation/cloudkit/ckdatabase#Fetching-Changes)

[`func databaseChanges(since: CKServerChangeToken?, resultsLimit: Int?) async throws -> (modifications: [CKDatabase.DatabaseChange.Modification], deletions: [CKDatabase.DatabaseChange.Deletion], changeToken: CKServerChangeToken, moreComing: Bool)`](/documentation/cloudkit/ckdatabase/databasechanges\(since:resultslimit:\))

Fetches all modified record zones and returns them to an awaiting caller.

[`func fetchDatabaseChanges(since: CKServerChangeToken?, resultsLimit: Int?, completionHandler: (Result<(modifications: [CKDatabase.DatabaseChange.Modification], deletions: [CKDatabase.DatabaseChange.Deletion], changeToken: CKServerChangeToken, moreComing: Bool), any Error>) -> Void)`](/documentation/cloudkit/ckdatabase/fetchdatabasechanges\(since:resultslimit:completionhandler:\))

Fetches all modified record zones and delivers them to a completion handler.

[`enum DatabaseChange`](/documentation/cloudkit/ckdatabase/databasechange)

Objects that indicate the type of database change.

[`func recordZoneChanges(inZoneWith: CKRecordZone.ID, since: CKServerChangeToken?, desiredKeys: [CKRecord.FieldKey]?, resultsLimit: Int?) async throws -> (modificationResultsByID: [CKRecord.ID : Result<CKDatabase.RecordZoneChange.Modification, any Error>], deletions: [CKDatabase.RecordZoneChange.Deletion], changeToken: CKServerChangeToken, moreComing: Bool)`](/documentation/cloudkit/ckdatabase/recordzonechanges\(inzonewith:since:desiredkeys:resultslimit:\))

Fetches all modified records from a specific record zone and returns them to an awaiting caller.

[`func fetchRecordZoneChanges(inZoneWith: CKRecordZone.ID, since: CKServerChangeToken?, desiredKeys: [CKRecord.FieldKey]?, resultsLimit: Int?, completionHandler: (Result<(modificationResultsByID: [CKRecord.ID : Result<CKDatabase.RecordZoneChange.Modification, any Error>], deletions: [CKDatabase.RecordZoneChange.Deletion], changeToken: CKServerChangeToken, moreComing: Bool), any Error>) -> Void)`](/documentation/cloudkit/ckdatabase/fetchrecordzonechanges\(inzonewith:since:desiredkeys:resultslimit:completionhandler:\))

Fetches all modified records from a specific record zone and delivers them to a completion handler.

[`enum RecordZoneChange`](/documentation/cloudkit/ckdatabase/recordzonechange)

Objects that indicate the type of record zone change.

### [Running Operations](/documentation/cloudkit/ckdatabase#Running-Operations)

[`func add(CKDatabaseOperation)`](/documentation/cloudkit/ckdatabase/add\(_:\))

Executes the specified operation in the current database.

### [Getting the Database Type](/documentation/cloudkit/ckdatabase#Getting-the-Database-Type)

[`var databaseScope: CKDatabase.Scope`](/documentation/cloudkit/ckdatabase/databasescope)

The type of database.

[`enum Scope`](/documentation/cloudkit/ckdatabase/scope)

Constants that represent the scope of a database.

## [Relationships](/documentation/cloudkit/ckdatabase#relationships)

### [Inherits From](/documentation/cloudkit/ckdatabase#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckdatabase#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

# configuredWith(configuration:group:body:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   configuredWith(configuration:group:body:)

Instance Method

# configuredWith(configuration:group:body:)

Applies a temporary configuration to the database within the scope of a closure that supports concurrency.

```
@discardableResult @preconcurrency
func configuredWith<R>(
    configuration: CKOperation.Configuration? = nil,
    group: CKOperationGroup? = nil,
    body: (CKDatabase) async throws -> R
) async rethrows -> R
```

## [Parameters](/documentation/cloudkit/ckdatabase/configuredwith\(configuration:group:body:\)-637p1#parameters)

`configuration`

An interim configuration to apply to the current database.

`group`

The group to associate with the methods you execute in the closure. Specifying a group helps the system prioritize those method calls, and helps you identify the calls in the server logs in CloudKit Console. For more information, see [`CKOperationGroup`](/documentation/cloudkit/ckoperationgroup).

`body`

The closure to execute with the temporarily configured database.

## [Discussion](/documentation/cloudkit/ckdatabase/configuredwith\(configuration:group:body:\)-637p1#Discussion)

Use this method to apply a specific configuration to the current database that lasts only for the duration of the trailing closure. For example, you might want to temporarily elevate the quality of service (QoS) for a group of method calls, or allow one or more expensive method calls to execute only while the device is using WiFi.

```
func fetchRecords(with ids: [CKRecord.ID]) async throws
    -> [CKRecord.ID: Result<CKRecord, Error>] {
    
    // Get a reference to the user's private database.
    let database = CKContainer.default().privateCloudDatabase
    
    // Create a configuration that denies cellular access.
    let config = CKOperation.Configuration()
    config.allowsCellularAccess = false
    
    // Configure the database and execute an expensive fetch.
    return try await database.configuredWith(configuration: config) { db in
        try await db.records(for: ids)
    }
}
```

# configuredWith(configuration:group:body:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   configuredWith(configuration:group:body:)

Instance Method

# configuredWith(configuration:group:body:)

Applies a temporary configuration to the database within the scope of a closure.

```
@discardableResult
func configuredWith<R>(
    configuration: CKOperation.Configuration? = nil,
    group: CKOperationGroup? = nil,
    body: (CKDatabase) throws -> R
) rethrows -> R
```

## [Parameters](/documentation/cloudkit/ckdatabase/configuredwith\(configuration:group:body:\)-12vrs#parameters)

`configuration`

An interim configuration to apply to the current database.

`group`

The group to associate with the methods you execute in the closure. Specifying a group helps the system prioritize those method calls, and helps you identify the calls in the server logs in CloudKit Console. For more information, see [`CKOperationGroup`](/documentation/cloudkit/ckoperationgroup).

`body`

The closure to execute with the temporarily configured database.

## [Discussion](/documentation/cloudkit/ckdatabase/configuredwith\(configuration:group:body:\)-12vrs#Discussion)

Use this method to apply a specific configuration to the current database that lasts only for the duration of the trailing closure. For example, you might want to temporarily elevate the quality of service (QoS) for a group of method calls, or allow one or more expensive method calls to execute only while the device is using WiFi.

```
func fetchRecords(with ids: [CKRecord.ID], completionHandler: @escaping
    (Result<[CKRecord.ID : Result<CKRecord, Error>], Error>) -> Void) {
    
    // Get a reference to the user's private database.
    let database = CKContainer.default().privateCloudDatabase
    
    // Create a configuration that denies cellular access.
    let config = CKOperation.Configuration()
    config.allowsCellularAccess = false
    
    // Configure the database and execute an expensive fetch.
    database.configuredWith(configuration: config) { db in
        db.fetch(withRecordIDs: ids, completionHandler: completionHandler)
    }
}
```

# records(for:desiredKeys:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   records(for:desiredKeys:)

Instance Method

# records(for:desiredKeys:)

Fetches the specified records and returns them to an awaiting caller.

```
func records(
    for ids: [CKRecord.ID],
    desiredKeys: [CKRecord.FieldKey]? = nil
) async throws -> [CKRecord.ID : Result<CKRecord, any Error>]
```

## [Parameters](/documentation/cloudkit/ckdatabase/records\(for:desiredkeys:\)#parameters)

`ids`

The identifiers of the records to fetch.

`desiredKeys`

The fields to include on each fetched record. To include all fields, specify `nil`; to fetch only system fields, specify an empty array.

## [Return Value](/documentation/cloudkit/ckdatabase/records\(for:desiredkeys:\)#return-value)

A dictionary that contains the fetched records. The dictionary uses the identifiers you specify in `ids` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either the corresponding fetched record, or an error that describes why CloudKit can’t provide that record.

## [Discussion](/documentation/cloudkit/ckdatabase/records\(for:desiredkeys:\)#Discussion)

If you’re fetching records of different types, make sure that `desiredKeys` is the union of all the fields you require across each distinct record type. This method throws an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account; otherwise, the returned dictionary includes any individual record errors.

For information on a more configurable way to fetch specific records, see [`CKFetchRecordsOperation`](/documentation/cloudkit/ckfetchrecordsoperation).
# fetch(withRecordIDs:desiredKeys:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   fetch(withRecordIDs:desiredKeys:completionHandler:)

Instance Method

# fetch(withRecordIDs:desiredKeys:completionHandler:)

Fetches the specified records and delivers them to a completion handler.

```
@preconcurrency
func fetch(
    withRecordIDs recordIDs: [CKRecord.ID],
    desiredKeys: [CKRecord.FieldKey]? = nil,
    completionHandler: @escaping (Result<[CKRecord.ID : Result<CKRecord, any Error>], any Error>) -> Void
)
```

## [Parameters](/documentation/cloudkit/ckdatabase/fetch\(withrecordids:desiredkeys:completionhandler:\)#parameters)

`recordIDs`

The identifiers of the records to fetch.

`desiredKeys`

The fields to include on each fetched record. To include all fields, specify `nil`; to fetch only system fields, specify an empty array.

`completionHandler`

The closure to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckdatabase/fetch\(withrecordids:desiredkeys:completionhandler:\)#Discussion)

The completion handler takes the following parameters:

-   A [`Result`](/documentation/Swift/Result) that contains either a dictionary of fetched records, or an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account. When present, the dictionary uses the identifiers you specify in `recordIDs` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either the corresponding fetched record, or an error that describes why CloudKit can’t provide that record.
    

If you’re fetching records of different types, make sure that `desiredKeys` is the union of all the fields you require across each distinct record type.

For information on a more configurable way to fetch specific records, see [`CKFetchRecordsOperation`](/documentation/cloudkit/ckfetchrecordsoperation).

# fetch(withRecordID:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   fetch(withRecordID:completionHandler:)

Instance Method

# fetch(withRecordID:completionHandler:)

Fetches a specific record.

```
func fetch(
    withRecordID recordID: CKRecord.ID,
    completionHandler: @escaping (CKRecord?, (any Error)?) -> Void
)
```

```
func record(for recordID: CKRecord.ID) async throws -> CKRecord
```

## [Parameters](/documentation/cloudkit/ckdatabase/fetch\(withrecordid:completionhandler:\)#parameters)

`recordID`

The identifier of the record to fetch.

`completionHandler`

The closure to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckdatabase/fetch\(withrecordid:completionhandler:\)#Discussion)

The completion handler takes the following parameters:

-   The requested record, or `nil` if CloudKit can’t provide that record.
    
-   An error if a problem occurs, or `nil` if the fetch completes successfully.
    

For information on a more convenient way to fetch specific records, see [`records(for:desiredKeys:)`](/documentation/cloudkit/ckdatabase/records\(for:desiredkeys:\)).

# records(matching:inZoneWith:desiredKeys:resultsLimit:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   records(matching:inZoneWith:desiredKeys:resultsLimit:)

Instance Method

# records(matching:inZoneWith:desiredKeys:resultsLimit:)

Searches for records that match a predicate and returns them to an awaiting caller.

```
func records(
    matching query: CKQuery,
    inZoneWith zoneID: CKRecordZone.ID? = nil,
    desiredKeys: [CKRecord.FieldKey]? = nil,
    resultsLimit: Int = CKQueryOperation.maximumResults
) async throws -> (matchResults: [(CKRecord.ID, Result<CKRecord, any Error>)], queryCursor: CKQueryOperation.Cursor?)
```

## [Parameters](/documentation/cloudkit/ckdatabase/records\(matching:inzonewith:desiredkeys:resultslimit:\)#parameters)

`query`

The query that contains the search parameters. For more information, see [`CKQuery`](/documentation/cloudkit/ckquery).

`zoneID`

The identifier of the record zone to search. If you’re searching a shared database, provide a record zone identifier; otherwise, you can specify `nil` to search all record zones in the database.

`desiredKeys`

The fields to include on each fetched record. To include all fields, specify `nil`; to fetch only system fields, specify an empty array.

`resultsLimit`

The maximum number of records to return in a single set of results.

## [Return Value](/documentation/cloudkit/ckdatabase/records\(matching:inzonewith:desiredkeys:resultslimit:\)#return-value)

A tuple with the following named elements:

## [Discussion](/documentation/cloudkit/ckdatabase/records\(matching:inzonewith:desiredkeys:resultslimit:\)#discussion)

`matchResults`

An array of tuples. Each tuple includes a record identifier and a [`Result`](/documentation/Swift/Result) that contains either the corresponding matched record, or an error that describes why CloudKit can’t provide that record. For example, if CloudKit fails to materialize an asset field, it returns an error instead of a partial record. CloudKit sorts the array according to the query’s sort descriptors.

`queryCursor`

A cursor if the number of results exceeds `resultsLimit`; otherwise, `nil`.

## [Discussion](/documentation/cloudkit/ckdatabase/records\(matching:inzonewith:desiredkeys:resultslimit:\)#Discussion)

If you specify `resultsLimit` and the number of matched records exceeds that value, this method returns only that number of records and a _cursor_ — an object that marks a specific location in the full search results. To retrieve the next subset of search results, pass that cursor to the [`records(continuingMatchFrom:desiredKeys:resultsLimit:)`](/documentation/cloudkit/ckdatabase/records\(continuingmatchfrom:desiredkeys:resultslimit:\)) method. This method throws an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account; otherwise, the returned tuple includes any individual record errors.

For information on a more configurable way to search a database, see [`CKQueryOperation`](/documentation/cloudkit/ckqueryoperation).
# records(continuingMatchFrom:desiredKeys:resultsLimit:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   records(continuingMatchFrom:desiredKeys:resultsLimit:)

Instance Method

# records(continuingMatchFrom:desiredKeys:resultsLimit:)

Retrieves the next batch of records from an existing search and returns them to an awaiting caller.

```
func records(
    continuingMatchFrom queryCursor: CKQueryOperation.Cursor,
    desiredKeys: [CKRecord.FieldKey]? = nil,
    resultsLimit: Int = CKQueryOperation.maximumResults
) async throws -> (matchResults: [(CKRecord.ID, Result<CKRecord, any Error>)], queryCursor: CKQueryOperation.Cursor?)
```

## [Parameters](/documentation/cloudkit/ckdatabase/records\(continuingmatchfrom:desiredkeys:resultslimit:\)#parameters)

`queryCursor`

The cursor that identifies, within the full search results, the location of the next subset of results to retrieve.

`desiredKeys`

The fields to include on each fetched record. To include all fields, specify `nil`; to fetch only system fields, specify an empty array.

`resultsLimit`

The maximum number of records to return in a single set of results.

## [Return Value](/documentation/cloudkit/ckdatabase/records\(continuingmatchfrom:desiredkeys:resultslimit:\)#return-value)

A tuple with the following named elements:

## [Discussion](/documentation/cloudkit/ckdatabase/records\(continuingmatchfrom:desiredkeys:resultslimit:\)#discussion)

`matchResults`

An array of tuples. Each tuple includes a record identifier and a [`Result`](/documentation/Swift/Result) that contains either the corresponding matched record, or an error that describes why CloudKit can’t provide that record. For example, if CloudKit fails to materialize an asset field, it returns an error instead of a partial record. CloudKit sorts the array according to the query’s sort descriptors.

`queryCursor`

A cursor if the number of results exceeds `resultsLimit`; otherwise, `nil`.

## [Discussion](/documentation/cloudkit/ckdatabase/records\(continuingmatchfrom:desiredkeys:resultslimit:\)#Discussion)

If you specify `resultsLimit` and the number of matched records exceeds that value, this method returns only that number of records and a _cursor_ — an object that marks a specific location in the full search results. To retrieve the next subset of search results, execute this method again and pass the returned cursor from previous execution. This method throws an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account; otherwise, the returned tuple includes any individual record errors.

For information on a more configurable way to search a database, see [`CKQueryOperation`](/documentation/cloudkit/ckqueryoperation).
# fetch(withQuery:inZoneWith:desiredKeys:resultsLimit:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   fetch(withQuery:inZoneWith:desiredKeys:resultsLimit:completionHandler:)

Instance Method

# fetch(withQuery:inZoneWith:desiredKeys:resultsLimit:completionHandler:)

Searches for records that match a predicate and delivers them to a completion handler.

```
@preconcurrency
func fetch(
    withQuery query: CKQuery,
    inZoneWith zoneID: CKRecordZone.ID? = nil,
    desiredKeys: [CKRecord.FieldKey]? = nil,
    resultsLimit: Int = CKQueryOperation.maximumResults,
    completionHandler: @escaping (Result<(matchResults: [(CKRecord.ID, Result<CKRecord, any Error>)], queryCursor: CKQueryOperation.Cursor?), any Error>) -> Void
)
```

## [Parameters](/documentation/cloudkit/ckdatabase/fetch\(withquery:inzonewith:desiredkeys:resultslimit:completionhandler:\)#parameters)

`query`

The query that contains the search parameters. For more information, see [`CKQuery`](/documentation/cloudkit/ckquery).

`zoneID`

The identifier of the record zone to search. If you’re searching a shared database, provide a record zone identifier; otherwise, you can specify `nil` to search all record zones in the database.

`desiredKeys`

The fields to include on each fetched record. To include all fields, specify `nil`; to fetch only system fields, specify an empty array.

`resultsLimit`

The maximum number of records to return in a single set of results.

`completionHandler`

The closure to execute with the search results.

## [Discussion](/documentation/cloudkit/ckdatabase/fetch\(withquery:inzonewith:desiredkeys:resultslimit:completionhandler:\)#Discussion)

The completion handler takes a single [`Result`](/documentation/Swift/Result) parameter that contains either a tuple, or an error if the request fails. For example, when the network is unavailable or the device doesn’t have an active iCloud account.

When present, the tuple contains the following named elements:

`matchResults`

An array of tuples. Each tuple includes a record identifier and a [`Result`](/documentation/Swift/Result) that contains either the corresponding matched record, or an error that describes why CloudKit can’t provide that record. For example, if CloudKit fails to materialize an asset field, it returns an error instead of a partial record. CloudKit sorts the array according to the query’s sort descriptors.

`queryCursor`

A cursor if the number of results exceeds `resultsLimit`; otherwise, `nil`.

If you specify `resultsLimit` and the number of matched records exceeds that value, CloudKit provides only that number of records and a _cursor_ — an object that marks a specific location in the full search results. To retrieve the next subset of search results, pass that cursor to the [`fetch(withCursor:desiredKeys:resultsLimit:completionHandler:)`](/documentation/cloudkit/ckdatabase/fetch\(withcursor:desiredkeys:resultslimit:completionhandler:\)) method.

For information on a more configurable way to search a database, see [`CKQueryOperation`](/documentation/cloudkit/ckqueryoperation).
# fetch(withCursor:desiredKeys:resultsLimit:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   fetch(withCursor:desiredKeys:resultsLimit:completionHandler:)

Instance Method

# fetch(withCursor:desiredKeys:resultsLimit:completionHandler:)

Retrieves the next batch of records from an existing search and delivers them to a completion handler.

```
@preconcurrency
func fetch(
    withCursor queryCursor: CKQueryOperation.Cursor,
    desiredKeys: [CKRecord.FieldKey]? = nil,
    resultsLimit: Int = CKQueryOperation.maximumResults,
    completionHandler: @escaping (Result<(matchResults: [(CKRecord.ID, Result<CKRecord, any Error>)], queryCursor: CKQueryOperation.Cursor?), any Error>) -> Void
)
```

## [Parameters](/documentation/cloudkit/ckdatabase/fetch\(withcursor:desiredkeys:resultslimit:completionhandler:\)#parameters)

`queryCursor`

The cursor that identifies, within the full search results, the location of the next subset of results to retrieve.

`desiredKeys`

The fields to include on each fetched record. To include all fields, specify `nil`; to fetch only system fields, specify an empty array.

`resultsLimit`

The maximum number of records to return in a single set of results.

`completionHandler`

The closure to execute with the search results.

## [Discussion](/documentation/cloudkit/ckdatabase/fetch\(withcursor:desiredkeys:resultslimit:completionhandler:\)#Discussion)

The completion handler takes a single [`Result`](/documentation/Swift/Result) parameter that contains either a tuple, or an error if the request fails. For example, when the network is unavailable or the device doesn’t have an active iCloud account.

When present, the tuple contains the following named elements:

`matchResults`

An array of tuples. Each tuple includes a record identifier and a [`Result`](/documentation/Swift/Result) that contains either the corresponding matched record, or an error that describes why CloudKit can’t provide that record. For example, if CloudKit fails to materialize an asset field, it returns an error instead of a partial record. CloudKit sorts the array according to the query’s sort descriptors.

`queryCursor`

A cursor if the number of results exceeds `resultsLimit`; otherwise, `nil`.

If you specify `resultsLimit` and the number of matched records exceeds that value, CloudKit provides only that number of records and a _cursor_ — an object that marks a specific location in the full search results. To retrieve the next subset of search results, execute this method again and pass the provided cursor from previous execution.

For information on a more configurable way to search a database, see [`CKQueryOperation`](/documentation/cloudkit/ckqueryoperation).
# modifyRecords(saving:deleting:savePolicy:atomically:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   modifyRecords(saving:deleting:savePolicy:atomically:)

Instance Method

# modifyRecords(saving:deleting:savePolicy:atomically:)

Modifies the specified records and returns the results to an awaiting caller.

```
func modifyRecords(
    saving recordsToSave: [CKRecord],
    deleting recordIDsToDelete: [CKRecord.ID],
    savePolicy: CKModifyRecordsOperation.RecordSavePolicy = .ifServerRecordUnchanged,
    atomically: Bool = true
) async throws -> (saveResults: [CKRecord.ID : Result<CKRecord, any Error>], deleteResults: [CKRecord.ID : Result<Void, any Error>])
```

## [Parameters](/documentation/cloudkit/ckdatabase/modifyrecords\(saving:deleting:savepolicy:atomically:\)#parameters)

`recordsToSave`

The records to save.

`recordIDsToDelete`

The identifiers of the records to permanently delete.

`savePolicy`

The policy to use when modifying existing records. For possible values, see [`CKModifyRecordsOperation.RecordSavePolicy`](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy).

`atomically`

If [`true`](/documentation/swift/true), the entire operation fails if CloudKit can’t modify one or more of the specified records; otherwise, CloudKit reports individual failures in the returned tuple. Atomic changes are only applicable in record zones that have the [`atomic`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/atomic) capability.

## [Return Value](/documentation/cloudkit/ckdatabase/modifyrecords\(saving:deleting:savepolicy:atomically:\)#return-value)

A tuple with the following named elements:

## [Discussion](/documentation/cloudkit/ckdatabase/modifyrecords\(saving:deleting:savepolicy:atomically:\)#discussion)

`saveResults`

A dictionary of saved records. The dictionary uses the identifiers of the records you specify in `recordsToSave` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either the corresponding modified record (as it appears on the server), or an error that describes why CloudKit can’t modify that record.

`deleteResults`

A dictionary of deleted records. The dictionary uses the identifiers you specify in `recordIDsToDelete` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either [`Void`](/documentation/Swift/Void) to indicate a successful deletion, or an error that describes why CloudKit can’t delete that record.

## [Discussion](/documentation/cloudkit/ckdatabase/modifyrecords\(saving:deleting:savepolicy:atomically:\)#Discussion)

Deleting records may cause additional deletions if other records in the database reference the deleted records. CloudKit doesn’t provide the identifiers of any additional records it deletes. This method throws an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account, or when `atomically` is [`true`](/documentation/swift/true) and one or more of the specified changes fail; otherwise, the returned tuple includes any individual record errors.

For information on a more configurable way to modify records, see [`CKModifyRecordsOperation`](/documentation/cloudkit/ckmodifyrecordsoperation).
# modifyRecords(saving:deleting:savePolicy:atomically:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   modifyRecords(saving:deleting:savePolicy:atomically:completionHandler:)

Instance Method

# modifyRecords(saving:deleting:savePolicy:atomically:completionHandler:)

Modifies the specified records and delivers the results to a completion hander.

```
@preconcurrency
func modifyRecords(
    saving recordsToSave: [CKRecord],
    deleting recordIDsToDelete: [CKRecord.ID],
    savePolicy: CKModifyRecordsOperation.RecordSavePolicy = .ifServerRecordUnchanged,
    atomically: Bool = true,
    completionHandler: @escaping (Result<(saveResults: [CKRecord.ID : Result<CKRecord, any Error>], deleteResults: [CKRecord.ID : Result<Void, any Error>]), any Error>) -> Void
)
```

## [Parameters](/documentation/cloudkit/ckdatabase/modifyrecords\(saving:deleting:savepolicy:atomically:completionhandler:\)#parameters)

`recordsToSave`

The records to save.

`recordIDsToDelete`

The identifiers of the records to permanently delete.

`savePolicy`

The policy to use when modifying existing records. For possible values, see [`CKModifyRecordsOperation.RecordSavePolicy`](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy).

`atomically`

If [`true`](/documentation/swift/true), the entire operation fails if CloudKit can’t modify one or more of the specified records; otherwise, CloudKit reports individual failures in the returned tuple. Atomic changes are only applicable in record zones that have the [`atomic`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/atomic) capability.

`completionHandler`

The closure to execute after CloudKit processes the changes.

## [Discussion](/documentation/cloudkit/ckdatabase/modifyrecords\(saving:deleting:savepolicy:atomically:completionhandler:\)#Discussion)

The completion handler takes a single [`Result`](/documentation/Swift/Result) parameter that contains either a tuple, or an error if the request fails. For example, when the network is unavailable or the device doesn’t have an active iCloud account, or when `atomically` is [`true`](/documentation/swift/true) and one or more of the specified changes fail.

When present, the tuple contains the following named elements:

`saveResults`

A dictionary of saved records. The dictionary uses the identifiers of the records you specify in `recordsToSave` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either the corresponding modified record (as it appears on the server), or an error that describes why CloudKit can’t modify that record.

`deleteResults`

A dictionary of deleted records. The dictionary uses the identifiers you specify in `recordIDsToDelete` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either [`Void`](/documentation/Swift/Void) to indicate a successful deletion, or an error that describes why CloudKit can’t delete that record.

Deleting records may cause additional deletions if other records in the database reference the deleted records. CloudKit doesn’t provide the identifiers of any additional records it deletes.

For information on a more configurable way to modify records, see [`CKModifyRecordsOperation`](/documentation/cloudkit/ckmodifyrecordsoperation).
# CKModifyRecordsOperation.RecordSavePolicy | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordsOperation](/documentation/cloudkit/ckmodifyrecordsoperation)
-   CKModifyRecordsOperation.RecordSavePolicy

Enumeration
# CKModifyRecordsOperation.RecordSavePolicy

Constants that indicate which policy to apply when saving records.

```
enum RecordSavePolicy
```

## [Topics](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy#topics)

### [Save Policies](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy#Save-Policies)

[`case ifServerRecordUnchanged`](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy/ifserverrecordunchanged)

A policy that instructs CloudKit to only proceed if the record’s change tag matches that of the server’s copy.

[`case changedKeys`](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy/changedkeys)

A policy that instructs CloudKit to save only the fields of a record that contain changes.

[`case allKeys`](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy/allkeys)

A policy that instructs CloudKit to save all keys of a record, even those without changes.

### [Initializers](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy#Initializers)

[`init?(rawValue: Int)`](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy/init\(rawvalue:\))

## [Relationships](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy#relationships)

### [Conforms To](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy#see-also)

### [Modifying Records](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy#Modifying-Records)

[`func modifyRecords(saving: [CKRecord], deleting: [CKRecord.ID], savePolicy: CKModifyRecordsOperation.RecordSavePolicy, atomically: Bool) async throws -> (saveResults: [CKRecord.ID : Result<CKRecord, any Error>], deleteResults: [CKRecord.ID : Result<Void, any Error>])`](/documentation/cloudkit/ckdatabase/modifyrecords\(saving:deleting:savepolicy:atomically:\))

Modifies the specified records and returns the results to an awaiting caller.

[`func modifyRecords(saving: [CKRecord], deleting: [CKRecord.ID], savePolicy: CKModifyRecordsOperation.RecordSavePolicy, atomically: Bool, completionHandler: (Result<(saveResults: [CKRecord.ID : Result<CKRecord, any Error>], deleteResults: [CKRecord.ID : Result<Void, any Error>]), any Error>) -> Void)`](/documentation/cloudkit/ckdatabase/modifyrecords\(saving:deleting:savepolicy:atomically:completionhandler:\))

Modifies the specified records and delivers the results to a completion hander.

[`func save(CKRecord, completionHandler: (CKRecord?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-3tatz)

Saves a specific record.

[`func delete(withRecordID: CKRecord.ID, completionHandler: (CKRecord.ID?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/delete\(withrecordid:completionhandler:\))

Deletes a specific record.

Current page is CKModifyRecordsOperation.RecordSavePolicy

# save(\_:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   save(\_:completionHandler:)

Instance Method

# save(\_:completionHandler:)

Saves a specific record.

```
func save(
    _ record: CKRecord,
    completionHandler: @escaping (CKRecord?, (any Error)?) -> Void
)
```

```
func save(_ record: CKRecord) async throws -> CKRecord
```

## [Parameters](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-3tatz#parameters)

`record`

The record to save.

`completionHandler`

The closure to execute after CloudKit saves the record.

## [Discussion](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-3tatz#Discussion)

The completion handler takes the following parameters:

-   The saved record (as it appears on the server), or `nil` if there’s an error.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully saves the record.
    

The save succeeds only when the specified record is new, or is a more recent version than the one on the server.

For information on a more convenient way to save records, see [`modifyRecords(saving:deleting:savePolicy:atomically:)`](/documentation/cloudkit/ckdatabase/modifyrecords\(saving:deleting:savepolicy:atomically:\)).
# delete(withRecordID:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   delete(withRecordID:completionHandler:)

Instance Method

# delete(withRecordID:completionHandler:)

Deletes a specific record.

```
func delete(
    withRecordID recordID: CKRecord.ID,
    completionHandler: @escaping (CKRecord.ID?, (any Error)?) -> Void
)
```

```
func deleteRecord(withID recordID: CKRecord.ID) async throws -> CKRecord.ID
```

## [Parameters](/documentation/cloudkit/ckdatabase/delete\(withrecordid:completionhandler:\)#parameters)

`recordID`

The identifier of the record to delete.

`completionHandler`

The closure to execute after CloudKit deletes the record.

## [Discussion](/documentation/cloudkit/ckdatabase/delete\(withrecordid:completionhandler:\)#Discussion)

The completion handler takes the following parameters:

-   The identifier of the deleted record, or `nil` if there’s an error.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully deletes the record.
    

Deleting a record may cause additional deletions if other records in the database reference the deleted record. CloudKit doesn’t provide the identifiers of any additional records it deletes.

For information on a more convenient way to delete records, see [`modifyRecords(saving:deleting:savePolicy:atomically:)`](/documentation/cloudkit/ckdatabase/modifyrecords\(saving:deleting:savepolicy:atomically:\)).

# recordZones(for:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   recordZones(for:)

Instance Method

# recordZones(for:)

Fetches the specified record zones and returns them to an awaiting caller.

```
func recordZones(for ids: [CKRecordZone.ID]) async throws -> [CKRecordZone.ID : Result<CKRecordZone, any Error>]
```

## [Parameters](/documentation/cloudkit/ckdatabase/recordzones\(for:\)#parameters)

`ids`

The identifiers of the record zones to fetch.

## [Return Value](/documentation/cloudkit/ckdatabase/recordzones\(for:\)#return-value)

A dictionary that contains the fetched record zones. The dictionary uses the specified record zone identifiers as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either the corresponding fetched record zone, or an error that describes why CloudKit can’t provide that record zone.

## [Discussion](/documentation/cloudkit/ckdatabase/recordzones\(for:\)#Discussion)

This method throws an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account; otherwise, the returned dictionary includes any individual record zone errors.

For information on a more configurable way to fetch specific record zones, see [`CKFetchRecordZonesOperation`](/documentation/cloudkit/ckfetchrecordzonesoperation).
# fetch(withRecordZoneIDs:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   fetch(withRecordZoneIDs:completionHandler:)

Instance Method

# fetch(withRecordZoneIDs:completionHandler:)

Fetches the specified record zones and delivers them to a completion handler.

```
@preconcurrency
func fetch(
    withRecordZoneIDs zoneIDs: [CKRecordZone.ID],
    completionHandler: @escaping (Result<[CKRecordZone.ID : Result<CKRecordZone, any Error>], any Error>) -> Void
)
```

## [Parameters](/documentation/cloudkit/ckdatabase/fetch\(withrecordzoneids:completionhandler:\)#parameters)

`zoneIDs`

The identifiers of the record zones to fetch.

`completionHandler`

The closure to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckdatabase/fetch\(withrecordzoneids:completionhandler:\)#Discussion)

The completion handler takes the following parameters:

-   A [`Result`](/documentation/Swift/Result) that contains either a dictionary of fetched record zones, or an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account. When present, the dictionary uses the identifiers you specify in `zoneIDs` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either the corresponding fetched record zone, or an error that describes why CloudKit can’t provide that record zone.
    

For information on a more configurable way to fetch specific record zones, see [`CKFetchRecordZonesOperation`](/documentation/cloudkit/ckfetchrecordzonesoperation).

# fetchAllRecordZones(completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   fetchAllRecordZones(completionHandler:)

Instance Method

# fetchAllRecordZones(completionHandler:)

Fetches all record zones from the current database.

```
func fetchAllRecordZones(completionHandler: @escaping ([CKRecordZone]?, (any Error)?) -> Void)
```

```
func allRecordZones() async throws -> [CKRecordZone]
```

## [Parameters](/documentation/cloudkit/ckdatabase/fetchallrecordzones\(completionhandler:\)#parameters)

`completionHandler`

The closure to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckdatabase/fetchallrecordzones\(completionhandler:\)#Discussion)

The completion handler takes the following parameters:

-   An array of fetched record zones, or `nil` if there’s an error. When present, the array contains at least one record zone, the default zone.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully fetches all record zones.
    

## [See Also](/documentation/cloudkit/ckdatabase/fetchallrecordzones\(completionhandler:\)#see-also)

### [Fetching Record Zones](/documentation/cloudkit/ckdatabase/fetchallrecordzones\(completionhandler:\)#Fetching-Record-Zones)

[`func recordZones(for: [CKRecordZone.ID]) async throws -> [CKRecordZone.ID : Result<CKRecordZone, any Error>]`](/documentation/cloudkit/ckdatabase/recordzones\(for:\))

Fetches the specified record zones and returns them to an awaiting caller.

[`func fetch(withRecordZoneIDs: [CKRecordZone.ID], completionHandler: (Result<[CKRecordZone.ID : Result<CKRecordZone, any Error>], any Error>) -> Void)`](/documentation/cloudkit/ckdatabase/fetch\(withrecordzoneids:completionhandler:\))

Fetches the specified record zones and delivers them to a completion handler.

[`func fetch(withRecordZoneID: CKRecordZone.ID, completionHandler: (CKRecordZone?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/fetch\(withrecordzoneid:completionhandler:\))

Fetches a specific record zone.

Current page is fetchAllRecordZones(completionHandler:)

# fetch(withRecordZoneID:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   fetch(withRecordZoneID:completionHandler:)

Instance Method

# fetch(withRecordZoneID:completionHandler:)

Fetches a specific record zone.

```
func fetch(
    withRecordZoneID zoneID: CKRecordZone.ID,
    completionHandler: @escaping (CKRecordZone?, (any Error)?) -> Void
)
```

```
func recordZone(for zoneID: CKRecordZone.ID) async throws -> CKRecordZone
```

## [Parameters](/documentation/cloudkit/ckdatabase/fetch\(withrecordzoneid:completionhandler:\)#parameters)

`zoneID`

The identifier of the record zone to fetch.

`completionHandler`

The closure to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckdatabase/fetch\(withrecordzoneid:completionhandler:\)#Discussion)

The completion handler takes the following parameters:

-   The fetched record zone, or `nil` if there’s an error.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully fetches the specified record zone.
    

For information on a more convenient way to fetch specific record zones, see [`recordZones(for:)`](/documentation/cloudkit/ckdatabase/recordzones\(for:\)) in Swift or [`CKFetchRecordZonesOperation`](/documentation/cloudkit/ckfetchrecordzonesoperation) in Objective-C.

# modifyRecordZones(saving:deleting:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   modifyRecordZones(saving:deleting:)

Instance Method

# modifyRecordZones(saving:deleting:)

Modifies the specified record zones and returns the results to an awaiting caller.

```
func modifyRecordZones(
    saving recordZonesToSave: [CKRecordZone],
    deleting recordZoneIDsToDelete: [CKRecordZone.ID]
) async throws -> (saveResults: [CKRecordZone.ID : Result<CKRecordZone, any Error>], deleteResults: [CKRecordZone.ID : Result<Void, any Error>])
```

## [Parameters](/documentation/cloudkit/ckdatabase/modifyrecordzones\(saving:deleting:\)#parameters)

`recordZonesToSave`

The record zones to save.

`recordZoneIDsToDelete`

The identifiers of the record zones to permanently delete.

## [Return Value](/documentation/cloudkit/ckdatabase/modifyrecordzones\(saving:deleting:\)#return-value)

A tuple with the following named elements:

## [Discussion](/documentation/cloudkit/ckdatabase/modifyrecordzones\(saving:deleting:\)#discussion)

`saveResults`

A dictionary of saved record zones. The dictionary uses the identifiers of the record zones you specify in `recordZonesToSave` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either the corresponding modified record zone (as it appears on the server), or an error that describes why CloudKit can’t modify that record zone.

`deleteResults`

A dictionary of deleted record zones. The dictionary uses the identifiers you specify in `recordZoneIDsToDelete` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either [`Void`](/documentation/Swift/Void) to indicate a successful deletion, or an error that describes why CloudKit can’t delete that record zone.

## [Discussion](/documentation/cloudkit/ckdatabase/modifyrecordzones\(saving:deleting:\)#Discussion)

This method throws an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account; otherwise, the returned tuple includes any individual record zone errors.

For information on a more configurable way to modify record zones, see [`CKModifyRecordZonesOperation`](/documentation/cloudkit/ckmodifyrecordzonesoperation).
# modifyRecordZones(saving:deleting:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   modifyRecordZones(saving:deleting:completionHandler:)

Instance Method

# modifyRecordZones(saving:deleting:completionHandler:)

Modifies the specified record zones and delivers the results to a completion handler.

```
@preconcurrency
func modifyRecordZones(
    saving recordZonesToSave: [CKRecordZone],
    deleting recordZoneIDsToDelete: [CKRecordZone.ID],
    completionHandler: @escaping (Result<(saveResults: [CKRecordZone.ID : Result<CKRecordZone, any Error>], deleteResults: [CKRecordZone.ID : Result<Void, any Error>]), any Error>) -> Void
)
```

## [Parameters](/documentation/cloudkit/ckdatabase/modifyrecordzones\(saving:deleting:completionhandler:\)#parameters)

`recordZonesToSave`

The record zones to save.

`recordZoneIDsToDelete`

The identifiers of the record zones to permanently delete.

`completionHandler`

The closure to execute after CloudKit processes the changes.

## [Discussion](/documentation/cloudkit/ckdatabase/modifyrecordzones\(saving:deleting:completionhandler:\)#Discussion)

The completion handler takes a single [`Result`](/documentation/Swift/Result) parameter that contains either a tuple, or an error if the request fails. For example, when the network is unavailable or the device doesn’t have an active iCloud account.

When present, the tuple contains the following named elements:

`saveResults`

A dictionary of saved record zones. The dictionary uses the identifiers of the record zones you specify in `recordsZonesToSave` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either the corresponding modified record zone (as it appears on the server), or an error that describes why CloudKit can’t modify that record.

`deleteResults`

A dictionary of deleted record zones. The dictionary uses the identifiers you specify in `recordZoneIDsToDelete` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either [`Void`](/documentation/Swift/Void) to indicate a successful deletion, or an error that describes why CloudKit can’t delete that record zone.

For information on a more configurable way to modify record zones, see [`CKModifyRecordZonesOperation`](/documentation/cloudkit/ckmodifyrecordzonesoperation).

# save(\_:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   save(\_:completionHandler:)

Instance Method

# save(\_:completionHandler:)

Saves a specific record zone.

```
func save(
    _ zone: CKRecordZone,
    completionHandler: @escaping (CKRecordZone?, (any Error)?) -> Void
)
```

```
func save(_ zone: CKRecordZone) async throws -> CKRecordZone
```

## [Parameters](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-32ffr#parameters)

`zone`

The record zone to save.

`completionHandler`

The closure to execute after CloudKit saves the record.

## [Discussion](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-32ffr#Discussion)

The completion handler takes the following parameters:

-   The saved record zone (as it appears on the server), or `nil` if there’s an error.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully saves the record zone.
    

For information on a more convenient way to save record zones, see [`modifyRecordZones(saving:deleting:)`](/documentation/cloudkit/ckdatabase/modifyrecordzones\(saving:deleting:\)).
# delete(withRecordZoneID:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   delete(withRecordZoneID:completionHandler:)

Instance Method

# delete(withRecordZoneID:completionHandler:)

Deletes a specific record zone.

```
func delete(
    withRecordZoneID zoneID: CKRecordZone.ID,
    completionHandler: @escaping (CKRecordZone.ID?, (any Error)?) -> Void
)
```

```
func deleteRecordZone(withID zoneID: CKRecordZone.ID) async throws -> CKRecordZone.ID
```

## [Parameters](/documentation/cloudkit/ckdatabase/delete\(withrecordzoneid:completionhandler:\)#parameters)

`zoneID`

The identifier of the record zone to delete.

`completionHandler`

The closure to execute after CloudKit deletes the record zone.

## [Discussion](/documentation/cloudkit/ckdatabase/delete\(withrecordzoneid:completionhandler:\)#Discussion)

The completion handler takes the following parameters:

-   The identifier of the deleted record zone, or `nil` if there’s an error.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully deletes the record zone.
    

For information on a more convenient way to delete record zones, see [`modifyRecordZones(saving:deleting:)`](/documentation/cloudkit/ckdatabase/modifyrecordzones\(saving:deleting:\)).

# subscriptions(for:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   subscriptions(for:)

Instance Method

# subscriptions(for:)

Fetches the specified subscriptions and returns them to an awaiting caller.

```
func subscriptions(for ids: [CKSubscription.ID]) async throws -> [CKSubscription.ID : Result<CKSubscription, any Error>]
```

## [Parameters](/documentation/cloudkit/ckdatabase/subscriptions\(for:\)#parameters)

`ids`

The identifiers of the subscriptions to fetch.

## [Return Value](/documentation/cloudkit/ckdatabase/subscriptions\(for:\)#return-value)

A dictionary that contains the fetched subscriptions. The dictionary uses the identifiers you specify in `ids` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either the corresponding fetched subscription, or an error that describes why CloudKit can’t provide that subscription.

## [Discussion](/documentation/cloudkit/ckdatabase/subscriptions\(for:\)#Discussion)

This method throws an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account; otherwise, the returned dictionary includes any individual subscription errors.

For information on a more configurable way to fetch specific subscriptions, see [`CKFetchSubscriptionsOperation`](/documentation/cloudkit/ckfetchsubscriptionsoperation).

# fetch(withSubscriptionIDs:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   fetch(withSubscriptionIDs:completionHandler:)

Instance Method

# fetch(withSubscriptionIDs:completionHandler:)

Fetches the specified subscriptions and delivers them to a completion handler.

```
@preconcurrency
func fetch(
    withSubscriptionIDs subscriptionIDs: [CKSubscription.ID],
    completionHandler: @escaping (Result<[CKSubscription.ID : Result<CKSubscription, any Error>], any Error>) -> Void
)
```

## [Parameters](/documentation/cloudkit/ckdatabase/fetch\(withsubscriptionids:completionhandler:\)#parameters)

`subscriptionIDs`

The identifiers of the subscriptions to fetch.

`completionHandler`

The closure to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckdatabase/fetch\(withsubscriptionids:completionhandler:\)#Discussion)

The completion handler takes the following parameters:

-   A [`Result`](/documentation/Swift/Result) that contains either a dictionary of fetched subscriptions, or an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account. When present, the dictionary uses the identifiers you specify in `subscriptionIDs` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either the corresponding fetched subscription, or an error that describes why CloudKit can’t provide that subscription.
    

For information on a more configurable way to fetch specific subscriptions, see [`CKFetchSubscriptionsOperation`](/documentation/cloudkit/ckfetchsubscriptionsoperation).

## [See Also](/documentation/cloudkit/ckdatabase/fetch\(withsubscriptionids:completionhandler:\)#see-also)

### [Fetching Subscriptions](/documentation/cloudkit/ckdatabase/fetch\(withsubscriptionids:completionhandler:\)#Fetching-Subscriptions)

[`func subscriptions(for: [CKSubscription.ID]) async throws -> [CKSubscription.ID : Result<CKSubscription, any Error>]`](/documentation/cloudkit/ckdatabase/subscriptions\(for:\))

Fetches the specified subscriptions and returns them to an awaiting caller.

[`func subscription(for: CKSubscription.ID) async throws -> CKSubscription`](/documentation/cloudkit/ckdatabase/subscription\(for:\))

Fetches a specific subscription and returns it to an awaiting caller.

[`func fetch(withSubscriptionID: CKSubscription.ID, completionHandler: (CKSubscription?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/fetch\(withsubscriptionid:completionhandler:\))

Fetches a specific subscription and delivers it to a completion handler.

[`func fetchAllSubscriptions(completionHandler: ([CKSubscription]?, (any Error)?) -> Void)`](/documentation/cloudkit/ckdatabase/fetchallsubscriptions\(completionhandler:\))

Fetches all subscriptions from the current database.

Current page is fetch(withSubscriptionIDs:completionHandler:)

# subscription(for:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   subscription(for:)

Instance Method

# subscription(for:)

Fetches a specific subscription and returns it to an awaiting caller.

```
func subscription(for subscriptionID: CKSubscription.ID) async throws -> CKSubscription
```

## [Parameters](/documentation/cloudkit/ckdatabase/subscription\(for:\)#parameters)

`subscriptionID`

The identifier of the subscription to fetch.

## [Return Value](/documentation/cloudkit/ckdatabase/subscription\(for:\)#return-value)

The fetched subscription.

## [Discussion](/documentation/cloudkit/ckdatabase/subscription\(for:\)#Discussion)

This method throws an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account.
# fetch(withSubscriptionID:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   fetch(withSubscriptionID:completionHandler:)

Instance Method

# fetch(withSubscriptionID:completionHandler:)

Fetches a specific subscription and delivers it to a completion handler.

```
@preconcurrency
func fetch(
    withSubscriptionID subscriptionID: CKSubscription.ID,
    completionHandler: @escaping (CKSubscription?, (any Error)?) -> Void
)
```

## [Parameters](/documentation/cloudkit/ckdatabase/fetch\(withsubscriptionid:completionhandler:\)#parameters)

`subscriptionID`

The identifier of the subscription to fetch.

`completionHandler`

The closure to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckdatabase/fetch\(withsubscriptionid:completionhandler:\)#Discussion)

The completion handler takes the following parameters:

-   The requested subscription, or `nil` if CloudKit can’t provide that subscription.
    
-   An error if a problem occurs, or `nil` if the fetch completes successfully.
    

For information on a more convenient way to fetch specific subscriptions, see [`subscriptions(for:)`](/documentation/cloudkit/ckdatabase/subscriptions\(for:\)).

# fetchAllSubscriptions(completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   fetchAllSubscriptions(completionHandler:)

Instance Method

# fetchAllSubscriptions(completionHandler:)

Fetches all subscriptions from the current database.

```
func fetchAllSubscriptions(completionHandler: @escaping ([CKSubscription]?, (any Error)?) -> Void)
```

```
func allSubscriptions() async throws -> [CKSubscription]
```

## [Parameters](/documentation/cloudkit/ckdatabase/fetchallsubscriptions\(completionhandler:\)#parameters)

`completionHandler`

The closure to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckdatabase/fetchallsubscriptions\(completionhandler:\)#Discussion)

The completion handler takes the following parameters:

-   The database’s subscriptions, or `nil` if CloudKit can’t provide the subscriptions.
    
-   An error if a problem occurs, or `nil` if the fetch completes successfully.
    

For information on a more configurable way to fetch all subscriptions from a specific database, see [`fetchAllSubscriptionsOperation()`](/documentation/cloudkit/ckfetchsubscriptionsoperation/fetchallsubscriptionsoperation\(\)).

# modifySubscriptions(saving:deleting:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   modifySubscriptions(saving:deleting:)

Instance Method

# modifySubscriptions(saving:deleting:)

Modifies the specified subscriptions and returns the results to an awaiting caller.

```
func modifySubscriptions(
    saving subscriptionsToSave: [CKSubscription],
    deleting subscriptionIDsToDelete: [CKSubscription.ID]
) async throws -> (saveResults: [CKSubscription.ID : Result<CKSubscription, any Error>], deleteResults: [CKSubscription.ID : Result<Void, any Error>])
```

## [Parameters](/documentation/cloudkit/ckdatabase/modifysubscriptions\(saving:deleting:\)#parameters)

`subscriptionsToSave`

The subscriptions to save.

`subscriptionIDsToDelete`

The identifiers of the subscriptions to permanently delete.

## [Return Value](/documentation/cloudkit/ckdatabase/modifysubscriptions\(saving:deleting:\)#return-value)

A tuple with the following named elements:

## [Discussion](/documentation/cloudkit/ckdatabase/modifysubscriptions\(saving:deleting:\)#discussion)

`saveResults`

A dictionary of saved subscriptions. The dictionary uses the identifiers of the subscriptions you specify in `subscriptionsToSave` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either the corresponding modified subscription (as it appears on the server), or an error that describes why CloudKit can’t modify that subscription.

`deleteResults`

A dictionary of deleted subscriptions. The dictionary uses the identifiers you specify in `subscriptionIDsToDelete` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either [`Void`](/documentation/Swift/Void) to indicate a successful deletion, or an error that describes why CloudKit can’t delete that subscription.

## [Discussion](/documentation/cloudkit/ckdatabase/modifysubscriptions\(saving:deleting:\)#Discussion)

This method throws an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account; otherwise, the returned tuple includes any individual subscription errors.

For information on a more configurable way to modify subscriptions, see [`CKModifySubscriptionsOperation`](/documentation/cloudkit/ckmodifysubscriptionsoperation).
# modifySubscriptions(saving:deleting:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   modifySubscriptions(saving:deleting:completionHandler:)

Instance Method

# modifySubscriptions(saving:deleting:completionHandler:)

Modifies the specified subscriptions and delivers the results to a completion handler.

```
@preconcurrency
func modifySubscriptions(
    saving subscriptionsToSave: [CKSubscription],
    deleting subscriptionIDsToDelete: [CKSubscription.ID],
    completionHandler: @escaping (Result<(saveResults: [CKSubscription.ID : Result<CKSubscription, any Error>], deleteResults: [CKSubscription.ID : Result<Void, any Error>]), any Error>) -> Void
)
```

## [Parameters](/documentation/cloudkit/ckdatabase/modifysubscriptions\(saving:deleting:completionhandler:\)#parameters)

`subscriptionsToSave`

The subscriptions to save.

`subscriptionIDsToDelete`

The identifiers of the subscriptions to permanently delete.

`completionHandler`

The closure to execute after CloudKit processes the changes.

## [Discussion](/documentation/cloudkit/ckdatabase/modifysubscriptions\(saving:deleting:completionhandler:\)#Discussion)

The completion handler takes a single [`Result`](/documentation/Swift/Result) parameter that contains either a tuple, or an error if the request fails. For example, when the network is unavailable or the device doesn’t have an active iCloud account.

When present, the tuple contains the following named elements:

`saveResults`

A dictionary of saved subscriptions. The dictionary uses the identifiers of the subscriptions you specify in `subscriptionsToSave` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either the corresponding modified subscription (as it appears on the server), or an error that describes why CloudKit can’t modify that subscription.

`deleteResults`

A dictionary of deleted subscriptions. The dictionary uses the identifiers you specify in `subscriptionIDsToDelete` as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either [`Void`](/documentation/Swift/Void) to indicate a successful deletion, or an error that describes why CloudKit can’t delete that subscription.

For information on a more configurable way to modify subscriptions, see [`CKModifySubscriptionsOperation`](/documentation/cloudkit/ckmodifysubscriptionsoperation).

# save(\_:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   save(\_:completionHandler:)

Instance Method

# save(\_:completionHandler:)

Saves a specific subscription.

```
func save(
    _ subscription: CKSubscription,
    completionHandler: @escaping (CKSubscription?, (any Error)?) -> Void
)
```

```
func save(_ subscription: CKSubscription) async throws -> CKSubscription
```

## [Parameters](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-9pona#parameters)

`subscription`

The subscription to save.

`completionHandler`

The closure to execute after CloudKit saves the subscription.

## [Discussion](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-9pona#Discussion)

The completion handler takes the following parameters:

-   The saved subscription (as it appears on the server), or `nil` if there’s an error.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully saves the subscription.
    

For information on a more convenient way to save subscriptions, see [`modifySubscriptions(saving:deleting:)`](/documentation/cloudkit/ckdatabase/modifysubscriptions\(saving:deleting:\)).

# deleteSubscription(withID:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   deleteSubscription(withID:)

Instance Method

# deleteSubscription(withID:)

Deletes a specific subscription and returns the deleted subscription’s identifier to an awaiting caller.

```
@discardableResult
func deleteSubscription(withID subscriptionID: CKSubscription.ID) async throws -> CKSubscription.ID
```

## [Parameters](/documentation/cloudkit/ckdatabase/deletesubscription\(withid:\)#parameters)

`subscriptionID`

The identifier of the subscription to delete.

## [Return Value](/documentation/cloudkit/ckdatabase/deletesubscription\(withid:\)#return-value)

The identifier of the deleted subscription.

## [Discussion](/documentation/cloudkit/ckdatabase/deletesubscription\(withid:\)#Discussion)

This method throws an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account.

For information on a more convenient way to delete subscriptions, see [`modifySubscriptions(saving:deleting:)`](/documentation/cloudkit/ckdatabase/modifysubscriptions\(saving:deleting:\)).

# delete(withSubscriptionID:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   delete(withSubscriptionID:completionHandler:)

Instance Method

# delete(withSubscriptionID:completionHandler:)

Deletes a specific subscription and delivers the deleted subscription’s identifier to a completion handler.

```
@preconcurrency
func delete(
    withSubscriptionID subscriptionID: CKSubscription.ID,
    completionHandler: @escaping (String?, (any Error)?) -> Void
)
```

## [Parameters](/documentation/cloudkit/ckdatabase/delete\(withsubscriptionid:completionhandler:\)#parameters)

`subscriptionID`

The identifier of the subscription to delete.

`completionHandler`

The closure to execute after CloudKit deletes the subscription.

## [Discussion](/documentation/cloudkit/ckdatabase/delete\(withsubscriptionid:completionhandler:\)#Discussion)

The completion handler takes the following parameters:

-   The identifier of the deleted subscription, or `nil` if there’s an error.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully deletes the subscription.
    

For information on a more convenient way to delete subscriptions, see [`modifySubscriptions(saving:deleting:)`](/documentation/cloudkit/ckdatabase/modifysubscriptions\(saving:deleting:\)).

# databaseChanges(since:resultsLimit:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   databaseChanges(since:resultsLimit:)

Instance Method

# databaseChanges(since:resultsLimit:)

Fetches all modified record zones and returns them to an awaiting caller.

```
func databaseChanges(
    since changeToken: CKServerChangeToken?,
    resultsLimit: Int? = nil
) async throws -> (modifications: [CKDatabase.DatabaseChange.Modification], deletions: [CKDatabase.DatabaseChange.Deletion], changeToken: CKServerChangeToken, moreComing: Bool)
```

## [Parameters](/documentation/cloudkit/ckdatabase/databasechanges\(since:resultslimit:\)#parameters)

`changeToken`

The change token from the previous execution of this method. If this is your app’s first fetch, or you want to refetch every change in the database’s history, specify `nil`.

`resultsLimit`

The maximum number of changes to return. The server may use a limit lower than this value.

## [Return Value](/documentation/cloudkit/ckdatabase/databasechanges\(since:resultslimit:\)#return-value)

A tuple with the following named elements:

## [Discussion](/documentation/cloudkit/ckdatabase/databasechanges\(since:resultslimit:\)#discussion)

`modifications`

An array of database modifications that occur after the time that `changeToken` denotes. Each modification contains details about a modified record zone.

`deletions`

An array of database deletions that occur after the time that `changeToken` denotes. Each deletion contains details about a deleted or purged record zone.

`changeToken`

The change token that corresponds to the fetch results’ most recent change.

`moreComing`

A Boolean value that indicates whether the server has additional changes for you to fetch.

## [Discussion](/documentation/cloudkit/ckdatabase/databasechanges\(since:resultslimit:\)#Discussion)

This method fetches record zone changes in a database, which includes new record zones, changed zones — including deleted or purged zones — and zones that contain record changes. It throws an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account.

Along with the fetched changes, CloudKit supplies a _change token_, which is an opaque token that denotes a specific point in the database’s history. Store this token and provide it the next time you execute this method. Change tokens conform to [`NSSecureCoding`](/documentation/Foundation/NSSecureCoding) and are safe to cache on-disk. Don’t infer any behavior or order from a token’s contents.

For information on a more configurable way to fetch database changes, see [`CKFetchDatabaseChangesOperation`](/documentation/cloudkit/ckfetchdatabasechangesoperation).
# fetchDatabaseChanges(since:resultsLimit:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   fetchDatabaseChanges(since:resultsLimit:completionHandler:)

Instance Method

# fetchDatabaseChanges(since:resultsLimit:completionHandler:)

Fetches all modified record zones and delivers them to a completion handler.

```
@preconcurrency
func fetchDatabaseChanges(
    since changeToken: CKServerChangeToken?,
    resultsLimit: Int? = nil,
    completionHandler: @escaping (Result<(modifications: [CKDatabase.DatabaseChange.Modification], deletions: [CKDatabase.DatabaseChange.Deletion], changeToken: CKServerChangeToken, moreComing: Bool), any Error>) -> Void
)
```

## [Parameters](/documentation/cloudkit/ckdatabase/fetchdatabasechanges\(since:resultslimit:completionhandler:\)#parameters)

`changeToken`

The change token from the previous execution of this method. If this is your app’s first fetch, or you want to refetch every change in the database’s history, specify `nil`.

`resultsLimit`

The maximum number of changes to return. The server may use a limit lower than this value.

`completionHandler`

The closure to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckdatabase/fetchdatabasechanges\(since:resultslimit:completionhandler:\)#Discussion)

The completion handler takes a single [`Result`](/documentation/Swift/Result) parameter that contains either a tuple, or an error if the request fails. For example, when the network is unavailable or the device doesn’t have an active iCloud account.

When present, the tuple contains the following named elements:

`modifications`

An array of database modifications that occur after the time that `changeToken` denotes. Each modification contains details about a modified record zone.

`deletions`

An array of database deletions that occur after the time that `changeToken` denotes. Each deletion contains details about a deleted or purged record zone.

`changeToken`

The change token that corresponds to the fetch results’ most recent change.

`moreComing`

A Boolean value that indicates whether the server has additional changes for you to fetch.

This method fetches record zone changes in a database, which includes new record zones, changed zones — including deleted or purged zones — and zones that contain record changes.

Along with the fetched changes, CloudKit supplies a _change token_, which is an opaque token that denotes a specific point in the database’s history. Store this token and provide it the next time you execute this method. Change tokens conform to [`NSSecureCoding`](/documentation/Foundation/NSSecureCoding) and are safe to cache on-disk. Don’t infer any behavior or order from a token’s contents.

For information on a more configurable way to fetch database changes, see [`CKFetchDatabaseChangesOperation`](/documentation/cloudkit/ckfetchdatabasechangesoperation).

# CKDatabase.DatabaseChange | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   CKDatabase.DatabaseChange

Enumeration

# CKDatabase.DatabaseChange

Objects that indicate the type of database change.

```
enum DatabaseChange
```

## [Topics](/documentation/cloudkit/ckdatabase/databasechange#topics)

### [Modifications](/documentation/cloudkit/ckdatabase/databasechange#Modifications)

[`struct Modification`](/documentation/cloudkit/ckdatabase/databasechange/modification)

A database change that represents the modification of a record zone.

### [Deletions](/documentation/cloudkit/ckdatabase/databasechange#Deletions)

[`struct Deletion`](/documentation/cloudkit/ckdatabase/databasechange/deletion)

A database change that represents the deletion of a record zone.
# recordZoneChanges(inZoneWith:since:desiredKeys:resultsLimit:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   recordZoneChanges(inZoneWith:since:desiredKeys:resultsLimit:)

Instance Method

# recordZoneChanges(inZoneWith:since:desiredKeys:resultsLimit:)

Fetches all modified records from a specific record zone and returns them to an awaiting caller.

```
func recordZoneChanges(
    inZoneWith zoneID: CKRecordZone.ID,
    since changeToken: CKServerChangeToken?,
    desiredKeys: [CKRecord.FieldKey]? = nil,
    resultsLimit: Int? = nil
) async throws -> (modificationResultsByID: [CKRecord.ID : Result<CKDatabase.RecordZoneChange.Modification, any Error>], deletions: [CKDatabase.RecordZoneChange.Deletion], changeToken: CKServerChangeToken, moreComing: Bool)
```

## [Parameters](/documentation/cloudkit/ckdatabase/recordzonechanges\(inzonewith:since:desiredkeys:resultslimit:\)#parameters)

`zoneID`

The identifier of the record zone with changes.

`changeToken`

The change token from the previous execution of this method. If this is your app’s first fetch, or you want to refetch every change in the record zone’s history, specify `nil`.

`desiredKeys`

The fields to include on each fetched record. To include all fields, specify `nil`; to fetch only system fields, specify an empty array.

`resultsLimit`

The maximum number of changes to return. The server may use a limit lower than this value.

## [Return Value](/documentation/cloudkit/ckdatabase/recordzonechanges\(inzonewith:since:desiredkeys:resultslimit:\)#return-value)

A tuple with the following named elements:

## [Discussion](/documentation/cloudkit/ckdatabase/recordzonechanges\(inzonewith:since:desiredkeys:resultslimit:\)#discussion)

`modificationResultsByID`

A dictionary of record modifications that occur after the time that `changeToken` denotes. The dictionary uses the identifiers of modified records as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either the corresponding modification, or an error that describes why CloudKit can’t provide that information.

`deletions`

An array of record deletions that occur after the time that `changeToken` denotes. Each deletion contains details about a deleted record.

`changeToken`

The change token that corresponds to the fetch results’ most recent change.

`moreComing`

A Boolean value that indicates whether the server has additional changes for you to fetch.

## [Discussion](/documentation/cloudkit/ckdatabase/recordzonechanges\(inzonewith:since:desiredkeys:resultslimit:\)#Discussion)

This method fetches record changes in the specfied record zone, such as those that occur during record creation, modification, and deletion. It throws an error if the request fails, such as when the network is unavailable or the device doesn’t have an active iCloud account; otherwise, the returned tuple includes any individual record errors.

Along with the fetched changes, CloudKit supplies a _change token_, which is an opaque token that denotes a specific point in the record zone’s history. Store this token and provide it the next time you execute this method. Change tokens conform to [`NSSecureCoding`](/documentation/Foundation/NSSecureCoding) and are safe to cache on-disk. Don’t infer any behavior or order from a token’s contents.

For information on a more configurable way to fetch record zone changes, see [`CKFetchRecordZoneChangesOperation`](/documentation/cloudkit/ckfetchrecordzonechangesoperation).
# fetchRecordZoneChanges(inZoneWith:since:desiredKeys:resultsLimit:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   fetchRecordZoneChanges(inZoneWith:since:desiredKeys:resultsLimit:completionHandler:)

Instance Method

# fetchRecordZoneChanges(inZoneWith:since:desiredKeys:resultsLimit:completionHandler:)

Fetches all modified records from a specific record zone and delivers them to a completion handler.

```
@preconcurrency
func fetchRecordZoneChanges(
    inZoneWith zoneID: CKRecordZone.ID,
    since changeToken: CKServerChangeToken?,
    desiredKeys: [CKRecord.FieldKey]? = nil,
    resultsLimit: Int? = nil,
    completionHandler: @escaping (Result<(modificationResultsByID: [CKRecord.ID : Result<CKDatabase.RecordZoneChange.Modification, any Error>], deletions: [CKDatabase.RecordZoneChange.Deletion], changeToken: CKServerChangeToken, moreComing: Bool), any Error>) -> Void
)
```

## [Parameters](/documentation/cloudkit/ckdatabase/fetchrecordzonechanges\(inzonewith:since:desiredkeys:resultslimit:completionhandler:\)#parameters)

`zoneID`

The identifier of the record zone with changes.

`changeToken`

The change token from the previous execution of this method. If this is your app’s first fetch, or you want to refetch every change in the record zone’s history, specify `nil`.

`desiredKeys`

The fields to include on each fetched record. To include all fields, specify `nil`; to fetch only system fields, specify an empty array.

`resultsLimit`

The maximum number of changes to return. The server may use a limit lower than this value.

`completionHandler`

The closure to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckdatabase/fetchrecordzonechanges\(inzonewith:since:desiredkeys:resultslimit:completionhandler:\)#Discussion)

The completion handler takes a single [`Result`](/documentation/Swift/Result) parameter that contains either a tuple, or an error if the request fails. For example, when the network is unavailable or the device doesn’t have an active iCloud account.

When present, the tuple contains the following named elements:

`modificationResultsByID`

A dictionary of record modifications that occur after the time that `changeToken` denotes. The dictionary uses the identifiers of modified records as its keys. The value of each key is a [`Result`](/documentation/Swift/Result) that contains either the corresponding modification, or an error that describes why CloudKit can’t provide that information.

`deletions`

An array of record deletions that occur after the time that `changeToken` denotes. Each deletion contains details about a deleted record.

`changeToken`

The change token that corresponds to the fetch results’ most recent change.

`moreComing`

A Boolean value that indicates whether the server has additional changes for you to fetch.

This method fetches record changes in the specfied record zone, such as those that occur during record creation, modification, and deletion.

Along with the fetched changes, CloudKit supplies a _change token_, which is an opaque token that denotes a specific point in the record zone’s history. Store this token and provide it the next time you execute this method. Change tokens conform to [`NSSecureCoding`](/documentation/Foundation/NSSecureCoding) and are safe to cache on-disk. Don’t infer any behavior or order from a token’s contents.

For information on a more configurable way to fetch record zone changes, see [`CKFetchRecordZoneChangesOperation`](/documentation/cloudkit/ckfetchrecordzonechangesoperation)
# CKDatabase.RecordZoneChange | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   CKDatabase.RecordZoneChange

Enumeration

# CKDatabase.RecordZoneChange

Objects that indicate the type of record zone change.

```
enum RecordZoneChange
```

## [Topics](/documentation/cloudkit/ckdatabase/recordzonechange#topics)

### [Modifications](/documentation/cloudkit/ckdatabase/recordzonechange#Modifications)

[`struct Modification`](/documentation/cloudkit/ckdatabase/recordzonechange/modification)

A record zone change that represents the modification of a record.

### [Deletions](/documentation/cloudkit/ckdatabase/recordzonechange#Deletions)

[`struct Deletion`](/documentation/cloudkit/ckdatabase/recordzonechange/deletion)

A record zone change that represents the deletion of a record.

# add(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   add(\_:)

Instance Method

# add(\_:)

Executes the specified operation in the current database.

```
func add(_ operation: CKDatabaseOperation)
```

## [Parameters](/documentation/cloudkit/ckdatabase/add\(_:\)#parameters)

`operation`

The operation to execute.

## [Discussion](/documentation/cloudkit/ckdatabase/add\(_:\)#Discussion)

Configure the operation fully before you call this method. Prior to the operation executing, CloudKit sets its [`database`](/documentation/cloudkit/ckdatabaseoperation/database) property to the current database. The operation executes at the priority and quality of service (QoS) that you specify using the [`queuePriority`](/documentation/Foundation/Operation/queuePriority-swift.property) and [`qualityOfService`](/documentation/Foundation/Operation/qualityOfService) properties.

Current page is add(\_:)

---
  
Getting the Database Type

# databaseScope | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   databaseScope

Instance Property

# databaseScope

The type of database.

```
var databaseScope: CKDatabase.Scope { get }
```

## [Discussion](/documentation/cloudkit/ckdatabase/databasescope#Discussion)

For possible values, see [`CKDatabase.Scope`](/documentation/cloudkit/ckdatabase/scope).

# CKDatabase.Scope | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabase](/documentation/cloudkit/ckdatabase)
-   CKDatabase.Scope

Enumeration

# CKDatabase.Scope

Constants that represent the scope of a database.

```
enum Scope
```

## [Topics](/documentation/cloudkit/ckdatabase/scope#topics)

### [Database Scopes](/documentation/cloudkit/ckdatabase/scope#Database-Scopes)

[`` case `public` ``](/documentation/cloudkit/ckdatabase/scope/public)

The public database.

[`` case `private` ``](/documentation/cloudkit/ckdatabase/scope/private)

The private database.

[`case shared`](/documentation/cloudkit/ckdatabase/scope/shared)

The shared database.

### [Initializers](/documentation/cloudkit/ckdatabase/scope#Initializers)

[`init?(rawValue: Int)`](/documentation/cloudkit/ckdatabase/scope/init\(rawvalue:\))

## [Relationships](/documentation/cloudkit/ckdatabase/scope#relationships)

### [Conforms To](/documentation/cloudkit/ckdatabase/scope#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)