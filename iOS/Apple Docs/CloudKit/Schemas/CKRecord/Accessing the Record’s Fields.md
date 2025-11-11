# object(forKey:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   object(forKey:)

Instance Method

# object(forKey:)

Returns the object that the record stores for the specified key.

```
func object(forKey key: CKRecord.FieldKey) -> (any __CKRecordObjCValue)?
```

## [Parameters](/documentation/cloudkit/ckrecord/object\(forkey:\)#parameters)

`key`

The string that identifies a field in the record. A key must consist of one or more alphanumeric characters and must start with a letter. CloudKit permits the use of underscores, but not spaces.

## [Return Value](/documentation/cloudkit/ckrecord/object\(forkey:\)#return-value)

The object for the specified key, or `nil` if no such key exists in the record.

## [Discussion](/documentation/cloudkit/ckrecord/object\(forkey:\)#Discussion)

New records don’t contain any keys or values. Values are always one of the data types in [Supported Data Types](/documentation/cloudkit/ckrecord#Supported-Data-Types).

You access the fields of a `CKRecord` object the same way you access key-value pairs in a dictionary. The `CKRecord` class defines the [`objectForKey:`](/documentation/cloudkit/ckrecord/objectforkey:) and [`setObject:forKey:`](/documentation/cloudkit/ckrecord/setobject:forkey:) methods for getting and setting values. It also supports dictionary index notation. The following example shows how to use both techniques to set a `firstName` field and retrieve a `lastName` field from a record:

```
// Equivalent ways to get a value.
var hiredAt = record.object(forKey: "hiredAt")
hiredAt = record["hiredAt"]
```

## [See Also](/documentation/cloudkit/ckrecord/object\(forkey:\)#see-also)

### [Accessing the Record’s Fields](/documentation/cloudkit/ckrecord/object\(forkey:\)#Accessing-the-Records-Fields)

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

[`protocol CKRecordKeyValueSetting`](/documentation/cloudkit/ckrecordkeyvaluesetting)

A protocol for managing the key-value pairs of a CloudKit record.

[`typealias CKRecordValue`](/documentation/cloudkit/ckrecordvalue-swift.typealias)

A data type for objects that CloudKit stores on the server.

Current page is object(forKey:)

---
# subscript(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   subscript(\_:)

Instance Subscript

# subscript(\_:)

Returns the object that the record stores for the specified key.

```
subscript(key: String) -> (any __CKRecordObjCValue)? { get set }
```

## [Parameters](/documentation/cloudkit/ckrecord/subscript\(_:\)-51whk#parameters)

`key`

The string that identifies a field in the record. A key must consist of one or more alphanumeric characters and must start with a letter. CloudKit permits the use of underscores, but not spaces.

## [Return Value](/documentation/cloudkit/ckrecord/subscript\(_:\)-51whk#return-value)

The object for the specified key, or `nil` if no such key exists in the record.

## [Discussion](/documentation/cloudkit/ckrecord/subscript\(_:\)-51whk#Discussion)

## [See Also](/documentation/cloudkit/ckrecord/subscript\(_:\)-51whk#see-also)

### [Accessing the Record’s Fields](/documentation/cloudkit/ckrecord/subscript\(_:\)-51whk#Accessing-the-Records-Fields)

[`func object(forKey: CKRecord.FieldKey) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecord/object\(forkey:\))

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

[`protocol CKRecordKeyValueSetting`](/documentation/cloudkit/ckrecordkeyvaluesetting)

A protocol for managing the key-value pairs of a CloudKit record.

[`typealias CKRecordValue`](/documentation/cloudkit/ckrecordvalue-swift.typealias)

A data type for objects that CloudKit stores on the server.

Current page is subscript(\_:)

---
# subscript(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   subscript(\_:)

Instance Subscript

# subscript(\_:)

Returns the object that the record stores for the specified key.

```
subscript(key: CKRecord.FieldKey) -> (any __CKRecordObjCValue)? { get set }
```

## [Parameters](/documentation/cloudkit/ckrecord/subscript\(_:\)-4g91i#parameters)

`key`

The string that identifies a field in the record. A key must consist of one or more alphanumeric characters and must start with a letter. CloudKit permits the use of underscores, but not spaces.

## [Return Value](/documentation/cloudkit/ckrecord/subscript\(_:\)-4g91i#return-value)

The object for the specified key, or `nil` if no such key exists in the record.

## [See Also](/documentation/cloudkit/ckrecord/subscript\(_:\)-4g91i#see-also)

### [Accessing the Record’s Fields](/documentation/cloudkit/ckrecord/subscript\(_:\)-4g91i#Accessing-the-Records-Fields)

[`func object(forKey: CKRecord.FieldKey) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecord/object\(forkey:\))

Returns the object that the record stores for the specified key.

[`subscript(String) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecord/subscript\(_:\)-51whk)

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

[`protocol CKRecordKeyValueSetting`](/documentation/cloudkit/ckrecordkeyvaluesetting)

A protocol for managing the key-value pairs of a CloudKit record.

[`typealias CKRecordValue`](/documentation/cloudkit/ckrecordvalue-swift.typealias)

A data type for objects that CloudKit stores on the server.

Current page is subscript(\_:)

---
# setObject(\_:forKey:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   setObject(\_:forKey:)

Instance Method

# setObject(\_:forKey:)

Stores an object in the record using the specified key.

```
func setObject(
    _ object: (any __CKRecordObjCValue)?,
    forKey key: CKRecord.FieldKey
)
```

## [Parameters](/documentation/cloudkit/ckrecord/setobject\(_:forkey:\)#parameters)

`object`

The object to store using the specified key. It must be one of the data types in [Supported Data Types](/documentation/cloudkit/ckrecord#Supported-Data-Types). You receive an error if you use a data type that CloudKit doesn’t support. If you specify `nil`, CloudKit removes any object that the record associates with the key.

`key`

The key to associate with `object`. Use this key to retrieve the value later. A key must consist of one or more alphanumeric characters and must start with a letter. CloudKit permits the use of underscores, but not spaces. Avoid using a key that matches the name of any property of `CKRecord`.

## [Discussion](/documentation/cloudkit/ckrecord/setobject\(_:forkey:\)#Discussion)

If the specified key already exists in the record, CloudKit deletes its previous value and replaces it with the one in the `object` parameter. This change affects only the local copy of the record. You must save the record to the server again before the change becomes available to other clients.

If the type of the `object` parameter differs from the type of the object that’s on the server, you encounter an error when you attempt to save this record to the server. For example, if the current value is an [`NSString`](/documentation/Foundation/NSString) object, you receive an error if you change the value to an [`NSNumber`](/documentation/Foundation/NSNumber) object and save the record.

You access the fields of a `CKRecord` object the same way you access key-value pairs in a dictionary. The `CKRecord` class defines the [`objectForKey:`](/documentation/cloudkit/ckrecord/objectforkey:) and [`setObject:forKey:`](/documentation/cloudkit/ckrecord/setobject:forkey:) methods for getting and setting values. It also supports dictionary index notation. The following example shows how to use both techniques to set a `firstName` field and get a `lastName` field from a record:

```
// Equivalent ways to set a value.
let record = CKRecord(recordType: "Employee")
record.setObject(NSDate(), forKey: "hiredAt")
record["hiredAt"] = NSDate()
```

## [See Also](/documentation/cloudkit/ckrecord/setobject\(_:forkey:\)#see-also)

### [Accessing the Record’s Fields](/documentation/cloudkit/ckrecord/setobject\(_:forkey:\)#Accessing-the-Records-Fields)

[`func object(forKey: CKRecord.FieldKey) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecord/object\(forkey:\))

Returns the object that the record stores for the specified key.

[`subscript(String) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecord/subscript\(_:\)-51whk)

Returns the object that the record stores for the specified key.

[`subscript(CKRecord.FieldKey) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecord/subscript\(_:\)-4g91i)

Returns the object that the record stores for the specified key.

[`func allKeys() -> [CKRecord.FieldKey]`](/documentation/cloudkit/ckrecord/allkeys\(\))

Returns an array of the record’s keys.

[`func changedKeys() -> [CKRecord.FieldKey]`](/documentation/cloudkit/ckrecord/changedkeys\(\))

Returns an array of keys with recent changes to their values.

[`struct CKRecordKeyValueIterator`](/documentation/cloudkit/ckrecordkeyvalueiterator)

An iterator of the record’s key-value pairs.

[`protocol CKRecordValueProtocol`](/documentation/cloudkit/ckrecordvalueprotocol)

A description of a CloudKit record value.

[`protocol CKRecordKeyValueSetting`](/documentation/cloudkit/ckrecordkeyvaluesetting)

A protocol for managing the key-value pairs of a CloudKit record.

[`typealias CKRecordValue`](/documentation/cloudkit/ckrecordvalue-swift.typealias)

A data type for objects that CloudKit stores on the server.

Current page is setObject(\_:forKey:)

---
# allKeys() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   allKeys()

Instance Method

# allKeys()

Returns an array of the record’s keys.

```
func allKeys() -> [CKRecord.FieldKey]
```

## [Return Value](/documentation/cloudkit/ckrecord/allkeys\(\)#return-value)

An array of keys, or an empty array if the record doesn’t contain any keys.

## [Discussion](/documentation/cloudkit/ckrecord/allkeys\(\)#Discussion)

The array contains only those keys with values that aren’t `nil`.

## [See Also](/documentation/cloudkit/ckrecord/allkeys\(\)#see-also)

### [Accessing the Record’s Fields](/documentation/cloudkit/ckrecord/allkeys\(\)#Accessing-the-Records-Fields)

[`func object(forKey: CKRecord.FieldKey) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecord/object\(forkey:\))

Returns the object that the record stores for the specified key.

[`subscript(String) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecord/subscript\(_:\)-51whk)

Returns the object that the record stores for the specified key.

[`subscript(CKRecord.FieldKey) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecord/subscript\(_:\)-4g91i)

Returns the object that the record stores for the specified key.

[`func setObject((any __CKRecordObjCValue)?, forKey: CKRecord.FieldKey)`](/documentation/cloudkit/ckrecord/setobject\(_:forkey:\))

Stores an object in the record using the specified key.

[`func changedKeys() -> [CKRecord.FieldKey]`](/documentation/cloudkit/ckrecord/changedkeys\(\))

Returns an array of keys with recent changes to their values.

[`struct CKRecordKeyValueIterator`](/documentation/cloudkit/ckrecordkeyvalueiterator)

An iterator of the record’s key-value pairs.

[`protocol CKRecordValueProtocol`](/documentation/cloudkit/ckrecordvalueprotocol)

A description of a CloudKit record value.

[`protocol CKRecordKeyValueSetting`](/documentation/cloudkit/ckrecordkeyvaluesetting)

A protocol for managing the key-value pairs of a CloudKit record.

[`typealias CKRecordValue`](/documentation/cloudkit/ckrecordvalue-swift.typealias)

A data type for objects that CloudKit stores on the server.

Current page is allKeys()

---
# changedKeys() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   changedKeys()

Instance Method

# changedKeys()

Returns an array of keys with recent changes to their values.

```
func changedKeys() -> [CKRecord.FieldKey]
```

## [Return Value](/documentation/cloudkit/ckrecord/changedkeys\(\)#return-value)

An array of keys with changed values since downloading or saving the record. If there aren’t any changed keys, this method returns an empty array.

## [See Also](/documentation/cloudkit/ckrecord/changedkeys\(\)#see-also)

### [Accessing the Record’s Fields](/documentation/cloudkit/ckrecord/changedkeys\(\)#Accessing-the-Records-Fields)

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

[`struct CKRecordKeyValueIterator`](/documentation/cloudkit/ckrecordkeyvalueiterator)

An iterator of the record’s key-value pairs.

[`protocol CKRecordValueProtocol`](/documentation/cloudkit/ckrecordvalueprotocol)

A description of a CloudKit record value.

[`protocol CKRecordKeyValueSetting`](/documentation/cloudkit/ckrecordkeyvaluesetting)

A protocol for managing the key-value pairs of a CloudKit record.

[`typealias CKRecordValue`](/documentation/cloudkit/ckrecordvalue-swift.typealias)

A data type for objects that CloudKit stores on the server.

Current page is changedKeys()

---
# CKRecordKeyValueIterator | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKRecordKeyValueIterator

Structure

# CKRecordKeyValueIterator

An iterator of the record’s key-value pairs.

```
struct CKRecordKeyValueIterator
```

## [Relationships](/documentation/cloudkit/ckrecordkeyvalueiterator#relationships)

### [Conforms To](/documentation/cloudkit/ckrecordkeyvalueiterator#conforms-to)

-   [`IteratorProtocol`](/documentation/Swift/IteratorProtocol)

## [See Also](/documentation/cloudkit/ckrecordkeyvalueiterator#see-also)

### [Accessing the Record’s Fields](/documentation/cloudkit/ckrecordkeyvalueiterator#Accessing-the-Records-Fields)

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

[`protocol CKRecordValueProtocol`](/documentation/cloudkit/ckrecordvalueprotocol)

A description of a CloudKit record value.

[`protocol CKRecordKeyValueSetting`](/documentation/cloudkit/ckrecordkeyvaluesetting)

A protocol for managing the key-value pairs of a CloudKit record.

[`typealias CKRecordValue`](/documentation/cloudkit/ckrecordvalue-swift.typealias)

A data type for objects that CloudKit stores on the server.

Current page is CKRecordKeyValueIterator

---
# CKRecordValueProtocol | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKRecordValueProtocol

Protocol

# CKRecordValueProtocol

A description of a CloudKit record value.

```
protocol CKRecordValueProtocol
```

## [Relationships](/documentation/cloudkit/ckrecordvalueprotocol#relationships)

### [Conforming Types](/documentation/cloudkit/ckrecordvalueprotocol#conforming-types)

-   [`CKAsset`](/documentation/cloudkit/ckasset)
-   [`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference)

## [See Also](/documentation/cloudkit/ckrecordvalueprotocol#see-also)

### [Accessing the Record’s Fields](/documentation/cloudkit/ckrecordvalueprotocol#Accessing-the-Records-Fields)

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

[`protocol CKRecordKeyValueSetting`](/documentation/cloudkit/ckrecordkeyvaluesetting)

A protocol for managing the key-value pairs of a CloudKit record.

[`typealias CKRecordValue`](/documentation/cloudkit/ckrecordvalue-swift.typealias)

A data type for objects that CloudKit stores on the server.

Current page is CKRecordValueProtocol

---
# CKRecordValue | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKRecordValue

Type Alias

# CKRecordValue

A data type for objects that CloudKit stores on the server.

```
typealias CKRecordValue = __CKRecordObjCValue
```

## [See Also](/documentation/cloudkit/ckrecordvalue-swift.typealias#see-also)

### [Accessing the Record’s Fields](/documentation/cloudkit/ckrecordvalue-swift.typealias#Accessing-the-Records-Fields)

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

[`protocol CKRecordKeyValueSetting`](/documentation/cloudkit/ckrecordkeyvaluesetting)

A protocol for managing the key-value pairs of a CloudKit record.

Current page is CKRecordValue