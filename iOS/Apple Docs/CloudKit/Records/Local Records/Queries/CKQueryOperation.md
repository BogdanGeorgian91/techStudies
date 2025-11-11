
# CKQueryOperation

An operation for executing queries in a database.

```
class CKQueryOperation
```

## [Overview](/documentation/cloudkit/ckqueryoperation#overview)

A `CKQueryOperation` object is a concrete operation that you can use to execute queries. A query operation applies query parameters to the specified database and record zone, delivering any matching records asynchronously to the handlers that you provide.

To perform a new search:

1.  Initialize a `CKQueryOperation` object with a [`CKQuery`](/documentation/cloudkit/ckquery) object that contains the search criteria and sorting information for the records you want.
    
2.  Assign a handler to the [`queryCompletionBlock`](/documentation/cloudkit/ckqueryoperation/querycompletionblock) property so that you can process the results and execute the operation.
    

If the search yields many records, the operation object may deliver a portion of the total results to your blocks immediately, along with a cursor for obtaining the remaining records. Use the cursor to initialize and execute a separate `CKQueryOperation` instance when you’re ready to process the next batch of results. 3. Optionally, configure the results by specifying values for the [`resultsLimit`](/documentation/cloudkit/ckqueryoperation/resultslimit) and [`desiredKeys`](/documentation/cloudkit/ckqueryoperation/desiredkeys-4a6vy) properties. 4. Pass the query operation object to the [`add(_:)`](/documentation/cloudkit/ckdatabase/add\(_:\)) method of the target database to execute the operation.

CloudKit restricts queries to the records in a single record zone. For new queries, you specify the zone when you initialize the query operation object. For cursor-based queries, the cursor contains the zone information. To search for records in multiple zones, you must create a separate `CKQueryOperation` object for each zone you want to search, although you can initialize each of them with the same [`CKQuery`](/documentation/cloudkit/ckquery) object.

If you assign a handler to the operation’s [`completionBlock`](/documentation/Foundation/Operation/completionBlock) property, the operation calls it after it executes and returns any results. Use a handler to perform housekeeping tasks for the operation, but don’t use it to process the results of the operation. The handler you provide should manage any failures, whether due to an error or an explicit cancellation.

## [Topics](/documentation/cloudkit/ckqueryoperation#topics)

### [Creating a Query Operation](/documentation/cloudkit/ckqueryoperation#Creating-a-Query-Operation)

[`convenience init(query: CKQuery)`](/documentation/cloudkit/ckqueryoperation/init\(query:\))

Creates an operation that searches for records in the specified record zone.

[`convenience init(cursor: CKQueryOperation.Cursor)`](/documentation/cloudkit/ckqueryoperation/init\(cursor:\))

Creates an operation with additional results from a previous search.

[`init()`](/documentation/cloudkit/ckqueryoperation/init\(\))

Creates an empty query operation.

### [Configuring the Query Operation](/documentation/cloudkit/ckqueryoperation#Configuring-the-Query-Operation)

[`var query: CKQuery?`](/documentation/cloudkit/ckqueryoperation/query)

The query for the search.

[`var cursor: CKQueryOperation.Cursor?`](/documentation/cloudkit/ckqueryoperation/cursor-swift.property)

The cursor for continuing the search.

[`class Cursor`](/documentation/cloudkit/ckqueryoperation/cursor-swift.class)

An object that marks the stopping point for a query and the starting point for retrieving the remaining results.

[`var zoneID: CKRecordZone.ID?`](/documentation/cloudkit/ckqueryoperation/zoneid)

The ID of the record zone that contains the records to search.

[`var resultsLimit: Int`](/documentation/cloudkit/ckqueryoperation/resultslimit)

The maximum number of records to return at one time.

[`class let maximumResults: Int`](/documentation/cloudkit/ckqueryoperation/maximumresults)

A constant value that represents the maximum number of results CloudKit retrieves.

[`var desiredKeys: [CKRecord.FieldKey]?`](/documentation/cloudkit/ckqueryoperation/desiredkeys-7qrse)

The fields of the records to fetch.

