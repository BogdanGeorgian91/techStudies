# CKFetchRecordsOperation

An operation for retrieving records from a database.

```
class CKFetchRecordsOperation
```

## [Overview](/documentation/cloudkit/ckfetchrecordsoperation#overview)

Use this operation to retrieve the entire contents of each record or only a subset of its contained values. As records become available, the operation object reports progress about the state of the operation to several different blocks, which you can use to process the results.

Fetching records is a common use of CloudKit, even if your app doesn’t cache record IDs locally. For example, when you fetch a record related to the current record through a [`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference) object, you use the ID in the reference to perform the fetch.

The handlers you assign to process the fetched records execute serially on an internal queue that the fetch operation manages. Your handlers must be capable of executing on a background thread, so any tasks that require access to the main thread must redirect accordingly.

In addition to data records, a fetch records operation can fetch the current user record. The [`fetchCurrentUserRecordOperation()`](/documentation/cloudkit/ckfetchrecordsoperation/fetchcurrentuserrecordoperation\(\)) method returns a specially configured operation object that retrieves the current user record. That record is a standard [`CKRecord`](/documentation/cloudkit/ckrecord) object that has no content initially. You can add data to the user record and save it as necessary. Don’t store sensitive personal information, such as passwords, in the user record because other users of your app can access the discoverable user record in a public database. If you must store sensitive information about a user, do so in a separate record that is accessible only to that user.

If you assign a closure to the [`completionBlock`](/documentation/Foundation/Operation/completionBlock) property of the operation object, CloudKit calls it after the operation executes and returns its results. Use a closure to perform any housekeeping tasks for the operation, but don’t use it to process the results of the operation. The closure you specify should handle the failure of the operation to complete its task, whether due to an error or an explicit cancellation.

## [Topics](/documentation/cloudkit/ckfetchrecordsoperation#topics)

### [Creating a Record Fetch Operation](/documentation/cloudkit/ckfetchrecordsoperation#Creating-a-Record-Fetch-Operation)

[`convenience init(recordIDs: [CKRecord.ID])`](/documentation/cloudkit/ckfetchrecordsoperation/init\(recordids:\))

Creates a fetch operation for retrieving the records with the specified IDs.

[`init()`](/documentation/cloudkit/ckfetchrecordsoperation/init\(\))

Creates an empty fetch operation.

### [Getting the Current User Record](/documentation/cloudkit/ckfetchrecordsoperation#Getting-the-Current-User-Record)

[`class func fetchCurrentUserRecordOperation() -> Self`](/documentation/cloudkit/ckfetchrecordsoperation/fetchcurrentuserrecordoperation\(\))

Returns a fetch operation for retrieving the current user record.

### [Configuring a Record Fetch Operation](/documentation/cloudkit/ckfetchrecordsoperation#Configuring-a-Record-Fetch-Operation)

[`var recordIDs: [CKRecord.ID]?`](/documentation/cloudkit/ckfetchrecordsoperation/recordids)

The record IDs of the records to fetch.

[`var desiredKeys: [CKRecord.FieldKey]?`](/documentation/cloudkit/ckfetchrecordsoperation/desiredkeys-31bbq)

The fields of the records to fetch.

### [Processing Record Fetch Results](/documentation/cloudkit/ckfetchrecordsoperation#Processing-Record-Fetch-Results)

[`var perRecordProgressBlock: ((CKRecord.ID, Double) -> Void)?`](/documentation/cloudkit/ckfetchrecordsoperation/perrecordprogressblock)

The closure to execute with progress information for individual records.

[`var perRecordCompletionBlock: ((CKRecord?, CKRecord.ID?, (any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchrecordsoperation/perrecordcompletionblock)

The closure to execute when a record becomes available.

Deprecated

[`var fetchRecordsCompletionBlock: (([CKRecord.ID : CKRecord]?, (any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchrecordsoperation/fetchrecordscompletionblock)

The closure to execute after CloudKit retrieves all of the records.

Deprecated

### [Instance Properties](/documentation/cloudkit/ckfetchrecordsoperation#Instance-Properties)

[`var fetchRecordsResultBlock: ((Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/ckfetchrecordsoperation/fetchrecordsresultblock)

[`var perRecordResultBlock: ((CKRecord.ID, Result<CKRecord, any Error>) -> Void)?`](/documentation/cloudkit/ckfetchrecordsoperation/perrecordresultblock)

## [Relationships](/documentation/cloudkit/ckfetchrecordsoperation#relationships)

### [Inherits From](/documentation/cloudkit/ckfetchrecordsoperation#inherits-from)

-   [`CKDatabaseOperation`](/documentation/cloudkit/ckdatabaseoperation)

### [Conforms To](/documentation/cloudkit/ckfetchrecordsoperation#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

# init(recordIDs:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordsOperation](/documentation/cloudkit/ckfetchrecordsoperation)
-   init(recordIDs:)

Initializer

# init(recordIDs:)

Creates a fetch operation for retrieving the records with the specified IDs.

```
convenience init(recordIDs: [CKRecord.ID])
```

## [Parameters](/documentation/cloudkit/ckfetchrecordsoperation/init\(recordids:\)#parameters)

`recordIDs`

An array of [`CKRecord.ID`](/documentation/cloudkit/ckrecord/id) objects that represents the records you want to retrieve. If you provide an empty array, you must set the [`recordIDs`](/documentation/cloudkit/ckfetchrecordsoperation/recordids) property before you execute the operation.

## [Discussion](/documentation/cloudkit/ckfetchrecordsoperation/init\(recordids:\)#Discussion)

A fetch operation retrieves all of a record’s fields, including any assets that those fields reference. If you want to minimize the amount of data that the operation returns, configure the [`desiredKeys`](/documentation/cloudkit/ckfetchrecordsoperation/desiredkeys-34l1l) property with only the keys that contain the values that you have an interest in.

After initializing the operation, you must associate at least one progress handler with the operation (excluding the completion handler) to process the results.

# init() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordsOperation](/documentation/cloudkit/ckfetchrecordsoperation)
-   init()

Initializer

# init()

Creates an empty fetch operation.

```
init()
```

## [Discussion](/documentation/cloudkit/ckfetchrecordsoperation/init\(\)#Discussion)

You must set the [`recordIDs`](/documentation/cloudkit/ckfetchrecordsoperation/recordids) property before you execute the operation.

A fetch operation retrieves all of a record’s fields, including any assets that those fields reference. If you want to minimize the amount of data that the operation returns, configure the [`desiredKeys`](/documentation/cloudkit/ckfetchrecordsoperation/desiredkeys-34l1l) property with only the keys that contain the values that you have an interest in.

After initializing the operation, you must associate at least one progress handler with the operation (excluding the completion handler) to process the results.

## [See Also](/documentation/cloudkit/ckfetchrecordsoperation/init\(\)#see-also)

### [Creating a Record Fetch Operation](/documentation/cloudkit/ckfetchrecordsoperation/init\(\)#Creating-a-Record-Fetch-Operation)

[`convenience init(recordIDs: [CKRecord.ID])`](/documentation/cloudkit/ckfetchrecordsoperation/init\(recordids:\))

Creates a fetch operation for retrieving the records with the specified IDs.

Current page is init()

---
  
Getting the Current User Record

# fetchCurrentUserRecordOperation() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordsOperation](/documentation/cloudkit/ckfetchrecordsoperation)
-   fetchCurrentUserRecordOperation()

Type Method

# fetchCurrentUserRecordOperation()

Returns a fetch operation for retrieving the current user record.

```
class func fetchCurrentUserRecordOperation() -> Self
```

## [Discussion](/documentation/cloudkit/ckfetchrecordsoperation/fetchcurrentuserrecordoperation\(\)#Discussion)

The returned operation object searches for the single record that corresponds to the current user record. You must associate at least one progress handler with the operation object (excluding the completion handler) to process the results.

Current page is fetchCurrentUserRecordOperation()

---
  
Configuring a Record Fetch Operation

# recordIDs | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordsOperation](/documentation/cloudkit/ckfetchrecordsoperation)
-   recordIDs

Instance Property

# recordIDs

The record IDs of the records to fetch.

```
var recordIDs: [CKRecord.ID]? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchrecordsoperation/recordids#Discussion)

Use this property to view or change the IDs of the records you want to retrieve. If you use the operation that [`fetchCurrentUserRecordOperation()`](/documentation/cloudkit/ckfetchrecordsoperation/fetchcurrentuserrecordoperation\(\)) returns, CloudKit ignores the contents of this property and sets its value to `nil`.

If you intend to specify a value other than `nil`, do so before you execute the operation or add the operation to a queue. The records you fetch don’t need to be in the same record zone. The record ID for each record provides the zone information that CloudKit needs to fetch the corresponding record.
# desiredKeys | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordsOperation](/documentation/cloudkit/ckfetchrecordsoperation)
-   desiredKeys

Instance Property

# desiredKeys

The fields of the records to fetch.

```
var desiredKeys: [CKRecord.FieldKey]? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchrecordsoperation/desiredkeys-31bbq#Discussion)

Use this property to limit the amount of data that CloudKit returns for each record during the fetch operation. When CloudKit returns a record, it only includes fields with names that match one of the keys in this property. The property’s default value is `nil`, which instructs CloudKit to return all of a record’s keys.

If you’re retrieving records of different types, make sure the array includes the fields you want from all of the various record types that the operation can return.

If you intend to specify a value other than `nil`, do so before you execute the operation or add the operation to a queue.
# perRecordProgressBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordsOperation](/documentation/cloudkit/ckfetchrecordsoperation)
-   perRecordProgressBlock

Instance Property

# perRecordProgressBlock

The closure to execute with progress information for individual records.

```
var perRecordProgressBlock: ((CKRecord.ID, Double) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchrecordsoperation/perrecordprogressblock#Discussion)

This property is a closure that returns no value and has the following parameters:

-   The ID of the record to retrieve.
    
-   The amount of data, as a percentage, that CloudKit downloads for the record. The range is `0.0` to `1.0`, where `0.0` indicates that CloudKit hasn’t downloaded anything, and `1.0` means the download is complete.
    

The fetch operation executes this closure one or more times for each record ID in the [`recordIDs`](/documentation/cloudkit/ckfetchrecordsoperation/recordids) property. Each time the closure executes, it executes serially with respect to the other progress closures of the operation. You can use this closure to track the ongoing progress of the download operation.

If you intend to use this closure to process results, set it before you execute the operation or add the operation to a queue.
# fetchRecordsResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordsOperation](/documentation/cloudkit/ckfetchrecordsoperation)
-   fetchRecordsResultBlock

Instance Property

# fetchRecordsResultBlock

```
var fetchRecordsResultBlock: ((Result<Void, any Error>) -> Void)? { get set }
```

Current page is fetchRecordsResultBlock

# perRecordResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchRecordsOperation](/documentation/cloudkit/ckfetchrecordsoperation)
-   perRecordResultBlock

Instance Property

# perRecordResultBlock

```
var perRecordResultBlock: ((CKRecord.ID, Result<CKRecord, any Error>) -> Void)? { get set }
```

Current page is perRecordResultBlock
