# CKModifyRecordZonesOperation

An operation that modifies one or more record zones.

```
class CKModifyRecordZonesOperation
```

## [Overview](/documentation/cloudkit/ckmodifyrecordzonesoperation#overview)

After you create one or more record zones, use this operation to save those zones to the database. You can also use the operation to delete record zones and their records.

If you assign a handler to the [`completionBlock`](/documentation/Foundation/Operation/completionBlock) property of the operation, CloudKit calls the handler after the operation executes and returns its results. Use the handler to perform housekeeping tasks for the operation, but don’t use it to process the results of the operation. The handler you provide should manage any failures of the operation, whether due to an error or an explicit cancellation.

## [Topics](/documentation/cloudkit/ckmodifyrecordzonesoperation#topics)

### [Creating a Modify Zones Operation](/documentation/cloudkit/ckmodifyrecordzonesoperation#Creating-a-Modify-Zones-Operation)

[`convenience init(recordZonesToSave: [CKRecordZone]?, recordZoneIDsToDelete: [CKRecordZone.ID]?)`](/documentation/cloudkit/ckmodifyrecordzonesoperation/init\(recordzonestosave:recordzoneidstodelete:\))

Creates an operation for modifying the specified record zones.

[`init()`](/documentation/cloudkit/ckmodifyrecordzonesoperation/init\(\))

Creates an empty modify record zones operation.

### [Configuring the Modify Zones Operation](/documentation/cloudkit/ckmodifyrecordzonesoperation#Configuring-the-Modify-Zones-Operation)

[`var recordZonesToSave: [CKRecordZone]?`](/documentation/cloudkit/ckmodifyrecordzonesoperation/recordzonestosave)

The record zones to save to the database.

[`var recordZoneIDsToDelete: [CKRecordZone.ID]?`](/documentation/cloudkit/ckmodifyrecordzonesoperation/recordzoneidstodelete)

The IDs of the record zones to delete permanently from the database.

### [Processing the Modify Zones Results](/documentation/cloudkit/ckmodifyrecordzonesoperation#Processing-the-Modify-Zones-Results)

[`var modifyRecordZonesCompletionBlock: (([CKRecordZone]?, [CKRecordZone.ID]?, (any Error)?) -> Void)?`](/documentation/cloudkit/ckmodifyrecordzonesoperation/modifyrecordzonescompletionblock)

The closure to execute after CloudKit modifies all of the record zones.

Deprecated

### [Instance Properties](/documentation/cloudkit/ckmodifyrecordzonesoperation#Instance-Properties)