### [Processing the Query Results](/documentation/cloudkit/ckqueryoperation#Processing-the-Query-Results)

[`var recordFetchedBlock: ((CKRecord) -> Void)?`](/documentation/cloudkit/ckqueryoperation/recordfetchedblock)

The closure to execute when a record becomes available.

Deprecated

[`var queryCompletionBlock: ((CKQueryOperation.Cursor?, (any Error)?) -> Void)?`](/documentation/cloudkit/ckqueryoperation/querycompletionblock)

The closure to execute after CloudKit retrieves all of the records.

Deprecated

### [Instance Properties](/documentation/cloudkit/ckqueryoperation#Instance-Properties)

[`var queryResultBlock: ((Result<CKQueryOperation.Cursor?, any Error>) -> Void)?`](/documentation/cloudkit/ckqueryoperation/queryresultblock)

[`var recordMatchedBlock: ((CKRecord.ID, Result<CKRecord, any Error>) -> Void)?`](/documentation/cloudkit/ckqueryoperation/recordmatchedblock-2qze7)

## [Relationships](/documentation/cloudkit/ckqueryoperation#relationships)

### [Inherits From](/documentation/cloudkit/ckqueryoperation#inherits-from)

-   [`CKDatabaseOperation`](/documentation/cloudkit/ckdatabaseoperation)

### [Conforms To](/documentation/cloudkit/ckqueryoperation#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

---
  
Creating a Query Operation

# init(query:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryOperation](/documentation/cloudkit/ckqueryoperation)
-   init(query:)

Initializer

# init(query:)

Creates an operation that searches for records in the specified record zone.

```
convenience init(query: CKQuery)
```

## [Parameters](/documentation/cloudkit/ckqueryoperation/init\(query:\)#parameters)

`query`

The query for the search.

## [Discussion](/documentation/cloudkit/ckqueryoperation/init\(query:\)#Discussion)

You can use the operation that this method returns only once to perform a search, but you can reuse the query that you provide. During execution, the operation performs a new search and returns the first batch of results. If there are more results available, you must create a separate query object using the provided cursor object.

## [See Also](/documentation/cloudkit/ckqueryoperation/init\(query:\)#see-also)

### [Creating a Query Operation](/documentation/cloudkit/ckqueryoperation/init\(query:\)#Creating-a-Query-Operation)

[`convenience init(cursor: CKQueryOperation.Cursor)`](/documentation/cloudkit/ckqueryoperation/init\(cursor:\))

Creates an operation with additional results from a previous search.

[`init()`](/documentation/cloudkit/ckqueryoperation/init\(\))

Creates an empty query operation.

Current page is init(query:)

# init(cursor:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryOperation](/documentation/cloudkit/ckqueryoperation)
-   init(cursor:)

Initializer

# init(cursor:)

Creates an operation with additional results from a previous search.

```
convenience init(cursor: CKQueryOperation.Cursor)
```

## [Parameters](/documentation/cloudkit/ckqueryoperation/init\(cursor:\)#parameters)

`cursor`

The cursor that identifies the previous search. CloudKit passes this value to the completion handler of the previous search. For more information, see the [`queryCompletionBlock`](/documentation/cloudkit/ckqueryoperation/querycompletionblock) property.

## [Discussion](/documentation/cloudkit/ckqueryoperation/init\(cursor:\)#Discussion)

Use this method to create an operation that retrieves the next batch of results from a previous search. When executing searches for a cursor, don’t cache cursors for a long time before using them. A cursor isn’t a snapshot of the previous search results; it stores a relative offset into the results list. An operation that you create from a cursor performs a new search, sorts the new set of results, and uses the previous offset value to determine where the next batch of results starts.

# init() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryOperation](/documentation/cloudkit/ckqueryoperation)
-   init()

Initializer

# init()

Creates an empty query operation.

```
init()
```

---
### Configuring the Query Operation

# query | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryOperation](/documentation/cloudkit/ckqueryoperation)
-   query

Instance Property

# query

The query for the search.

```
@NSCopying
var query: CKQuery? { get set }
```

## [Discussion](/documentation/cloudkit/ckqueryoperation/query#Discussion)

