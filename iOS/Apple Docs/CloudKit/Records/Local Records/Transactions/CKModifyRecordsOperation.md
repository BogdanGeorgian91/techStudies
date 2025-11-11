# CKModifyRecordsOperation

An operation that modifies one or more records.

```
class CKModifyRecordsOperation
```

## [Mentioned in](/documentation/cloudkit/ckmodifyrecordsoperation#mentions)

[

Encrypting User Data](/documentation/cloudkit/encrypting-user-data)

## [Overview](/documentation/cloudkit/ckmodifyrecordsoperation#overview)

After modifying the fields of a record, use this operation to save those changes to a database. You also use this operation to delete records permanently from a database.

If you’re saving a record that contains a reference to another record, set the reference’s [`action`](/documentation/cloudkit/ckrecord/reference/action-swift.property) to indicate if the target record’s deletion should cascade to the saved record. This helps avoid orphaned records in explicit record hierarchies. When creating two new records that have a reference between them, use the same operation to save both records at the same time. During a save operation, CloudKit requires that the target record of the [`parent`](/documentation/cloudkit/ckrecord/parent) reference, if set, exists in the database or is part of the same operation; all other reference fields are exempt from this requirement.

When you save records, the value in the [`savePolicy`](/documentation/cloudkit/ckmodifyrecordsoperation/savepolicy) property determines how to proceed when CloudKit detects conflicts. Because records can change between the time you fetch them and the time you save them, the save policy determines whether new changes overwrite existing changes. By default, the operation reports an error when there’s a newer version on the server. You can change the default setting to permit your changes to overwrite the server values wholly or partially.

The handlers you assign to monitor progress of the operation execute serially on an internal queue that the operation manages. Your handlers must be capable of executing on a background thread, so any tasks that require access to the main thread must redirect accordingly.

If you assign a completion handler to the [`completionBlock`](/documentation/Foundation/Operation/completionBlock) property of the operation, CloudKit calls it after the operation executes and returns the results. Use the completion handler to perform any housekeeping tasks for the operation, but don’t use it to process the results of the operation. The completion handler you provide should manage any failures of the operation, whether due to an error or an explicit cancellation.

## [Topics](/documentation/cloudkit/ckmodifyrecordsoperation#topics)

### [Creating a Modify Record Operation](/documentation/cloudkit/ckmodifyrecordsoperation#Creating-a-Modify-Record-Operation)

[`convenience init(recordsToSave: [CKRecord]?, recordIDsToDelete: [CKRecord.ID]?)`](/documentation/cloudkit/ckmodifyrecordsoperation/init\(recordstosave:recordidstodelete:\))

Creates an operation for modifying the specified records.

[`init()`](/documentation/cloudkit/ckmodifyrecordsoperation/init\(\))

Creates an empty modify records operation.

### [Configuring the Modify Record Operation](/documentation/cloudkit/ckmodifyrecordsoperation#Configuring-the-Modify-Record-Operation)

[`var recordsToSave: [CKRecord]?`](/documentation/cloudkit/ckmodifyrecordsoperation/recordstosave)

The records to save to the database.

[`var recordIDsToDelete: [CKRecord.ID]?`](/documentation/cloudkit/ckmodifyrecordsoperation/recordidstodelete)

The IDs of the records to delete permanently from the database.

[`var clientChangeTokenData: Data?`](/documentation/cloudkit/ckmodifyrecordsoperation/clientchangetokendata)

A token that tracks local changes to records.

[`var isAtomic: Bool`](/documentation/cloudkit/ckmodifyrecordsoperation/isatomic)

A Boolean value that indicates whether the entire operation fails when CloudKit can’t update one or more records in a record zone.

[`var savePolicy: CKModifyRecordsOperation.RecordSavePolicy`](/documentation/cloudkit/ckmodifyrecordsoperation/savepolicy)

The policy to use when saving changes to records.

[`enum RecordSavePolicy`](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy)

Constants that indicate which policy to apply when saving records.

### [Processing the Modify Record Results](/documentation/cloudkit/ckmodifyrecordsoperation#Processing-the-Modify-Record-Results)

[`var perRecordProgressBlock: ((CKRecord, Double) -> Void)?`](/documentation/cloudkit/ckmodifyrecordsoperation/perrecordprogressblock)

The closure to execute with progress information for individual records.

