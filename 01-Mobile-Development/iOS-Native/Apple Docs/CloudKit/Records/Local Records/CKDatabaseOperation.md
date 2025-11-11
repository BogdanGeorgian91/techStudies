# CKDatabaseOperation

The abstract base class for operations that act upon databases in CloudKit.

```
class CKDatabaseOperation
```

## [Overview](/documentation/cloudkit/ckdatabaseoperation#overview)

Database operations typically involve fetching and saving records and other database objects, as well as executing queries on the contents of the database. Use this class’s [`database`](/documentation/cloudkit/ckdatabaseoperation/database) property to tell the operation which database to use when you execute it. Don’t subclass this class or create instances of it. Instead, create instances of one of its concrete subclasses.

## [Topics](/documentation/cloudkit/ckdatabaseoperation#topics)

### [Accessing the Database](/documentation/cloudkit/ckdatabaseoperation#Accessing-the-Database)

[`var database: CKDatabase?`](/documentation/cloudkit/ckdatabaseoperation/database)

The database that the operation uses.

## [Relationships](/documentation/cloudkit/ckdatabaseoperation#relationships)

### [Inherits From](/documentation/cloudkit/ckdatabaseoperation#inherits-from)

-   [`CKOperation`](/documentation/cloudkit/ckoperation)

### [Inherited By](/documentation/cloudkit/ckdatabaseoperation#inherited-by)

-   [`CKFetchDatabaseChangesOperation`](/documentation/cloudkit/ckfetchdatabasechangesoperation)
-   [`CKFetchRecordChangesOperation`](/documentation/cloudkit/ckfetchrecordchangesoperation)
-   [`CKFetchRecordZoneChangesOperation`](/documentation/cloudkit/ckfetchrecordzonechangesoperation)
-   [`CKFetchRecordZonesOperation`](/documentation/cloudkit/ckfetchrecordzonesoperation)
-   [`CKFetchRecordsOperation`](/documentation/cloudkit/ckfetchrecordsoperation)
-   [`CKFetchSubscriptionsOperation`](/documentation/cloudkit/ckfetchsubscriptionsoperation)
-   [`CKFetchWebAuthTokenOperation`](/documentation/cloudkit/ckfetchwebauthtokenoperation)
-   [`CKModifyRecordZonesOperation`](/documentation/cloudkit/ckmodifyrecordzonesoperation)
-   [`CKModifyRecordsOperation`](/documentation/cloudkit/ckmodifyrecordsoperation)
-   [`CKModifySubscriptionsOperation`](/documentation/cloudkit/ckmodifysubscriptionsoperation)
-   [`CKQueryOperation`](/documentation/cloudkit/ckqueryoperation)

### [Conforms To](/documentation/cloudkit/ckdatabaseoperation#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

Current page is CKDatabaseOperation

---
# Accessing the database
# database | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKDatabaseOperation](/documentation/cloudkit/ckdatabaseoperation)
-   database

Instance Property

# database

The database that the operation uses.

```
var database: CKDatabase? { get set }
```

## [Discussion](/documentation/cloudkit/ckdatabaseoperation/database#Discussion)

For operations that you execute in a custom queue, use this property to specify the target database. Setting the database also sets the corresponding container, which it inherits from [`CKOperation`](/documentation/cloudkit/ckoperation). If this property’s value is `nil`, the operation targets the user’s private database.

The default value is `nil`.

Current page is database