[`var modifyRecordZonesResultBlock: ((Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/ckmodifyrecordzonesoperation/modifyrecordzonesresultblock)

[`var perRecordZoneDeleteBlock: ((CKRecordZone.ID, Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/ckmodifyrecordzonesoperation/perrecordzonedeleteblock-6c82y)

[`var perRecordZoneSaveBlock: ((CKRecordZone.ID, Result<CKRecordZone, any Error>) -> Void)?`](/documentation/cloudkit/ckmodifyrecordzonesoperation/perrecordzonesaveblock-1m45y)

## [Relationships](/documentation/cloudkit/ckmodifyrecordzonesoperation#relationships)

### [Inherits From](/documentation/cloudkit/ckmodifyrecordzonesoperation#inherits-from)

-   [`CKDatabaseOperation`](/documentation/cloudkit/ckdatabaseoperation)

### [Conforms To](/documentation/cloudkit/ckmodifyrecordzonesoperation#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)
# init(recordZonesToSave:recordZoneIDsToDelete:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordZonesOperation](/documentation/cloudkit/ckmodifyrecordzonesoperation)
-   init(recordZonesToSave:recordZoneIDsToDelete:)

Initializer

# init(recordZonesToSave:recordZoneIDsToDelete:)

Creates an operation for modifying the specified record zones.

```
convenience init(
    recordZonesToSave: [CKRecordZone]? = nil,
    recordZoneIDsToDelete: [CKRecordZone.ID]? = nil
)
```

## [Parameters](/documentation/cloudkit/ckmodifyrecordzonesoperation/init\(recordzonestosave:recordzoneidstodelete:\)#parameters)

`recordZonesToSave`

The record zones to save. You can specify `nil` for this parameter.

`recordZoneIDsToDelete`

The IDs of the record zones to delete. You can specify `nil` for this parameter.

## [Discussion](/documentation/cloudkit/ckmodifyrecordzonesoperation/init\(recordzonestosave:recordzoneidstodelete:\)#Discussion)

The record zones you intend to save or delete must all reside in the same database, which you specify when you configure the operation. If you delete a record zone, CloudKit deletes any records it contains.
# init() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordZonesOperation](/documentation/cloudkit/ckmodifyrecordzonesoperation)
-   init()

Initializer

# init()

Creates an empty modify record zones operation.

```
init()
```

## [Discussion](/documentation/cloudkit/ckmodifyrecordzonesoperation/init\(\)#Discussion)

You must set at least one of the [`recordZonesToSave`](/documentation/cloudkit/ckmodifyrecordzonesoperation/recordzonestosave) or [`recordZoneIDsToDelete`](/documentation/cloudkit/ckmodifyrecordzonesoperation/recordzoneidstodelete) properties before you execute the operation.
# recordZonesToSave | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordZonesOperation](/documentation/cloudkit/ckmodifyrecordzonesoperation)
-   recordZonesToSave

Instance Property

# recordZonesToSave

The record zones to save to the database.

```
var recordZonesToSave: [CKRecordZone]? { get set }
```

## [Discussion](/documentation/cloudkit/ckmodifyrecordzonesoperation/recordzonestosave#Discussion)

The initial value of the property is the array that you provide to the [`initWithRecordZonesToSave:recordZoneIDsToDelete:`](/documentation/cloudkit/ckmodifyrecordzonesoperation/initwithrecordzonestosave:recordzoneidstodelete:) method. You can modify this array as necessary before you execute the operation. The record zones must all target the same database. You can specify `nil`, or an empty array, for this property.

If you intend to change the value of this property, do so before you execute the operation or submit the operation to a queue.
# recordZoneIDsToDelete | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordZonesOperation](/documentation/cloudkit/ckmodifyrecordzonesoperation)
-   recordZoneIDsToDelete

Instance Property

# recordZoneIDsToDelete

The IDs of the record zones to delete permanently from the database.

```
var recordZoneIDsToDelete: [CKRecordZone.ID]? { get set }
```

## [Discussion](/documentation/cloudkit/ckmodifyrecordzonesoperation/recordzoneidstodelete#Discussion)

The initial value of the property is the array of zone IDs that you provide to the [`initWithRecordZonesToSave:recordZoneIDsToDelete:`](/documentation/cloudkit/ckmodifyrecordzonesoperation/initwithrecordzonestosave:recordzoneidstodelete:) method. You can modify this array as necessary before you execute the operation. The record zones must all target the same database. You can specify `nil`, or an empty array, for this property.

If you intend to change the value of this property, do so before you execute the operation or submit the operation to a queue.
# modifyRecordZonesCompletionBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordZonesOperation](/documentation/cloudkit/ckmodifyrecordzonesoperation)
-   modifyRecordZonesCompletionBlock Deprecated

Instance Property

# modifyRecordZonesCompletionBlock

The closure to execute after CloudKit modifies all of the record zones.

```
var modifyRecordZonesCompletionBlock: (([CKRecordZone]?, [CKRecordZone.ID]?, (any Error)?) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/ckmodifyrecordzonesoperation/modifyrecordzonescompletionblock#Discussion)

This property is a closure that returns no value and has the following parameters:

-   The record zones that CloudKit saves.
    
-   The IDs of the record zones that CloudKit deletes.
    
-   If CloudKit can’t modify any of the record zones, this parameter provides information about the failure; otherwise, it’s `nil`.
    

The closure executes once, and represents your only opportunity to process the results.

The closure reports an error of type [`CKError.Code.partialFailure`](/documentation/cloudkit/ckerror/code/partialfailure) when it modifies only some of the record zones successfully. The [`userInfo`](/documentation/Foundation/NSError/userInfo) dictionary of the error contains a [`CKPartialErrorsByItemIDKey`](/documentation/cloudkit/ckpartialerrorsbyitemidkey) key that has a dictionary as its value. The keys of the dictionary are the IDs of the record zones that the operation can’t modify, and the corresponding values are errors that contain information about the failures.

If you intend to use this closure to process the results, set it before you execute the operation or submit the operation to a queue.

Current page is modifyRecordZonesCompletionBlock

---
### instance properties
# modifyRecordZonesResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordZonesOperation](/documentation/cloudkit/ckmodifyrecordzonesoperation)
-   modifyRecordZonesResultBlock

Instance Property

# modifyRecordZonesResultBlock

```
var modifyRecordZonesResultBlock: ((Result<Void, any Error>) -> Void)? { get set }
```

Current page is modifyRecordZonesResultBlock

# perRecordZoneDeleteBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordZonesOperation](/documentation/cloudkit/ckmodifyrecordzonesoperation)
-   perRecordZoneDeleteBlock

Instance Property

# perRecordZoneDeleteBlock

```
var perRecordZoneDeleteBlock: ((CKRecordZone.ID, Result<Void, any Error>) -> Void)? { get set }
```

Current page is perRecordZoneDeleteBlock

# perRecordZoneSaveBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifyRecordZonesOperation](/documentation/cloudkit/ckmodifyrecordzonesoperation)
-   perRecordZoneSaveBlock

Instance Property

# perRecordZoneSaveBlock

```
var perRecordZoneSaveBlock: ((CKRecordZone.ID, Result<CKRecordZone, any Error>) -> Void)? { get set }
```

Current page is perRecordZoneSaveBlock