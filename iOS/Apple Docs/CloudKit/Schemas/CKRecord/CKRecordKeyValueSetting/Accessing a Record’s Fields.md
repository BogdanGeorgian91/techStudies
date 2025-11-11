# object(forKey:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordKeyValueSetting](/documentation/cloudkit/ckrecordkeyvaluesetting)
-   object(forKey:)

Instance Method

# object(forKey:)

Returns the object that the record stores for the specified key.

```
func object(forKey key: String) -> (any __CKRecordObjCValue)?
```

**Required**

## [Parameters](/documentation/cloudkit/ckrecordkeyvaluesetting/object\(forkey:\)#parameters)

`key`

The string that identifies a field in the record. A key must consist of one or more alphanumeric characters and must start with a letter. CloudKit permits the use of underscores, but not spaces.

## [Return Value](/documentation/cloudkit/ckrecordkeyvaluesetting/object\(forkey:\)#return-value)

The object for the specified key, or `nil` if no such key exists in the record.

## [See Also](/documentation/cloudkit/ckrecordkeyvaluesetting/object\(forkey:\)#see-also)

### [Accessing a Record’s Fields](/documentation/cloudkit/ckrecordkeyvaluesetting/object\(forkey:\)#Accessing-a-Records-Fields)

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

Current page is object(forKey:)

---
# subscript(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordKeyValueSetting](/documentation/cloudkit/ckrecordkeyvaluesetting)
-   subscript(\_:)

Instance Subscript

# subscript(\_:)

Returns the object that the record stores for the specified key.

```
subscript(key: String) -> (any __CKRecordObjCValue)? { get set }
```

**Required** Default implementations provided.

## [Parameters](/documentation/cloudkit/ckrecordkeyvaluesetting/subscript\(_:\)#parameters)

`key`

The string that identifies a field in the record. A key must consist of one or more alphanumeric characters and must start with a letter. CloudKit permits the use of underscores, but not spaces.

## [Return Value](/documentation/cloudkit/ckrecordkeyvaluesetting/subscript\(_:\)#return-value)

The object for the specified key, or `nil` if no such key exists in the record.

## [Default Implementations](/documentation/cloudkit/ckrecordkeyvaluesetting/subscript\(_:\)#default-implementations)

### [CKRecordKeyValueSetting Implementations](/documentation/cloudkit/ckrecordkeyvaluesetting/subscript\(_:\)#CKRecordKeyValueSetting-Implementations)

[`subscript<T>(CKRecord.FieldKey) -> T?`](/documentation/cloudkit/ckrecordkeyvaluesetting/subscript\(_:\)-4f12u)

Accesses the value for the specified key in the record.

[`subscript(CKRecord.FieldKey) -> (any CKRecordValueProtocol)?`](/documentation/cloudkit/ckrecordkeyvaluesetting/subscript\(_:\)-2uqmn)

Accesses the value for the specified key in the record.

## [See Also](/documentation/cloudkit/ckrecordkeyvaluesetting/subscript\(_:\)#see-also)

### [Accessing a Record’s Fields](/documentation/cloudkit/ckrecordkeyvaluesetting/subscript\(_:\)#Accessing-a-Records-Fields)