The initial value of this property is the query that you provide to the [`init(query:)`](/documentation/cloudkit/ckqueryoperation/init\(query:\)) method. When the value in the [`cursor`](/documentation/cloudkit/ckqueryoperation/cursor-swift.property) property is `nil`, the operation uses this property’s value to execute a new search and return its results to your completion handler. If [`cursor`](/documentation/cloudkit/ckqueryoperation/cursor-swift.property) isn’t `nil`, the operation uses the cursor instead.

If you intend to specify or change the value of this property, do so before you execute the operation or submit it to a queue.
# cursor | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryOperation](/documentation/cloudkit/ckqueryoperation)
-   cursor

Instance Property

# cursor

The cursor for continuing the search.

```
@NSCopying
var cursor: CKQueryOperation.Cursor? { get set }
```

## [Discussion](/documentation/cloudkit/ckqueryoperation/cursor-swift.property#Discussion)

The initial value of this property is the cursor that you provide to the [`init(cursor:)`](/documentation/cloudkit/ckqueryoperation/init\(cursor:\)) method. When you use a cursor, the operation ignores the contents of the [`query`](/documentation/cloudkit/ckqueryoperation/query) property. This property’s value is an opaque value that CloudKit provides. For more information, see the [`queryCompletionBlock`](/documentation/cloudkit/ckqueryoperation/querycompletionblock) property.

If you intend to specify or change the value in this property, do so before you execute the operation or submit it to a queue.

# CKQueryOperation.Cursor | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryOperation](/documentation/cloudkit/ckqueryoperation)
-   CKQueryOperation.Cursor

Class

# CKQueryOperation.Cursor

An object that marks the stopping point for a query and the starting point for retrieving the remaining results.

```
class Cursor
```

## [Overview](/documentation/cloudkit/ckqueryoperation/cursor-swift.class#overview)

You don’t create instances of this class yourself. When fetching records using a query operation, if the number of results exceeds the limit for the query, CloudKit provides a cursor. Use that cursor to create a new instance of [`CKQueryOperation`](/documentation/cloudkit/ckqueryoperation) and retrieve the next batch of results for the same query.

For information about how to use a [`CKQueryOperation.Cursor`](/documentation/cloudkit/ckqueryoperation/cursor-swift.class) object, see [`CKQueryOperation`](/documentation/cloudkit/ckqueryoperation).

## [Relationships](/documentation/cloudkit/ckqueryoperation/cursor-swift.class#relationships)

### [Inherits From](/documentation/cloudkit/ckqueryoperation/cursor-swift.class#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckqueryoperation/cursor-swift.class#conforms-to)

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
# zoneID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryOperation](/documentation/cloudkit/ckqueryoperation)
-   zoneID

Instance Property

# zoneID

The ID of the record zone that contains the records to search.

```
@NSCopying
var zoneID: CKRecordZone.ID? { get set }
```

## [Discussion](/documentation/cloudkit/ckqueryoperation/zoneid#Discussion)

The value of this property limits the scope of the search to only the records in the specified record zone. If you don’t specify a record zone, the search includes all record zones.

When you create an operation using the [`init(cursor:)`](/documentation/cloudkit/ckqueryoperation/init\(cursor:\)) method, this property’s value is `nil` and CloudKit ignores any changes that you make to it. When the operation executes, the cursor provides the record zone information from the original search that provides the cursor.

## [See Also](/documentation/cloudkit/ckqueryoperation/zoneid#see-also)

### [Configuring the Query Operation](/documentation/cloudkit/ckqueryoperation/zoneid#Configuring-the-Query-Operation)

[`var query: CKQuery?`](/documentation/cloudkit/ckqueryoperation/query)

The query for the search.

[`var cursor: CKQueryOperation.Cursor?`](/documentation/cloudkit/ckqueryoperation/cursor-swift.property)

The cursor for continuing the search.

[`class Cursor`](/documentation/cloudkit/ckqueryoperation/cursor-swift.class)

An object that marks the stopping point for a query and the starting point for retrieving the remaining results.

[`var resultsLimit: Int`](/documentation/cloudkit/ckqueryoperation/resultslimit)

