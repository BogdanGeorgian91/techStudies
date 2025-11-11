# init(recordType:recordID:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   init(recordType:recordID:)

Initializer

# init(recordType:recordID:)

Creates a record using an ID that you provide.

```
convenience init(
    recordType: CKRecord.RecordType,
    recordID: CKRecord.ID = CKRecord.ID()
)
```

## [Parameters](/documentation/cloudkit/ckrecord/init\(recordtype:recordid:\)#parameters)

`recordType`

A string that represents the type of record that you want to create. You can’t change the record type after initialization. You define the record types that your app supports and use them to distinguish between records with different types of data. This parameter must not be `nil` or contain an empty string.

A record type must consist of one or more alphanumeric characters and must start with a letter. CloudKit permits the use of underscores, but not spaces.

`recordID`

The ID to assign to the record. When creating the ID, you can specify the zone where you want to store the record. The ID must be unique across all records and can’t be `nil`.

## [Discussion](/documentation/cloudkit/ckrecord/init\(recordtype:recordid:\)#Discussion)

Use this method to initialize a new record object with the specified ID. The newly created record contains no data.

Upon creation, record objects exist only in memory on the local device. Save the record using a [`CKModifyRecordsOperation`](/documentation/cloudkit/ckmodifyrecordsoperation) object or by using the [`save(_:completionHandler:)`](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-3tatz) method to transfer the record’s contents to the server.

## [See Also](/documentation/cloudkit/ckrecord/init\(recordtype:recordid:\)#see-also)

### [Creating a Record](/documentation/cloudkit/ckrecord/init\(recordtype:recordid:\)#Creating-a-Record)

[`typealias RecordType`](/documentation/cloudkit/ckrecord/recordtype-swift.typealias)

The data type that CloudKit requires for record types.

[`typealias FieldKey`](/documentation/cloudkit/ckrecord/fieldkey)

The data type that CloudKit requires for record field names.

[`convenience init(recordType: CKRecord.RecordType, zoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckrecord/init\(recordtype:zoneid:\))

Creates a record in the specified zone.

Deprecated

Current page is init(recordType:recordID:)

---
# CKRecord.RecordType | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   CKRecord.RecordType

Type Alias

# CKRecord.RecordType

The data type that CloudKit requires for record types.

```
typealias RecordType = String
```

## [See Also](/documentation/cloudkit/ckrecord/recordtype-swift.typealias#see-also)

### [Creating a Record](/documentation/cloudkit/ckrecord/recordtype-swift.typealias#Creating-a-Record)

[`convenience init(recordType: CKRecord.RecordType, recordID: CKRecord.ID)`](/documentation/cloudkit/ckrecord/init\(recordtype:recordid:\))

Creates a record using an ID that you provide.

[`typealias FieldKey`](/documentation/cloudkit/ckrecord/fieldkey)

The data type that CloudKit requires for record field names.

[`convenience init(recordType: CKRecord.RecordType, zoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckrecord/init\(recordtype:zoneid:\))

Creates a record in the specified zone.

Deprecated

Current page is CKRecord.RecordType

---
# CKRecord.FieldKey | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   CKRecord.FieldKey

Type Alias

# CKRecord.FieldKey

The data type that CloudKit requires for record field names.

```
typealias FieldKey = String
```

## [See Also](/documentation/cloudkit/ckrecord/fieldkey#see-also)

### [Creating a Record](/documentation/cloudkit/ckrecord/fieldkey#Creating-a-Record)

[`convenience init(recordType: CKRecord.RecordType, recordID: CKRecord.ID)`](/documentation/cloudkit/ckrecord/init\(recordtype:recordid:\))

Creates a record using an ID that you provide.

[`typealias RecordType`](/documentation/cloudkit/ckrecord/recordtype-swift.typealias)

The data type that CloudKit requires for record types.

[`convenience init(recordType: CKRecord.RecordType, zoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckrecord/init\(recordtype:zoneid:\))

Creates a record in the specified zone.

Deprecated

Current page is CKRecord.FieldKey

---
# init(recordType:zoneID:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   init(recordType:zoneID:) Deprecated

Initializer

# init(recordType:zoneID:)

Creates a record in the specified zone.

```
convenience init(
    recordType: CKRecord.RecordType,
    zoneID: CKRecordZone.ID
)
```

## [Parameters](/documentation/cloudkit/ckrecord/init\(recordtype:zoneid:\)#parameters)

`recordType`

A string that represents the type of record that you want to create. You can’t change the record type after initialization. You define the record types that your app supports and use them to distinguish between records with different types of data. This parameter must not be `nil` or contain an empty string.

A record type must consist of one or more alphanumeric characters and must start with a letter. CloudKit permits the use of underscores, but not spaces.

`zoneID`

The ID of the record zone where you want to store the record.

## [Discussion](/documentation/cloudkit/ckrecord/init\(recordtype:zoneid:\)#Discussion)

Use this method to initialize a new record object with the specified ID. The newly created record contains no data.

Upon creation, record objects exist only in memory on the local device. Save the record using a [`CKModifyRecordsOperation`](/documentation/cloudkit/ckmodifyrecordsoperation) object or by using the [`save(_:completionHandler:)`](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-3tatz) method to transfer the record’s contents to the server.

## [See Also](/documentation/cloudkit/ckrecord/init\(recordtype:zoneid:\)#see-also)

### [Creating a Record](/documentation/cloudkit/ckrecord/init\(recordtype:zoneid:\)#Creating-a-Record)

[`convenience init(recordType: CKRecord.RecordType, recordID: CKRecord.ID)`](/documentation/cloudkit/ckrecord/init\(recordtype:recordid:\))

Creates a record using an ID that you provide.

[`typealias RecordType`](/documentation/cloudkit/ckrecord/recordtype-swift.typealias)

The data type that CloudKit requires for record types.

[`typealias FieldKey`](/documentation/cloudkit/ckrecord/fieldkey)

The data type that CloudKit requires for record field names.

Current page is init(recordType:zoneID:)