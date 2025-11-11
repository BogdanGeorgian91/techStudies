# CKFetchRecordZonesOperation

An operation for retrieving record zones from a database.

```
class CKFetchRecordZonesOperation
```

## [Overview](/documentation/cloudkit/ckfetchrecordzonesoperation#overview)

Use this operation object to fetch record zones so that you can ascertain their capabilities.

If you assign a handler to the [`completionBlock`](/documentation/Foundation/Operation/completionBlock) property of the operation, CloudKit calls it after the operation executes and returns its results. You can use the handler to perform any housekeeping tasks that relate to the operation, but don’t use it to process the results of the operation. The handler you specify should manage any failures, whether due to an error or an explicit cancellation.

## [Topics](/documentation/cloudkit/ckfetchrecordzonesoperation#topics)

### [Initializing the Zone Fetch Operation](/documentation/cloudkit/ckfetchrecordzonesoperation#Initializing-the-Zone-Fetch-Operation)

[`convenience init(recordZoneIDs: [CKRecordZone.ID])`](/documentation/cloudkit/ckfetchrecordzonesoperation/init\(recordzoneids:\))

Creates an operation for fetching the specified record zones.

[`init()`](/documentation/cloudkit/ckfetchrecordzonesoperation/init\(\))

Creates an empty fetch zones operation.

### [Getting All Record Zones](/documentation/cloudkit/ckfetchrecordzonesoperation#Getting-All-Record-Zones)

[`class func fetchAllRecordZonesOperation() -> Self`](/documentation/cloudkit/ckfetchrecordzonesoperation/fetchallrecordzonesoperation\(\))

Returns an operation for fetching all record zones in the current database.

### [Configuring a Zone Fetch Operation](/documentation/cloudkit/ckfetchrecordzonesoperation#Configuring-a-Zone-Fetch-Operation)

[`var recordZoneIDs: [CKRecordZone.ID]?`](/documentation/cloudkit/ckfetchrecordzonesoperation/recordzoneids)

The IDs of the record zones to retrieve.

### [Processing Zone Fetch Results](/documentation/cloudkit/ckfetchrecordzonesoperation#Processing-Zone-Fetch-Results)

