# CKRecord | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKRecord

Class

# CKRecord

A collection of key-value pairs that store your app’s data.

```
class CKRecord
```

## [Mentioned in](/documentation/cloudkit/ckrecord#mentions)

[

Designing and Creating a CloudKit Database](/documentation/cloudkit/designing-and-creating-a-cloudkit-database)

[

Encrypting User Data](/documentation/cloudkit/encrypting-user-data)

## [Overview](/documentation/cloudkit/ckrecord#overview)

Records are the fundamental objects that manage data in CloudKit. You can define any number of record types for your app, with each record type corresponding to a different type of information. Within a record type, you then define one or more fields, each with a name and a value. Records can contain simple data types, such as strings and numbers, or more complex types, such as geographic locations or pointers to other records.

An important step in using CloudKit is defining the record types your app supports. A new record object doesn’t contain any keys or values. During development, you can add new keys and values at any time. The first time you set a value for a key and save the record, the server associates that type with the key for all records of the same type. The `CKRecord` class doesn’t impose these type constraints or do any local validation of a record’s contents. CloudKit enforces these constraints when you save the records.

Although records behave like dictionaries, there are limitations to the types of values you can assign to keys. The following are the object types that the `CKRecord` class supports. Attempting to specify objects of any other type results in failure. Fields of all types are searchable unless otherwise noted.

### [Supported Data Types](/documentation/cloudkit/ckrecord#Supported-Data-Types)

`CKRecord` fields support the following data types:

[`NSString`](/documentation/Foundation/NSString)

Stores relatively small amounts of text. Although strings themselves can be any length, use a [`CKAsset`](/documentation/cloudkit/ckasset) to store large amounts of text.

[`NSNumber`](/documentation/Foundation/NSNumber)

Stores any numerical information, including integers and floating-point numbers.

[`NSData`](/documentation/Foundation/NSData)

Stores arbitrary bytes of data. A typical use for data objects is to map the bytes that they contain to a `struct`. Don’t use data objects for storing large binary data files; use a [`CKAsset`](/documentation/cloudkit/ckasset) instead. Data fields aren’t searchable.

[`NSDate`](/documentation/Foundation/NSDate)

Stores day and time information in an accessible form.

[`NSArray`](/documentation/Foundation/NSArray)

Stores one or more objects of any other type in this table. You can store arrays of strings, arrays of numbers, arrays of references, and so on.

[`CLLocation`](/documentation/CoreLocation/CLLocation)

Stores geographic coordinate data. You use locations in conjunction with the Core Location framework and any other services that handle location information.

[`CKAsset`](/documentation/cloudkit/ckasset)

Associates a disk-based file with the record. Although assets have a close association with records, you manage them separately. For more information about using assets, see [`CKAsset`](/documentation/cloudkit/ckasset).

[`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference)

Creates a link to a related record. A reference stores the ID of the target record. The advantage of using a reference instead of storing the ID as a string is that references can initiate cascade deletions of dependent records. The disadvantage is that references can only link between records in the same record zone. For more information, see [`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference).

### [Defining Records](/documentation/cloudkit/ckrecord#Defining-Records)

The process for defining your record types depends entirely on your app and the data you’re trying to represent. It’s best to design records that encapsulate data for one unit of information. For example, you might use one record type to store an employee’s name, job title, and date of hire, and use a separate record type to store the employee’s address information. Using different record types lets you manage, manipulate, and validate the two types of information separately.

