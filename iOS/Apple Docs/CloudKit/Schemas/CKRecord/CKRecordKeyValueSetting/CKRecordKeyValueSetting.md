# CKRecordKeyValueSetting | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKRecordKeyValueSetting

Protocol

# CKRecordKeyValueSetting

A protocol for managing the key-value pairs of a CloudKit record.

```
protocol CKRecordKeyValueSetting : NSObjectProtocol
```

## [Topics](/documentation/cloudkit/ckrecordkeyvaluesetting#topics)

### [Accessing a Record’s Fields](/documentation/cloudkit/ckrecordkeyvaluesetting#Accessing-a-Records-Fields)

[`func object(forKey: String) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecordkeyvaluesetting/object\(forkey:\))

Returns the object that the record stores for the specified key.

**Required**

[`subscript(String) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecordkeyvaluesetting/subscript\(_:\))

Returns the object that the record stores for the specified key.

**Required** Default implementations provided.

[`func setObject((any __CKRecordObjCValue)?, forKey: String)`](/documentation/cloudkit/ckrecordkeyvaluesetting/setobject\(_:forkey:\))

Stores an object in the record using the specified key.

**Required**

[`func allKeys() -> [String]`](/documentation/cloudkit/ckrecordkeyvaluesetting/allkeys\(\))

Returns an array of the record’s keys.

**Required**

[`func changedKeys() -> [String]`](/documentation/cloudkit/ckrecordkeyvaluesetting/changedkeys\(\))

Returns an array of keys with recent changes to their values.

**Required**

## [Relationships](/documentation/cloudkit/ckrecordkeyvaluesetting#relationships)

### [Inherits From](/documentation/cloudkit/ckrecordkeyvaluesetting#inherits-from)

-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)

### [Conforming Types](/documentation/cloudkit/ckrecordkeyvaluesetting#conforming-types)

-   [`CKRecord`](/documentation/cloudkit/ckrecord)
-   [`CKShare`](/documentation/cloudkit/ckshare)

## [See Also](/documentation/cloudkit/ckrecordkeyvaluesetting#see-also)

### [Accessing the Record’s Fields](/documentation/cloudkit/ckrecordkeyvaluesetting#Accessing-the-Records-Fields)

[`func object(forKey: CKRecord.FieldKey) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecord/object\(forkey:\))

Returns the object that the record stores for the specified key.

[`subscript(String) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecord/subscript\(_:\)-51whk)

Returns the object that the record stores for the specified key.

[`subscript(CKRecord.FieldKey) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecord/subscript\(_:\)-4g91i)

Returns the object that the record stores for the specified key.

[`func setObject((any __CKRecordObjCValue)?, forKey: CKRecord.FieldKey)`](/documentation/cloudkit/ckrecord/setobject\(_:forkey:\))

Stores an object in the record using the specified key.

[`func allKeys() -> [CKRecord.FieldKey]`](/documentation/cloudkit/ckrecord/allkeys\(\))

Returns an array of the record’s keys.

[`func changedKeys() -> [CKRecord.FieldKey]`](/documentation/cloudkit/ckrecord/changedkeys\(\))

Returns an array of keys with recent changes to their values.

[`struct CKRecordKeyValueIterator`](/documentation/cloudkit/ckrecordkeyvalueiterator)

An iterator of the record’s key-value pairs.

[`protocol CKRecordValueProtocol`](/documentation/cloudkit/ckrecordvalueprotocol)

A description of a CloudKit record value.

[`typealias CKRecordValue`](/documentation/cloudkit/ckrecordvalue-swift.typealias)

A data type for objects that CloudKit stores on the server.

Current page is CKRecordKeyValueSetting