[`var fetchRecordZonesCompletionBlock: (([CKRecordZone.ID : CKRecordZone]?, (any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonesoperation/fetchrecordzonescompletionblock)

The closure to execute after CloudKit retrieves all of the record zones.

Deprecated

### [Instance Properties](/documentation/cloudkit/ckfetchrecordzonesoperation#Instance-Properties)

[`var fetchRecordZonesResultBlock: ((Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonesoperation/fetchrecordzonesresultblock)

[`var perRecordZoneResultBlock: ((CKRecordZone.ID, Result<CKRecordZone, any Error>) -> Void)?`](/documentation/cloudkit/ckfetchrecordzonesoperation/perrecordzoneresultblock)

## [Relationships](/documentation/cloudkit/ckfetchrecordzonesoperation#relationships)

### [Inherits From](/documentation/cloudkit/ckfetchrecordzonesoperation#inherits-from)

-   [`CKDatabaseOperation`](/documentation/cloudkit/ckdatabaseoperation)

### [Conforms To](/documentation/cloudkit/ckfetchrecordzonesoperation#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

---

  # Initializing the Zone Fetch Operation
  # init(recordZoneIDs:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZonesOperation](/documentation/cloudkit/ckfetchrecordzonesoperation)
-   init(recordZoneIDs:)

Initializer

# init(recordZoneIDs:)

Creates an operation for fetching the specified record zones.

```
convenience init(recordZoneIDs zoneIDs: [CKRecordZone.ID])
```

## [Parameters](/documentation/cloudkit/ckfetchrecordzonesoperation/init\(recordzoneids:\)#parameters)

`zoneIDs`

An array of[`CKRecordZone.ID`](/documentation/cloudkit/ckrecordzone/id) objects that represents the zones you want to retrieve. If you provide an empty array, you must set the [`recordZoneIDs`](/documentation/cloudkit/ckfetchrecordzonesoperation/recordzoneids) property before you execute the operation.

## [Discussion](/documentation/cloudkit/ckfetchrecordzonesoperation/init\(recordzoneids:\)#Discussion)

After creating the operation, assign a value to the [`fetchRecordZonesCompletionBlock`](/documentation/cloudkit/ckfetchrecordzonesoperation/fetchrecordzonescompletionblock) property so you can process the results.

# init() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZonesOperation](/documentation/cloudkit/ckfetchrecordzonesoperation)
-   init()

Initializer

# init()

Creates an empty fetch zones operation.

```
init()
```

## [Discussion](/documentation/cloudkit/ckfetchrecordzonesoperation/init\(\)#Discussion)

You must set the [`recordZoneIDs`](/documentation/cloudkit/ckfetchrecordzonesoperation/recordzoneids) property before you execute the operation.

After creating the operation, assign a value to the [`fetchRecordZonesCompletionBlock`](/documentation/cloudkit/ckfetchrecordzonesoperation/fetchrecordzonescompletionblock) property so you can process the results.

---
  
Getting All Record Zones

# fetchAllRecordZonesOperation() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZonesOperation](/documentation/cloudkit/ckfetchrecordzonesoperation)
-   fetchAllRecordZonesOperation()

Type Method

# fetchAllRecordZonesOperation()

Returns an operation for fetching all record zones in the current database.

```
class func fetchAllRecordZonesOperation() -> Self
```

## [Discussion](/documentation/cloudkit/ckfetchrecordzonesoperation/fetchallrecordzonesoperation\(\)#Discussion)

Assign a value to the [`fetchRecordZonesCompletionBlock`](/documentation/cloudkit/ckfetchrecordzonesoperation/fetchrecordzonescompletionblock) property of the operation that this method returns so that you can process the results.

Current page is fetchAllRecordZonesOperation()

---
  
Configuring a Zone Fetch Operation

# recordZoneIDs | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZonesOperation](/documentation/cloudkit/ckfetchrecordzonesoperation)
-   recordZoneIDs

Instance Property

# recordZoneIDs

The IDs of the record zones to retrieve.

```
var recordZoneIDs: [CKRecordZone.ID]? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchrecordzonesoperation/recordzoneids#Discussion)

Use this property to view or change the IDs of the record zones you want to retrieve. If you intend to change the value of this property, do so before you execute the operation or submit the operation to a queue.

If you use the operation that [`fetchAllRecordZonesOperation()`](/documentation/cloudkit/ckfetchrecordzonesoperation/fetchallrecordzonesoperation\(\)) returns, CloudKit ignores the contents of this property and sets its value to `nil`.

Current page is recordZoneIDs

---

### Processing Zone Fetch Results
# fetchRecordZonesCompletionBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZonesOperation](/documentation/cloudkit/ckfetchrecordzonesoperation)
-   fetchRecordZonesCompletionBlock Deprecated

Instance Property

# fetchRecordZonesCompletionBlock

The closure to execute after CloudKit retrieves all of the record zones.

```
var fetchRecordZonesCompletionBlock: (([CKRecordZone.ID : CKRecordZone]?, (any Error)?) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchrecordzonesoperation/fetchrecordzonescompletionblock#Discussion)

This property is a closure that returns no value and has the following parameters:

-   A dictionary that maps the zone IDs you request to the results. The keys in the dictionary are [`CKRecordZone.ID`](/documentation/cloudkit/ckrecordzone/id) objects, and the values are the corresponding [`CKRecordZone`](/documentation/cloudkit/ckrecordzone) objects that CloudKit returns.
    
-   If CloudKit can’t retrieve any of the record zones, an error that provides information about the failure; otherwise, `nil`.
    

The operation executes the closure only once, and it’s your only chance to process the results. The closure must be capable of executing on a background thread, so any tasks that require access to the main thread must redirect accordingly.

The closure reports an error of type [`CKError.Code.partialFailure`](/documentation/cloudkit/ckerror/code/partialfailure) when it retrieves only some of the record zones successfully. The [`userInfo`](/documentation/Foundation/NSError/userInfo) dictionary of the error contains a [`CKPartialErrorsByItemIDKey`](/documentation/cloudkit/ckpartialerrorsbyitemidkey) key that has a dictionary as its value. The keys of the dictionary are the IDs of the record zones that the operation can’t retrieve, and the corresponding values are errors that contain information about the failures.

If you intend to use this closure to process results, set it before you execute the operation or submit the operation to a queue.

Current page is fetchRecordZonesCompletionBlock

---
  
Instance Properties

# fetchRecordZonesResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZonesOperation](/documentation/cloudkit/ckfetchrecordzonesoperation)
-   fetchRecordZonesResultBlock

Instance Property

# fetchRecordZonesResultBlock

```
var fetchRecordZonesResultBlock: ((Result<Void, any Error>) -> Void)? { get set }
```

Current page is fetchRecordZonesResultBlock

# perRecordZoneResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordZonesOperation](/documentation/cloudkit/ckfetchrecordzonesoperation)
-   perRecordZoneResultBlock

Instance Property

# perRecordZoneResultBlock

```
var perRecordZoneResultBlock: ((CKRecordZone.ID, Result<CKRecordZone, any Error>) -> Void)? { get set }
```

Current page is perRecordZoneResultBlock