The maximum number of records to return at one time.

[`class let maximumResults: Int`](/documentation/cloudkit/ckqueryoperation/maximumresults)

A constant value that represents the maximum number of results CloudKit retrieves.

[`var desiredKeys: [CKRecord.FieldKey]?`](/documentation/cloudkit/ckqueryoperation/desiredkeys-7qrse)

The fields of the records to fetch.

Current page is zoneID

# resultsLimit | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryOperation](/documentation/cloudkit/ckqueryoperation)
-   resultsLimit

Instance Property

# resultsLimit

The maximum number of records to return at one time.

```
var resultsLimit: Int { get set }
```

## [Discussion](/documentation/cloudkit/ckqueryoperation/resultslimit#Discussion)

For most queries, leave the value of this property as the default value, which is the [`maximumResults`](/documentation/cloudkit/ckqueryoperation/maximumresults) constant. When using that value, CloudKit returns as many records as possible while minimizing delays in receiving those records. If you want to process a fixed number of results, change the value of this property accordingly.
# maximumResults | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryOperation](/documentation/cloudkit/ckqueryoperation)
-   maximumResults

Type Property

# maximumResults

A constant value that represents the maximum number of results CloudKit retrieves.

```
class let maximumResults: Int
```

## [Discussion](/documentation/cloudkit/ckqueryoperation/maximumresults#Discussion)

The value of this constant doesn’t correspond to the actual number of records. CloudKit dynamically determines the actual number according to various conditions at runtime.

This constant is the [`resultsLimit`](/documentation/cloudkit/ckqueryoperation/resultslimit) property’s default value.

## [See Also](/documentation/cloudkit/ckqueryoperation/maximumresults#see-also)

### [Configuring the Query Operation](/documentation/cloudkit/ckqueryoperation/maximumresults#Configuring-the-Query-Operation)

[`var query: CKQuery?`](/documentation/cloudkit/ckqueryoperation/query)

The query for the search.

[`var cursor: CKQueryOperation.Cursor?`](/documentation/cloudkit/ckqueryoperation/cursor-swift.property)

The cursor for continuing the search.

[`class Cursor`](/documentation/cloudkit/ckqueryoperation/cursor-swift.class)

An object that marks the stopping point for a query and the starting point for retrieving the remaining results.

[`var zoneID: CKRecordZone.ID?`](/documentation/cloudkit/ckqueryoperation/zoneid)

The ID of the record zone that contains the records to search.

[`var resultsLimit: Int`](/documentation/cloudkit/ckqueryoperation/resultslimit)

The maximum number of records to return at one time.

[`var desiredKeys: [CKRecord.FieldKey]?`](/documentation/cloudkit/ckqueryoperation/desiredkeys-7qrse)

The fields of the records to fetch.

Current page is maximumResults

# desiredKeys | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryOperation](/documentation/cloudkit/ckqueryoperation)
-   desiredKeys

Instance Property

# desiredKeys

The fields of the records to fetch.

```
var desiredKeys: [CKRecord.FieldKey]? { get set }
```

## [Discussion](/documentation/cloudkit/ckqueryoperation/desiredkeys-7qrse#Discussion)

Use this property to limit the amount of data that CloudKit returns for each record. When CloudKit returns a record, it only includes fields with names that match one of the keys in this property. The property’s default value is `nil`, which instructs CloudKit to return all of a record’s keys.

If you intend to specify a value other than `nil`, do so before you execute the operation or add the operation to a queue.
# queryResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryOperation](/documentation/cloudkit/ckqueryoperation)
-   queryResultBlock

Instance Property

# queryResultBlock

```
var queryResultBlock: ((Result<CKQueryOperation.Cursor?, any Error>) -> Void)? { get set }
```

Current page is queryResultBlock

# recordMatchedBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKQueryOperation](/documentation/cloudkit/ckqueryoperation)
-   recordMatchedBlock

Instance Property

# recordMatchedBlock

```
var recordMatchedBlock: ((CKRecord.ID, Result<CKRecord, any Error>) -> Void)? { get set }
```

Current page is recordMatchedBlock