Use fields that contain [`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference) objects to establish relationships between different types of records. After you define your record types, use the iCloud Dashboard to set them up. During development, you can also create new record types programmatically.

### [Indexing the Fields of a Record](/documentation/cloudkit/ckrecord#Indexing-the-Fields-of-a-Record)

Indexes make it possible to search the contents of your records efficiently. During development, the server indexes all fields with data types it can use in the predicate of a query. This automatic indexing makes it easier to experiment with queries during development, but the indexes require space in a database and require time to generate and maintain.

To manage the indexing behavior of your records in the production environment, use CloudKit Dashboard. When migrating your schema from the development environment to the production environment, enable indexing only for the fields that your app uses in queries, and disable it for all other fields.

### [Customizing Records](/documentation/cloudkit/ckrecord#Customizing-Records)

Use this class as-is to manage data coming from or going to the server, and don’t subclass it.

### [Storing Records Locally](/documentation/cloudkit/ckrecord#Storing-Records-Locally)

If you store records in a local database, use the [`encodeSystemFields(with:)`](/documentation/cloudkit/ckrecord/encodesystemfields\(with:\)) method to encode and store the record’s metadata. The metadata contains the record ID and the change tag, which you need later to sync records in a local database with those in CloudKit.

## [Topics](/documentation/cloudkit/ckrecord#topics)

### [Creating a Record](/documentation/cloudkit/ckrecord#Creating-a-Record)

[`convenience init(recordType: CKRecord.RecordType, recordID: CKRecord.ID)`](/documentation/cloudkit/ckrecord/init\(recordtype:recordid:\))

Creates a record using an ID that you provide.

[`typealias RecordType`](/documentation/cloudkit/ckrecord/recordtype-swift.typealias)

The data type that CloudKit requires for record types.

[`typealias FieldKey`](/documentation/cloudkit/ckrecord/fieldkey)

The data type that CloudKit requires for record field names.

[`convenience init(recordType: CKRecord.RecordType, zoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckrecord/init\(recordtype:zoneid:\))

Creates a record in the specified zone.

Deprecated

### [Accessing the Record’s Fields](/documentation/cloudkit/ckrecord#Accessing-the-Records-Fields)

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

[`typealias CKRecordValue`](/documentation/cloudkit/ckrecordvalue-swift.typealias)

A data type for objects that CloudKit stores on the server.

### [Accessing the Record’s Metadata](/documentation/cloudkit/ckrecord#Accessing-the-Records-Metadata)

[`var recordID: CKRecord.ID`](/documentation/cloudkit/ckrecord/recordid)

The unique ID of the record.

[`var recordType: CKRecord.RecordType`](/documentation/cloudkit/ckrecord/recordtype-6v7au)

The value that your app defines to identify the type of record.

[`enum SystemType`](/documentation/cloudkit/ckrecord/systemtype)

Possible values for record types of system records.

[`var creationDate: Date?`](/documentation/cloudkit/ckrecord/creationdate)

The time when CloudKit first saves the record to the server.

[`var creatorUserRecordID: CKRecord.ID?`](/documentation/cloudkit/ckrecord/creatoruserrecordid)

The ID of the user who creates the record.

[`var modificationDate: Date?`](/documentation/cloudkit/ckrecord/modificationdate)

The most recent time that CloudKit saved the record to the server.

[`var lastModifiedUserRecordID: CKRecord.ID?`](/documentation/cloudkit/ckrecord/lastmodifieduserrecordid)

The ID of the user who most recently modified the record.

[`var recordChangeTag: String?`](/documentation/cloudkit/ckrecord/recordchangetag)

The server change token for the record.

[`class ID`](/documentation/cloudkit/ckrecord/id)

An object that uniquely identifies a record in a database.

### [Encrypting the Record’s Values](/documentation/cloudkit/ckrecord#Encrypting-the-Records-Values)

[`var encryptedValues: any CKRecordKeyValueSetting & Sendable`](/documentation/cloudkit/ckrecord/encryptedvalues)

An object that manages the record’s encrypted key-value pairs.

### [Getting Data for Full-Text Searches](/documentation/cloudkit/ckrecord#Getting-Data-for-Full-Text-Searches)

[`func allTokens() -> [String]`](/documentation/cloudkit/ckrecord/alltokens\(\))

Returns an array of strings to use for full-text searches of the field’s string-based values.

### [Encoding the Record’s Metadata](/documentation/cloudkit/ckrecord#Encoding-the-Records-Metadata)

[`func encodeSystemFields(with: NSCoder)`](/documentation/cloudkit/ckrecord/encodesystemfields\(with:\))

Encodes the record’s system fields using the specified archiver.

### [Sharing Records](/documentation/cloudkit/ckrecord#Sharing-Records)

[`var parent: CKRecord.Reference?`](/documentation/cloudkit/ckrecord/parent)

A reference to the record’s parent record.

[`var share: CKRecord.Reference?`](/documentation/cloudkit/ckrecord/share)

A reference to the share object that determines the share status of the record.

[`class Reference`](/documentation/cloudkit/ckrecord/reference)

A relationship between two records in a record zone.

[`func setParent(CKRecord?)`](/documentation/cloudkit/ckrecord/setparent\(_:\)-23du1)

Creates and sets a reference object for a parent from its record.

[`func setParent(CKRecord.ID?)`](/documentation/cloudkit/ckrecord/setparent\(_:\)-7egcx)

Creates and sets a reference object for a parent from the parent’s record ID.

[`enum SystemFieldKey`](/documentation/cloudkit/ckrecord/systemfieldkey)

Possible values for types of system field keys on records.

## [Relationships](/documentation/cloudkit/ckrecord#relationships)

### [Inherits From](/documentation/cloudkit/ckrecord#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Inherited By](/documentation/cloudkit/ckrecord#inherited-by)

-   [`CKShare`](/documentation/cloudkit/ckshare)

### [Conforms To](/documentation/cloudkit/ckrecord#conforms-to)

-   [`CKRecordKeyValueSetting`](/documentation/cloudkit/ckrecordkeyvaluesetting)
-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`Copyable`](/documentation/Swift/Copyable)
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
-   [`Sequence`](/documentation/Swift/Sequence)

## [See Also](/documentation/cloudkit/ckrecord#see-also)

### [Schemas](/documentation/cloudkit/ckrecord#Schemas)

[

Designing and Creating a CloudKit Database](/documentation/cloudkit/designing-and-creating-a-cloudkit-database)

Create a schema to store your app’s objects as records in iCloud using CloudKit.

[

API Reference

Managing iCloud Containers with CloudKit Database App](/documentation/cloudkit/managing-icloud-containers-with-cloudkit-database-app)

Inspect and modify the schema and data for your app’s iCloud container.

[`class CKRecordZone`](/documentation/cloudkit/ckrecordzone)

A database partition that contains related records.

[`class Reference`](/documentation/cloudkit/ckrecord/reference)

A relationship between two records in a record zone.

[`class CKAsset`](/documentation/cloudkit/ckasset)

An external file that belongs to a record.

[

Integrating a Text-Based Schema into Your Workflow](/documentation/cloudkit/integrating-a-text-based-schema-into-your-workflow)

Define and update your schema with the CloudKit Schema Language.

Current page is CKRecord