[`func object(forKey: String) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecordkeyvaluesetting/object\(forkey:\))

Returns the object that the record stores for the specified key.

**Required**

[`func setObject((any __CKRecordObjCValue)?, forKey: String)`](/documentation/cloudkit/ckrecordkeyvaluesetting/setobject\(_:forkey:\))

Stores an object in the record using the specified key.

**Required**

[`func allKeys() -> [String]`](/documentation/cloudkit/ckrecordkeyvaluesetting/allkeys\(\))

Returns an array of the record’s keys.

**Required**

[`func changedKeys() -> [String]`](/documentation/cloudkit/ckrecordkeyvaluesetting/changedkeys\(\))

Returns an array of keys with recent changes to their values.

**Required**

Current page is subscript(\_:)

---
# setObject(\_:forKey:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordKeyValueSetting](/documentation/cloudkit/ckrecordkeyvaluesetting)
-   setObject(\_:forKey:)

Instance Method

# setObject(\_:forKey:)

Stores an object in the record using the specified key.

```
func setObject(
    _ object: (any __CKRecordObjCValue)?,
    forKey key: String
)
```

**Required**

## [Parameters](/documentation/cloudkit/ckrecordkeyvaluesetting/setobject\(_:forkey:\)#parameters)

`object`

The object to store using the specified key. It must be one of the data types in [Supported Data Types](/documentation/cloudkit/ckrecord#Supported-Data-Types). You receive an error if you use a data type that CloudKit doesn’t support. If you specify `nil`, CloudKit removes any object that the record associates with the key.

`key`

The key to associate with `object`. Use this key to retrieve the value later. A key must consist of one or more alphanumeric characters and must start with a letter. CloudKit permits the use of underscores, but not spaces. Avoid using a key that matches the name of any property of `CKRecord`.

## [See Also](/documentation/cloudkit/ckrecordkeyvaluesetting/setobject\(_:forkey:\)#see-also)

### [Accessing a Record’s Fields](/documentation/cloudkit/ckrecordkeyvaluesetting/setobject\(_:forkey:\)#Accessing-a-Records-Fields)

[`func object(forKey: String) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecordkeyvaluesetting/object\(forkey:\))

Returns the object that the record stores for the specified key.

**Required**

[`subscript(String) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecordkeyvaluesetting/subscript\(_:\))

Returns the object that the record stores for the specified key.

**Required** Default implementations provided.

[`func allKeys() -> [String]`](/documentation/cloudkit/ckrecordkeyvaluesetting/allkeys\(\))

Returns an array of the record’s keys.

**Required**

[`func changedKeys() -> [String]`](/documentation/cloudkit/ckrecordkeyvaluesetting/changedkeys\(\))

Returns an array of keys with recent changes to their values.

**Required**

Current page is setObject(\_:forKey:)

---
# allKeys() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordKeyValueSetting](/documentation/cloudkit/ckrecordkeyvaluesetting)
-   allKeys()

Instance Method

# allKeys()

Returns an array of the record’s keys.

```
func allKeys() -> [String]
```

**Required**

## [Return Value](/documentation/cloudkit/ckrecordkeyvaluesetting/allkeys\(\)#return-value)

An array of keys, or an empty array if the record doesn’t contain any keys.

## [See Also](/documentation/cloudkit/ckrecordkeyvaluesetting/allkeys\(\)#see-also)

### [Accessing a Record’s Fields](/documentation/cloudkit/ckrecordkeyvaluesetting/allkeys\(\)#Accessing-a-Records-Fields)

[`func object(forKey: String) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecordkeyvaluesetting/object\(forkey:\))

Returns the object that the record stores for the specified key.

**Required**

[`subscript(String) -> (any __CKRecordObjCValue)?`](/documentation/cloudkit/ckrecordkeyvaluesetting/subscript\(_:\))

Returns the object that the record stores for the specified key.

**Required** Default implementations provided.

[`func setObject((any __CKRecordObjCValue)?, forKey: String)`](/documentation/cloudkit/ckrecordkeyvaluesetting/setobject\(_:forkey:\))

Stores an object in the record using the specified key.

**Required**

[`func changedKeys() -> [String]`](/documentation/cloudkit/ckrecordkeyvaluesetting/changedkeys\(\))

Returns an array of keys with recent changes to their values.

**Required**

Current page is allKeys()

---
# changedKeys() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordKeyValueSetting](/documentation/cloudkit/ckrecordkeyvaluesetting)
-   changedKeys()

Instance Method

# changedKeys()

Returns an array of keys with recent changes to their values.

```
func changedKeys() -> [String]
```

**Required**

## [Return Value](/documentation/cloudkit/ckrecordkeyvaluesetting/changedkeys\(\)#return-value)

An array of keys with changed values since downloading or saving the record. If there aren’t any changed keys, this method returns an empty array.

## [See Also](/documentation/cloudkit/ckrecordkeyvaluesetting/changedkeys\(\)#see-also)

### [Accessing a Record’s Fields](/documentation/cloudkit/ckrecordkeyvaluesetting/changedkeys\(\)#Accessing-a-Records-Fields)

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

Current page is changedKeys()