[`var perRecordCompletionBlock: ((CKRecord, (any Error)?) -> Void)?`](/documentation/cloudkit/ckmodifyrecordsoperation/perrecordcompletionblock)

The closure to execute when CloudKit saves a record.

Deprecated

[`var modifyRecordsCompletionBlock: (([CKRecord]?, [CKRecord.ID]?, (any Error)?) -> Void)?`](/documentation/cloudkit/ckmodifyrecordsoperation/modifyrecordscompletionblock)

The closure to execute after CloudKit modifies all of the records.

Deprecated

### [Instance Properties](/documentation/cloudkit/ckmodifyrecordsoperation#Instance-Properties)

[`var modifyRecordsResultBlock: ((Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/ckmodifyrecordsoperation/modifyrecordsresultblock)

[`var perRecordDeleteBlock: ((CKRecord.ID, Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/ckmodifyrecordsoperation/perrecorddeleteblock-9czoo)

[`var perRecordSaveBlock: ((CKRecord.ID, Result<CKRecord, any Error>) -> Void)?`](/documentation/cloudkit/ckmodifyrecordsoperation/perrecordsaveblock-7yq9d)

## [Relationships](/documentation/cloudkit/ckmodifyrecordsoperation#relationships)

### [Inherits From](/documentation/cloudkit/ckmodifyrecordsoperation#inherits-from)

-   [`CKDatabaseOperation`](/documentation/cloudkit/ckdatabaseoperation)

### [Conforms To](/documentation/cloudkit/ckmodifyrecordsoperation#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## Creating a Modify Record Operation

# init(recordsToSave:recordIDsToDelete:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordsOperation](/documentation/cloudkit/ckmodifyrecordsoperation)
-   init(recordsToSave:recordIDsToDelete:)

Initializer

# init(recordsToSave:recordIDsToDelete:)

Creates an operation for modifying the specified records.

```
convenience init(
    recordsToSave: [CKRecord]? = nil,
    recordIDsToDelete: [CKRecord.ID]? = nil
)
```

## [Parameters](/documentation/cloudkit/ckmodifyrecordsoperation/init\(recordstosave:recordidstodelete:\)#parameters)

`recordsToSave`

The records to save. You can specify `nil` for this parameter.

`recordIDsToDelete`

The IDs of the records to delete. You can specify `nil` for this parameter.

## [Discussion](/documentation/cloudkit/ckmodifyrecordsoperation/init\(recordstosave:recordidstodelete:\)#Discussion)

The records that you intend to save or delete must all reside in the same database, which you specify when you configure the operation. If your app saves a record in a database that doesn’t exist, the server creates the database.

# init() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordsOperation](/documentation/cloudkit/ckmodifyrecordsoperation)
-   init()

Initializer

# init()

Creates an empty modify records operation.

```
init()
```

## [Discussion](/documentation/cloudkit/ckmodifyrecordsoperation/init\(\)#Discussion)

You must set at least one of the [`recordsToSave`](/documentation/cloudkit/ckmodifyrecordsoperation/recordstosave) or [`recordIDsToDelete`](/documentation/cloudkit/ckmodifyrecordsoperation/recordidstodelete) properties before you execute the operation.

## Configuring the Modify Record Operation
# recordsToSave | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordsOperation](/documentation/cloudkit/ckmodifyrecordsoperation)
-   recordsToSave

Instance Property

# recordsToSave

The records to save to the database.

```
var recordsToSave: [CKRecord]? { get set }
```

## [Discussion](/documentation/cloudkit/ckmodifyrecordsoperation/recordstosave#Discussion)

The initial value of the property is the array that you provide to the [`initWithRecordsToSave:recordIDsToDelete:`](/documentation/cloudkit/ckmodifyrecordsoperation/initwithrecordstosave:recordidstodelete:) method. You can modify this array as necessary before you execute the operation. The records must all target the same database, but can belong to different record zones.

If you intend to change the value of this property, do so before you execute the operation or submit the operation to a queue.

# recordIDsToDelete | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordsOperation](/documentation/cloudkit/ckmodifyrecordsoperation)
-   recordIDsToDelete

Instance Property

# recordIDsToDelete

The IDs of the records to delete permanently from the database.

```
var recordIDsToDelete: [CKRecord.ID]? { get set }
```

## [Discussion](/documentation/cloudkit/ckmodifyrecordsoperation/recordidstodelete#Discussion)

An array of [`CKRecord.ID`](/documentation/cloudkit/ckrecord/id) objects that identifies the records to delete. The initial value of the property is the array of record IDs that you provide to the [`initWithRecordsToSave:recordIDsToDelete:`](/documentation/cloudkit/ckmodifyrecordsoperation/initwithrecordstosave:recordidstodelete:) method.

When deleting records, the operation reports progress only on the records with the IDs that you specify in this property. Deleting records can trigger the deletion of related records if there is an owner-owned relationship between the records involving a [`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference) object. When additional deletions occur, CloudKit doesn’t pass them to the progress handler of the operation. For that reason, it’s important to understand the implications of the ownership model you use when you relate records to each other through a [`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference) object. For more information about owner-owned relationships, see [`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference).

If you intend to change the value of this property, do so before you execute the operation or submit the operation to a queue.
# clientChangeTokenData | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordsOperation](/documentation/cloudkit/ckmodifyrecordsoperation)
-   clientChangeTokenData

Instance Property

# clientChangeTokenData

A token that tracks local changes to records.

```
var clientChangeTokenData: Data? { get set }
```

## [Discussion](/documentation/cloudkit/ckmodifyrecordsoperation/clientchangetokendata#Discussion)

The default value is `nil`.

When you modify records from a fetch operation, specify a token using this property to indicate which version of the record you most recently modified. Compare the token you supply to the token in the next record fetch to confirm the server successfully receives the device’s most recent modify request.

If you intend to change the value of this property, do so before you execute the operation or submit the operation to a queue.

# isAtomic | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordsOperation](/documentation/cloudkit/ckmodifyrecordsoperation)
-   isAtomic

Instance Property

# isAtomic

A Boolean value that indicates whether the entire operation fails when CloudKit can’t update one or more records in a record zone.

```
var isAtomic: Bool { get set }
```

## [Discussion](/documentation/cloudkit/ckmodifyrecordsoperation/isatomic#Discussion)

Modifying records atomically prevents you from updating your data in a way that would leave it in an inconsistent state. You use atomic updates when you want to write multiple records to the same record zone. If there’s a failure to modify any of the records in a zone, CloudKit doesn’t change the other records in that same zone. The record zone must have the [`atomic`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/atomic) capability for this behavior to apply. If a record zone doesn’t support the atomic capability, setting this property has no effect.

The default value of this property is [`true`](/documentation/swift/true), which causes all modifications within a single record zone to occur atomically. If your operation contains records in multiple record zones, a failure in one zone doesn’t prevent modifications to records in a different zone. Changing the value of this property to [`false`](/documentation/swift/false) causes CloudKit to modify records individually, regardless of whether the record zone supports atomic modifications.
# savePolicy | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordsOperation](/documentation/cloudkit/ckmodifyrecordsoperation)
-   savePolicy

Instance Property

# savePolicy

The policy to use when saving changes to records.

```
var savePolicy: CKModifyRecordsOperation.RecordSavePolicy { get set }
```

## [Discussion](/documentation/cloudkit/ckmodifyrecordsoperation/savepolicy#Discussion)

The server uses this property to determine how to proceed when saving record changes. The exact behavior depends on the policy you choose:

-   Use [`CKModifyRecordsOperation.RecordSavePolicy.ifServerRecordUnchanged`](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy/ifserverrecordunchanged) to only save a record when the change tag of the local copy matches that of the server’s copy. If the server record’s change tag is more recent, CloudKit discards the save and returns a [`CKError.Code.serverRecordChanged`](/documentation/cloudkit/ckerror/code/serverrecordchanged) error.
    
-   Use [`CKModifyRecordsOperation.RecordSavePolicy.changedKeys`](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy/changedkeys) to save only the fields of the record that contain changes. The server doesn’t compare record change tags when using this policy.
    
-   Use [`CKModifyRecordsOperation.RecordSavePolicy.allKeys`](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy/allkeys) to save every field of the record, even those without changes. The server doesn’t compare record change tags when using this policy.
    

If you change the property’s value, do so before you execute the operation or submit the operation to a queue. The default value is [`CKModifyRecordsOperation.RecordSavePolicy.ifServerRecordUnchanged`](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy/ifserverrecordunchanged).

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

---
## save policies
# CKModifyRecordsOperation.RecordSavePolicy.ifServerRecordUnchanged | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordsOperation](/documentation/cloudkit/ckmodifyrecordsoperation)
-   [CKModifyRecordsOperation.RecordSavePolicy](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy)
-   CKModifyRecordsOperation.RecordSavePolicy.ifServerRecordUnchanged

Case

# CKModifyRecordsOperation.RecordSavePolicy.ifServerRecordUnchanged

A policy that instructs CloudKit to only proceed if the record’s change tag matches that of the server’s copy.

```
case ifServerRecordUnchanged
```

## [Discussion](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy/ifserverrecordunchanged#Discussion)

The server maintains a change tag for each record automatically. When you fetch a record, that change tag accompanies the rest of the record’s data. If the change tag in your local record matches the change tag of the record on the server, the save operation proceeds normally. If the server record contains a newer change tag, CloudKit doesn’t save the record and reports a [`CKError.Code.serverRecordChanged`](/documentation/cloudkit/ckerror/code/serverrecordchanged) error.

# CKModifyRecordsOperation.RecordSavePolicy.changedKeys

A policy that instructs CloudKit to save only the fields of a record that contain changes.

```
case changedKeys
```

# CKModifyRecordsOperation.RecordSavePolicy.allKeys

A policy that instructs CloudKit to save all keys of a record, even those without changes.

```
case allKeys
```

## [Discussion](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy/allkeys#Discussion)

This policy causes CloudKit to overwrite any existing values on the server. It’s possible for a server record to contain keys that aren’t present locally. Another client might add keys to the record after you fetch it. Also, if you use the [`desiredKeys`](/documentation/cloudkit/ckfetchrecordsoperation/desiredkeys-34l1l) property to request a subset of keys during a fetch operation, saving that same record modifies only those keys that you include in the fetch and any keys you add to the record after that.

## initializers
# init(rawValue:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordsOperation](/documentation/cloudkit/ckmodifyrecordsoperation)
-   [CKModifyRecordsOperation.RecordSavePolicy](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy)
-   init(rawValue:)

Initializer

# init(rawValue:)

```
init?(rawValue: Int)
```

Current page is init(rawValue:)

---
### Processing the Modify Record Results
# perRecordProgressBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordsOperation](/documentation/cloudkit/ckmodifyrecordsoperation)
-   perRecordProgressBlock

Instance Property

# perRecordProgressBlock

The closure to execute with progress information for individual records.

```
var perRecordProgressBlock: ((CKRecord, Double) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/ckmodifyrecordsoperation/perrecordprogressblock#Discussion)

This property is a closure that returns no value and has the following parameters:

-   The record that CloudKit saves.
    
-   The amount of data, as a percentage, that CloudKit saves for the record. The range is `0.0` to `1.0`, where `0.0` indicates that CloudKit hasn’t saved any data, and `1.0` means that CloudKit has saved the entire record.
    

The modify records operation executes this closure one or more times for each record in the [`recordsToSave`](/documentation/cloudkit/ckmodifyrecordsoperation/recordstosave) property. Each time the closure executes, it executes serially with respect to the other progress closures of the operation. You can use this closure to track the ongoing progress of the operation.

If you intend to use this closure to process results, set it before you execute the operation or add the operation to a queue.
## Instance Properties
# modifyRecordsResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordsOperation](/documentation/cloudkit/ckmodifyrecordsoperation)
-   modifyRecordsResultBlock

Instance Property

# modifyRecordsResultBlock

```
var modifyRecordsResultBlock: ((Result<Void, any Error>) -> Void)? { get set }
```

Current page is modifyRecordsResultBlock

# perRecordDeleteBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordsOperation](/documentation/cloudkit/ckmodifyrecordsoperation)
-   perRecordDeleteBlock

Instance Property

# perRecordDeleteBlock

```
var perRecordDeleteBlock: ((CKRecord.ID, Result<Void, any Error>) -> Void)? { get set }
```

Current page is perRecordDeleteBlock

# perRecordSaveBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordsOperation](/documentation/cloudkit/ckmodifyrecordsoperation)
-   perRecordSaveBlock

Instance Property

# perRecordSaveBlock

```
var perRecordSaveBlock: ((CKRecord.ID, Result<CKRecord, any Error>) -> Void)? { get set }
```

Current page is perRecordSaveBlock