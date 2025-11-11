# recordID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   recordID

Instance Property

# recordID

The unique ID of the record.

```
@NSCopying
var recordID: CKRecord.ID { get }
```

## [Discussion](/documentation/cloudkit/ckrecord/recordid#Discussion)

The system sets the ID of a new record at initialization time. If you use the [`init(recordType:recordID:)`](/documentation/cloudkit/ckrecord/init\(recordtype:recordid:\)) method to initialize the record, the ID derives from the [`CKRecord.ID`](/documentation/cloudkit/ckrecord/id) object you provide. In all other cases, the record generates a UUID and bases its ID on that value. The ID of a record never changes during its lifetime.

When you save a new record object to the server, the server validates the uniqueness of the record, but returns an error only if the save policy calls for it. Specifically, it returns an error when the save policy is [`CKModifyRecordsOperation.RecordSavePolicy.ifServerRecordUnchanged`](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy/ifserverrecordunchanged), which is the default. For all other save policies, the server overwrites the contents of the existing record.

## [See Also](/documentation/cloudkit/ckrecord/recordid#see-also)

### [Accessing the Record’s Metadata](/documentation/cloudkit/ckrecord/recordid#Accessing-the-Records-Metadata)

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

Current page is recordID

---
# recordType | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   recordType

Instance Property

# recordType

The value that your app defines to identify the type of record.

```
var recordType: CKRecord.RecordType { get }
```

## [Discussion](/documentation/cloudkit/ckrecord/recordtype-6v7au#Discussion)

Use this value to differentiate between different record types in your app. The value is primarily for your benefit, so choose record types that represent the data in the corresponding records.

CloudKit provides two system-defined record types:

Record Type

Description

[`CKRecordTypeUserRecord`](/documentation/cloudkit/ckrecordtypeuserrecord-49k30)

Identifies records that represent users.

[`CKRecordTypeShare`](/documentation/cloudkit/ckrecordtypeshare-8b6yt)

Identifies records that the user shares.

## [See Also](/documentation/cloudkit/ckrecord/recordtype-6v7au#see-also)

### [Accessing the Record’s Metadata](/documentation/cloudkit/ckrecord/recordtype-6v7au#Accessing-the-Records-Metadata)

[`var recordID: CKRecord.ID`](/documentation/cloudkit/ckrecord/recordid)

The unique ID of the record.

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

Current page is recordType

---
# CKRecord.SystemType | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   CKRecord.SystemType

Enumeration

# CKRecord.SystemType

Possible values for record types of system records.

```
enum SystemType
```

## [Topics](/documentation/cloudkit/ckrecord/systemtype#topics)

### [Types of System Records](/documentation/cloudkit/ckrecord/systemtype#Types-of-System-Records)

[`static let share: CKRecord.RecordType`](/documentation/cloudkit/ckrecord/systemtype/share)

A string that represents the record type for CloudKit share records.

[`static let userRecord: CKRecord.RecordType`](/documentation/cloudkit/ckrecord/systemtype/userrecord)

A string that represents the record type for CloudKit user records.

## [See Also](/documentation/cloudkit/ckrecord/systemtype#see-also)

### [Accessing the Record’s Metadata](/documentation/cloudkit/ckrecord/systemtype#Accessing-the-Records-Metadata)

[`var recordID: CKRecord.ID`](/documentation/cloudkit/ckrecord/recordid)

The unique ID of the record.

[`var recordType: CKRecord.RecordType`](/documentation/cloudkit/ckrecord/recordtype-6v7au)

The value that your app defines to identify the type of record.

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

Current page is CKRecord.SystemType

## Types of System Records

# share | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.SystemType](/documentation/cloudkit/ckrecord/systemtype)
-   share

Type Property

# share

A string that represents the record type for CloudKit share records.

```
static let share: CKRecord.RecordType
```

## [See Also](/documentation/cloudkit/ckrecord/systemtype/share#see-also)

### [Types of System Records](/documentation/cloudkit/ckrecord/systemtype/share#Types-of-System-Records)

[`static let userRecord: CKRecord.RecordType`](/documentation/cloudkit/ckrecord/systemtype/userrecord)

A string that represents the record type for CloudKit user records.

Current page is share

# userRecord | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.SystemType](/documentation/cloudkit/ckrecord/systemtype)
-   userRecord

Type Property

# userRecord

A string that represents the record type for CloudKit user records.

```
static let userRecord: CKRecord.RecordType
```

## [See Also](/documentation/cloudkit/ckrecord/systemtype/userrecord#see-also)

### [Types of System Records](/documentation/cloudkit/ckrecord/systemtype/userrecord#Types-of-System-Records)

[`static let share: CKRecord.RecordType`](/documentation/cloudkit/ckrecord/systemtype/share)

A string that represents the record type for CloudKit share records.

Current page is userRecord

---
# creationDate | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   creationDate

Instance Property

# creationDate

The time when CloudKit first saves the record to the server.

```
var creationDate: Date? { get }
```

## [Discussion](/documentation/cloudkit/ckrecord/creationdate#Discussion)

The creation date reflects the time when CloudKit creates a record on the server with the current record’s ID. For new instances of this class, the value of this property is initially `nil`. When you save the record to the server, the value updates with the creation date for the record.

## [See Also](/documentation/cloudkit/ckrecord/creationdate#see-also)

### [Accessing the Record’s Metadata](/documentation/cloudkit/ckrecord/creationdate#Accessing-the-Records-Metadata)

[`var recordID: CKRecord.ID`](/documentation/cloudkit/ckrecord/recordid)

The unique ID of the record.

[`var recordType: CKRecord.RecordType`](/documentation/cloudkit/ckrecord/recordtype-6v7au)

The value that your app defines to identify the type of record.

[`enum SystemType`](/documentation/cloudkit/ckrecord/systemtype)

Possible values for record types of system records.

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

Current page is creationDate

---
# creatorUserRecordID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   creatorUserRecordID

Instance Property

# creatorUserRecordID

The ID of the user who creates the record.

```
@NSCopying
var creatorUserRecordID: CKRecord.ID? { get }
```

## [Discussion](/documentation/cloudkit/ckrecord/creatoruserrecordid#Discussion)

Use this property’s value to retrieve the user record for the user who creates this record. Every user of the app has a unique user record that is empty by default. Apps can add data to the user record on behalf of the user, but don’t store sensitive data in it.

## [See Also](/documentation/cloudkit/ckrecord/creatoruserrecordid#see-also)

### [Accessing the Record’s Metadata](/documentation/cloudkit/ckrecord/creatoruserrecordid#Accessing-the-Records-Metadata)

[`var recordID: CKRecord.ID`](/documentation/cloudkit/ckrecord/recordid)

The unique ID of the record.

[`var recordType: CKRecord.RecordType`](/documentation/cloudkit/ckrecord/recordtype-6v7au)

The value that your app defines to identify the type of record.

[`enum SystemType`](/documentation/cloudkit/ckrecord/systemtype)

Possible values for record types of system records.

[`var creationDate: Date?`](/documentation/cloudkit/ckrecord/creationdate)

The time when CloudKit first saves the record to the server.

[`var modificationDate: Date?`](/documentation/cloudkit/ckrecord/modificationdate)

The most recent time that CloudKit saved the record to the server.

[`var lastModifiedUserRecordID: CKRecord.ID?`](/documentation/cloudkit/ckrecord/lastmodifieduserrecordid)

The ID of the user who most recently modified the record.

[`var recordChangeTag: String?`](/documentation/cloudkit/ckrecord/recordchangetag)

The server change token for the record.

[`class ID`](/documentation/cloudkit/ckrecord/id)

An object that uniquely identifies a record in a database.

Current page is creatorUserRecordID

---
# modificationDate | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   modificationDate

Instance Property

# modificationDate

The most recent time that CloudKit saved the record to the server.

```
var modificationDate: Date? { get }
```

## [Discussion](/documentation/cloudkit/ckrecord/modificationdate#Discussion)

The modification date reflects the most recent time that CloudKit saved a record with the current record’s ID to the server. For new instances of this class, the value of this property is initially `nil`. When you save the record to the server, the value updates with the modification date for the record.

## [See Also](/documentation/cloudkit/ckrecord/modificationdate#see-also)

### [Accessing the Record’s Metadata](/documentation/cloudkit/ckrecord/modificationdate#Accessing-the-Records-Metadata)

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

[`var lastModifiedUserRecordID: CKRecord.ID?`](/documentation/cloudkit/ckrecord/lastmodifieduserrecordid)

The ID of the user who most recently modified the record.

[`var recordChangeTag: String?`](/documentation/cloudkit/ckrecord/recordchangetag)

The server change token for the record.

[`class ID`](/documentation/cloudkit/ckrecord/id)

An object that uniquely identifies a record in a database.

Current page is modificationDate

---
# lastModifiedUserRecordID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   lastModifiedUserRecordID

Instance Property

# lastModifiedUserRecordID

The ID of the user who most recently modified the record.

```
@NSCopying
var lastModifiedUserRecordID: CKRecord.ID? { get }
```

## [Discussion](/documentation/cloudkit/ckrecord/lastmodifieduserrecordid#Discussion)

Use this property’s value to retrieve the user record of the user who most recently modified this record. Every user of the app has a unique user record that is empty by default. Apps can add data to the user record on behalf of the user, but don’t store sensitive data in it.

## [See Also](/documentation/cloudkit/ckrecord/lastmodifieduserrecordid#see-also)

### [Accessing the Record’s Metadata](/documentation/cloudkit/ckrecord/lastmodifieduserrecordid#Accessing-the-Records-Metadata)

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

[`var recordChangeTag: String?`](/documentation/cloudkit/ckrecord/recordchangetag)

The server change token for the record.

[`class ID`](/documentation/cloudkit/ckrecord/id)

An object that uniquely identifies a record in a database.

Current page is lastModifiedUserRecordID

---
# recordChangeTag | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   recordChangeTag

Instance Property

# recordChangeTag

The server change token for the record.

```
var recordChangeTag: String? { get }
```

## [Discussion](/documentation/cloudkit/ckrecord/recordchangetag#Discussion)

When you fetch a record from the server, you get the current version of that record as it exists on the server. However, at any time after you fetch a record, other users might save a newer version of it to the server. Every time CloudKit saves a record, the server updates the record’s change token to a new value. When you save your copy of the record, the server compares your record’s token with the token on the server. If the two tokens match, the server interprets that you modified the latest version of the record and that it can apply your changes immediately. If the two tokens don’t match, the server checks your app’s save policy to determine how to proceed.

In your own code, you can use change tokens to distinguish between two different versions of the same record.

## [See Also](/documentation/cloudkit/ckrecord/recordchangetag#see-also)

### [Accessing the Record’s Metadata](/documentation/cloudkit/ckrecord/recordchangetag#Accessing-the-Records-Metadata)

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

[`class ID`](/documentation/cloudkit/ckrecord/id)

An object that uniquely identifies a record in a database.

Current page is recordChangeTag

---
# CKRecord.ID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   CKRecord.ID

Class

# CKRecord.ID

An object that uniquely identifies a record in a database.

```
class ID
```

## [Overview](/documentation/cloudkit/ckrecord/id#overview)

A record ID object consists of a name string and a zone ID. The name string is an ASCII string that doesn’t exceed 255 characters in length. For automatically created records, the ID name string derives from a UUID and is, therefore, unique. When creating your own record ID objects, you can use names that have more meaning to your app or to the user, as long as each name is unique within the specified zone. For example, you might use a document name for the name string.

Record IDs must be unique within the specified database, but you can reuse record IDs in different databases. Each container has a public and a private database, and the private database is different for each unique user. This configuration provides for the reusing of record IDs in each user’s private database, but ensures that only one record uses a specific record ID in the public database.

CloudKit generally creates record IDs when it first saves a new record, but you might manually instantiate instances of `CKRecordID` in specific situations. For example, you must create an instance when saving a record in a zone other than the default zone. You also instantiate instances of `CKRecordID` when retrieving specific records from a database.

Don’t subclass `CKRecordID`.

### [Interacting with Record IDs](/documentation/cloudkit/ckrecord/id#Interacting-with-Record-IDs)

After you create a `CKRecordID` object, interactions with that object typically involve creating a new record or retrieving an existing record from a database.

You might also use record IDs when you can’t use a [`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference) object to refer to a record. References are only valid within a single zone of a database. To refer to objects outside of the current zone or database, save the strings in the record’s `CKRecordID` and [`CKRecordZone.ID`](/documentation/cloudkit/ckrecordzone/id) objects. When you want to retrieve the record later, use those strings to recreate the record and zone ID objects so that you can fetch the record.

#### [Creating Record IDs for New Records](/documentation/cloudkit/ckrecord/id#Creating-Record-IDs-for-New-Records)

To assign a custom record ID to a new record, you must create the `CKRecordID` object first. You need to know the intended name and zone information for that record, which might also require creating a [`CKRecordZone.ID`](/documentation/cloudkit/ckrecordzone/id) object. After creating the record ID object, initialize your new record using its [`initWithRecordType:recordID:`](/documentation/cloudkit/ckrecord/initwithrecordtype:recordid:) method.

#### [Using Record IDs to Fetch Records](/documentation/cloudkit/ckrecord/id#Using-Record-IDs-to-Fetch-Records)

Use a record ID to fetch the corresponding [`CKRecord`](/documentation/cloudkit/ckrecord) object from a database quickly. You perform the fetch operation using a [`CKFetchRecordsOperation`](/documentation/cloudkit/ckfetchrecordsoperation) object or the [`fetch(withRecordID:completionHandler:)`](/documentation/cloudkit/ckdatabase/fetch\(withrecordid:completionhandler:\)) method of the [`CKDatabase`](/documentation/cloudkit/ckdatabase) class. In both cases, CloudKit returns the record asynchronously using the handler you provide.

## [Topics](/documentation/cloudkit/ckrecord/id#topics)

### [Creating a Record ID](/documentation/cloudkit/ckrecord/id#Creating-a-Record-ID)

[`convenience init(recordName: String)`](/documentation/cloudkit/ckrecord/id/init\(recordname:\))

Creates a new record ID with the specified name in the default zone.

[`convenience init(recordName: String, zoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckrecord/id/init\(recordname:zoneid:\))

Creates a new record ID with the specified name and zone information.

[`let CKRecordNameZoneWideShare: String`](/documentation/cloudkit/ckrecordnamezonewideshare)

The name of a share record that manages a shared record zone.

### [Getting the Record ID’s Name](/documentation/cloudkit/ckrecord/id#Getting-the-Record-IDs-Name)

[`var recordName: String`](/documentation/cloudkit/ckrecord/id/recordname)

The unique name of the record.

### [Getting the Record ID’s Zone](/documentation/cloudkit/ckrecord/id#Getting-the-Record-IDs-Zone)

[`var zoneID: CKRecordZone.ID`](/documentation/cloudkit/ckrecord/id/zoneid)

The ID of the zone that contains the record.

## [Relationships](/documentation/cloudkit/ckrecord/id#relationships)

### [Inherits From](/documentation/cloudkit/ckrecord/id#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckrecord/id#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
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

## [See Also](/documentation/cloudkit/ckrecord/id#see-also)

### [Accessing the Record’s Metadata](/documentation/cloudkit/ckrecord/id#Accessing-the-Records-Metadata)

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

Current page is CKRecord.ID

## Creating a Record ID

# init(recordName:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.ID](/documentation/cloudkit/ckrecord/id)
-   init(recordName:)

Initializer

# init(recordName:)

Creates a new record ID with the specified name in the default zone.

```
convenience init(recordName: String)
```

## [Parameters](/documentation/cloudkit/ckrecord/id/init\(recordname:\)#parameters)

`recordName`

The name that identifies the record. The string must contain only ASCII characters, must not exceed 255 characters, and must not start with an underscore. If you specify `nil` or an empty string for this parameter, the method throws an exception.

## [Return Value](/documentation/cloudkit/ckrecord/id/init\(recordname:\)#return-value)

An initialized record ID object, or `nil` if CloudKit can’t create the object.

## [Discussion](/documentation/cloudkit/ckrecord/id/init\(recordname:\)#Discussion)

Use this method when you’re creating or searching for records in the default zone.

## [See Also](/documentation/cloudkit/ckrecord/id/init\(recordname:\)#see-also)

### [Creating a Record ID](/documentation/cloudkit/ckrecord/id/init\(recordname:\)#Creating-a-Record-ID)

[`convenience init(recordName: String, zoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckrecord/id/init\(recordname:zoneid:\))

Creates a new record ID with the specified name and zone information.

[`let CKRecordNameZoneWideShare: String`](/documentation/cloudkit/ckrecordnamezonewideshare)

The name of a share record that manages a shared record zone.

Current page is init(recordName:)

# init(recordName:zoneID:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.ID](/documentation/cloudkit/ckrecord/id)
-   init(recordName:zoneID:)

Initializer

# init(recordName:zoneID:)

Creates a new record ID with the specified name and zone information.

```
convenience init(
    recordName: String = UUID().uuidString,
    zoneID: CKRecordZone.ID = CKRecordZone.ID.default
)
```

## [Parameters](/documentation/cloudkit/ckrecord/id/init\(recordname:zoneid:\)#parameters)

`recordName`

The name that identifies the record. The string must contain only ASCII characters, must not exceed 255 characters, and must not start with an underscore. If you specify `nil` or an empty string for this parameter, the method throws an exception.

`zoneID`

The ID of the record zone where you want to store the record. This parameter must not be `nil`.

## [Discussion](/documentation/cloudkit/ckrecord/id/init\(recordname:zoneid:\)#Discussion)

Use this method when you create or search for records in a zone other than the default zone. The value in the `zoneID` parameter must represent a zone that already exists in the database. If the record zone doesn’t exist, save the corresponding [`CKRecordZone`](/documentation/cloudkit/ckrecordzone) object to the database before attempting to save any [`CKRecord`](/documentation/cloudkit/ckrecord) objects in that zone.

## [See Also](/documentation/cloudkit/ckrecord/id/init\(recordname:zoneid:\)#see-also)

### [Creating a Record ID](/documentation/cloudkit/ckrecord/id/init\(recordname:zoneid:\)#Creating-a-Record-ID)

[`convenience init(recordName: String)`](/documentation/cloudkit/ckrecord/id/init\(recordname:\))

Creates a new record ID with the specified name in the default zone.

[`let CKRecordNameZoneWideShare: String`](/documentation/cloudkit/ckrecordnamezonewideshare)

The name of a share record that manages a shared record zone.

Current page is init(recordName:zoneID:)

# CKRecordNameZoneWideShare | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKRecordNameZoneWideShare

Global Variable

# CKRecordNameZoneWideShare

The name of a share record that manages a shared record zone.

```
let CKRecordNameZoneWideShare: String
```

## [Discussion](/documentation/cloudkit/ckrecordnamezonewideshare#Discussion)

When you create an instance of [`CKShare`](/documentation/cloudkit/ckshare) for sharing a record zone, CloudKit automatically assigns this constant as the [`recordName`](/documentation/cloudkit/ckrecord/id/recordname) element of the share record’s [`recordID`](/documentation/cloudkit/ckrecord/recordid). After you save the share record to iCloud, you can fetch it by reconstructing the record ID using this constant, as the following example shows:

```
func fetchShare(forZone zone: CKRecordZone,
                completion: @escaping (Result<CKShare, Error>) -> Void) {
    let database = CKContainer.default().privateCloudDatabase
        
    // Use the 'CKRecordNameZoneWideShare' constant to create the record ID.
    let recordID = CKRecord.ID(recordName: CKRecordNameZoneWideShare,
                               zoneID: zone.zoneID)
        
    // Fetch the share record from the specified record zone.
    database.fetch(withRecordID: recordID) { share, error in
        if let error = error {
            // If the fetch fails, inform the caller.
            completion(.failure(error))
        } else if let share = share as? CKShare {
            // Otherwise, pass the fetched share record to the
            // completion handler.
            completion(.success(share))
        } else {
            fatalError("Unable to fetch record with ID: \(recordID)")
        }
    }
}
```

## [See Also](/documentation/cloudkit/ckrecordnamezonewideshare#see-also)

### [Creating a Record ID](/documentation/cloudkit/ckrecordnamezonewideshare#Creating-a-Record-ID)

[`convenience init(recordName: String)`](/documentation/cloudkit/ckrecord/id/init\(recordname:\))

Creates a new record ID with the specified name in the default zone.

[`convenience init(recordName: String, zoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckrecord/id/init\(recordname:zoneid:\))

Creates a new record ID with the specified name and zone information.

Current page is CKRecordNameZoneWideShare

## Getting the Record ID’s Name
# recordName | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.ID](/documentation/cloudkit/ckrecord/id)
-   recordName

Instance Property

# recordName

The unique name of the record.

```
var recordName: String { get }
```

## [Discussion](/documentation/cloudkit/ckrecord/id/recordname#Discussion)

For share records that manage a shared record zone, this property’s value is always [`CKRecordNameZoneWideShare`](/documentation/cloudkit/ckrecordnamezonewideshare).

Current page is recordName

## Getting the Record ID’s Zone
# zoneID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.ID](/documentation/cloudkit/ckrecord/id)
-   zoneID

Instance Property

# zoneID

The ID of the zone that contains the record.

```
@NSCopying
var zoneID: CKRecordZone.ID { get }
```

Current page is zoneID

----
  
# Encrypting the Record’s Values

# encryptedValues | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   encryptedValues

Instance Property

# encryptedValues

An object that manages the record’s encrypted key-value pairs.

```
@NSCopying
var encryptedValues: any CKRecordKeyValueSetting & Sendable { get }
```

## [Mentioned in](/documentation/cloudkit/ckrecord/encryptedvalues#mentions)

[

Encrypting User Data](/documentation/cloudkit/encrypting-user-data)

## [Discussion](/documentation/cloudkit/ckrecord/encryptedvalues#Discussion)

Use the object this property returns to read and write encrypted key-value pairs that you store on the record. You can encrypt values of any data type that CloudKit supports, except [`CKAsset`](/documentation/cloudkit/ckasset), which is encrypted by default, and [`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference), which isn’t encrypted so it remains available for server-side use. Only encrypt new fields. CloudKit doesn’t allow encryption on fields that already exist in your app’s schema, or on records that you store in the public database.

CloudKit encrypts the fields’ values on-device before saving them to iCloud, and decrypts the values only after fetching them from the server. When you enable Advanced Data Protection, the encryption keys are available exclusively to the record’s owner and, if the user shares the record, that share’s participants.

The following example shows how to use `encryptedValues` to encrypt and decrypt a string value:

```
let record = CKRecord(recordType: "Property")


// Encrypt the name of the property's owner.
record.encryptedValues["ownerName"] = "Maria Ruiz"


// Decrypt the name of the property's owner, using the
// appropriate data type, and assign it to a local variable.
var clientName = record.encryptedValues["ownerName"] as? NSString
```

Current page is encryptedValues

#  Getting Data for Full-Text Searches

# allTokens() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   allTokens()

Instance Method

# allTokens()

Returns an array of strings to use for full-text searches of the field’s string-based values.

```
func allTokens() -> [String]
```

## [Return Value](/documentation/cloudkit/ckrecord/alltokens\(\)#return-value)

An array of strings that contains data from the record’s string-based fields.

## [Discussion](/documentation/cloudkit/ckrecord/alltokens\(\)#Discussion)

When performing your own full-text searches, you can use this method to get a list of strings for your search. The method acts only on keys with string values. It breaks each value string apart at whitespace boundaries, creates new strings for each word, adds the new strings to an array, and returns the array. This tokenized version of the record’s string values makes it easier to do string-based comparisons of individual words.

Current page is allTokens()

# Encoding the Record’s Metadata

# encodeSystemFields(with:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   encodeSystemFields(with:)

Instance Method

# encodeSystemFields(with:)

Encodes the record’s system fields using the specified archiver.

```
func encodeSystemFields(with coder: NSCoder)
```

## [Parameters](/documentation/cloudkit/ckrecord/encodesystemfields\(with:\)#parameters)

`coder`

An archiver object.

## [Discussion](/documentation/cloudkit/ckrecord/encodesystemfields\(with:\)#Discussion)

Use this method to encode the record’s metadata that CloudKit provides. Every record has keys that the system defines that correspond to record metadata, such as the record ID, record type, creation date, and so on. This method encodes those keys in the specified archiver. This method doesn’t include any keys you add to the record. It also doesn’t encode the keys that the [`changedKeys`](/documentation/cloudkit/ckrecord/changedkeys) method returns.

You might use this method when you want to store only the system metadata because you store the actual record data elsewhere.

Current page is encodeSystemFields(with:)

# Sharing Records
# parent | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   parent

Instance Property

# parent

A reference to the record’s parent record.

```
@NSCopying
var parent: CKRecord.Reference? { get set }
```

## [Discussion](/documentation/cloudkit/ckrecord/parent#Discussion)

Use parent references to inform CloudKit about the hierarchy of your records. CloudKit shares the hierarchy when a [`CKShare`](/documentation/cloudkit/ckshare) includes a referenced record. Add relationships between records as you create them, even if you don’t plan to share them. This allows you to manage the sharing of a hierarchy by only modifying the root record’s [`share`](/documentation/cloudkit/ckrecord/share) reference.

To indicate that a record belongs to its parent, set this property to a reference that points to the parent record. The reference must use the [`CKRecord.ReferenceAction.none`](/documentation/cloudkit/ckrecord/referenceaction/none) action or CloudKit throws an exception. The parent record must exist on the server when you save the child, or must be part of the same save operation. Otherwise, the operation fails.

## [See Also](/documentation/cloudkit/ckrecord/parent#see-also)

### [Sharing Records](/documentation/cloudkit/ckrecord/parent#Sharing-Records)

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

Current page is parent

---
# share | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   share

Instance Property

# share

A reference to the share object that determines the share status of the record.

```
@NSCopying
var share: CKRecord.Reference? { get }
```

## [Discussion](/documentation/cloudkit/ckrecord/share#Discussion)

CloudKit clears this property’s value when it deletes the corresponding [`CKShare`](/documentation/cloudkit/ckshare) object on the server. Send this record in the same batch operation as the share object you’re deleting, and this property updates accordingly.

CloudKit only supports sharing in zones with the `CKRecordZoneCapabilitySharing` capability. The default zone doesn’t support sharing.

If any records have a parent reference to this record, CloudKit implicitly shares them along with this record.

## [See Also](/documentation/cloudkit/ckrecord/share#see-also)

### [Sharing Records](/documentation/cloudkit/ckrecord/share#Sharing-Records)

[`var parent: CKRecord.Reference?`](/documentation/cloudkit/ckrecord/parent)

A reference to the record’s parent record.

[`class Reference`](/documentation/cloudkit/ckrecord/reference)

A relationship between two records in a record zone.

[`func setParent(CKRecord?)`](/documentation/cloudkit/ckrecord/setparent\(_:\)-23du1)

Creates and sets a reference object for a parent from its record.

[`func setParent(CKRecord.ID?)`](/documentation/cloudkit/ckrecord/setparent\(_:\)-7egcx)

Creates and sets a reference object for a parent from the parent’s record ID.

[`enum SystemFieldKey`](/documentation/cloudkit/ckrecord/systemfieldkey)

Possible values for types of system field keys on records.

Current page is share

---
# CKRecord.Reference | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   CKRecord.Reference

Class

# CKRecord.Reference

A relationship between two records in a record zone.

```
class Reference
```

## [Mentioned in](/documentation/cloudkit/ckrecord/reference#mentions)

[

Designing and Creating a CloudKit Database](/documentation/cloudkit/designing-and-creating-a-cloudkit-database)

[

Encrypting User Data](/documentation/cloudkit/encrypting-user-data)

## [Overview](/documentation/cloudkit/ckrecord/reference#overview)

A `CKReference` object creates a many-to-one relationship between records in your database. Each reference object stores information about the one record that is the target of the reference. You then save the reference object in the fields of one or more records to create a link from those records to the target. Both records must be in the same zone of the same database.

References create a stronger relationship between records than just saving the ID of a record as a string. Specifically, you can use references to create an ownership model between two records. When the reference object’s action is [`CKRecord.ReferenceAction.deleteSelf`](/documentation/cloudkit/ckrecord/referenceaction/deleteself), the target of the reference—that is, the record in the reference’s [`recordID`](/documentation/cloudkit/ckrecord/reference/recordid) property—becomes the owner of the source record. Deleting the target (owner) record deletes all its source records. The deletion of any owned records can trigger further deletions if those records are the owners of other records. If a record contains two or more `CKReference` objects with an action of [`CKRecord.ReferenceAction.deleteSelf`](/documentation/cloudkit/ckrecord/referenceaction/deleteself), CloudKit deletes the record when it deletes any of the objects it references.

To save multiple records that contain references between them, save the target records first or save all the records in one batch operation using [`CKModifyRecordsOperation`](/documentation/cloudkit/ckmodifyrecordsoperation).

### [Interacting with Reference Objects](/documentation/cloudkit/ckrecord/reference#Interacting-with-Reference-Objects)

You use reference objects to create strong links between two records and to search for related fields. When you create new records, you create reference objects and assign them to fields of your records. The only other time you create reference objects is when you build a search predicate to search for related records.

#### [Linking to Another Record](/documentation/cloudkit/ckrecord/reference#Linking-to-Another-Record)

To link records together and create a strong relationship between them, create a new `CKReference` object, initialize it with the owner record, and assign that reference object to a field of the owned record. When you design the relationships among your own records, make the owner the more important of two related records. The owner record rarely depends on any records that point to it. The owner record is also the one that you typically fetch first from the database.

The figure below shows an example of a relationship between a to-do list record and a set of item records that represent individual items to complete. The to-do list is the primary record, or owner, in the relationship because it represents the entire to-do list, including all items on the list. As a result, each item record has a field that contains a `CKReference` object that points to the owning to-do list record.

![A figure that shows the relationship between a parent record and its children.](https://docs-assets.developer.apple.com/published/251fe779254091b04fe03fb4c610471d/media-1965777%402x.png)

The following code sample shows how to create the reference object for each item record and configure it to point at the list record:

```
itemRecord["owningList"] = CKReference(record: listRecord, action: .deleteSelf)
```

```
CKReference* ref = [[CKReference alloc] initWithRecord:listRecord action:CKReferenceActionDeleteSelf];
itemRecord[@"owningList"] = ref;
```

An ownership type of organization is useful even if one object doesn’t explicitly own another. Ownership helps establish the relationships between records and how you search for them in the database. Ownership doesn’t require the deletion of the owned records when you delete their owner record. You can prevent such deletions by specifying the [`CKRecord.ReferenceAction.none`](/documentation/cloudkit/ckrecord/referenceaction/none) action when you create a `CKReference` object.

#### [Searching for Related Records](/documentation/cloudkit/ckrecord/reference#Searching-for-Related-Records)

When you want to find records for a single owner object, you create a `CKReference` object and use it to build your search predicate. When you use reference objects in search predicates, the search code looks only at the ID value in the reference object. It matches the ID in records of the specified type with the ID you provide in the `CKReference` object.

The code sample below shows how to use a reference object to construct a query for the records in the figure above. The `listID` variable is a placeholder for the record ID of the list with the items you want to retrieve. The predicate tells the query object to search the `owningList` field of the target records and compare the reference object there with the one in the `recordToMatch` variable. Executing the query operation object returns the matching records asynchronously to the completion block you provide.

```
// Match item records with an owningList field that points to the specified list record.
let listID = listRecord.recordID
let recordToMatch = CKReference(recordID: listID, action: .deleteSelf)
let predicate = NSPredicate(format: "owningList == %@", recordToMatch)
// Create the query object.
let query = CKQuery(recordType: "Item", predicate: predicate)
let queryOp = CKQueryOperation(query: query)
queryOp.queryCompletionBlock = { (cursor, error) in
    // Process the results…
}
// Add the CKQueryOperation to a queue to execute it and process the results asynchronously.
```

```
// Match item records with an owningList field that points to the specified list record.
CKReference* recordToMatch = [[CKReference alloc] initWithRecordID:listID
                                                            action:CKReferenceActionDeleteSelf];
NSPredicate* predicate = [NSPredicate predicateWithFormat:@"owningList == %@", recordToMatch];
// Create the query object.
CKQuery* query = [[CKQuery alloc] initWithRecordType:@"Item" predicate:predicate];
CKQueryOperation *queryOp = [[CKQueryOperation alloc] initWithQuery:query];
[queryOp setQueryCompletionBlock:^(CKQueryCursor *nextCursor, NSError *error) {
    // Process the results…
}];
// Add the CKQueryOperation to a queue to execute it and process the results asynchronously.
```

## [Topics](/documentation/cloudkit/ckrecord/reference#topics)

### [Creating a Reference](/documentation/cloudkit/ckrecord/reference#Creating-a-Reference)

[`init(recordID: CKRecord.ID, action: CKRecord.ReferenceAction)`](/documentation/cloudkit/ckrecord/reference/init\(recordid:action:\))

Creates a reference object that points to the record with the specified ID.

[`convenience init(record: CKRecord, action: CKRecord.ReferenceAction)`](/documentation/cloudkit/ckrecord/reference/init\(record:action:\))

Creates a reference object that points to the specified record object.

[`typealias Action`](/documentation/cloudkit/ckrecord/reference/action-swift.typealias)

A type that represents additional actions that occur when deleting references.

### [Getting the Reference Attributes](/documentation/cloudkit/ckrecord/reference#Getting-the-Reference-Attributes)

[`var action: CKRecord.ReferenceAction`](/documentation/cloudkit/ckrecord/reference/action-swift.property)

The ownership behavior for the records.

[`var recordID: CKRecord.ID`](/documentation/cloudkit/ckrecord/reference/recordid)

The ID of the referenced record.

[`enum ReferenceAction`](/documentation/cloudkit/ckrecord/referenceaction)

Constants that indicate the behavior when deleting a referenced record.

## [Relationships](/documentation/cloudkit/ckrecord/reference#relationships)

### [Inherits From](/documentation/cloudkit/ckrecord/reference#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckrecord/reference#conforms-to)

-   [`CKRecordValueProtocol`](/documentation/cloudkit/ckrecordvalueprotocol)
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

## [See Also](/documentation/cloudkit/ckrecord/reference#see-also)

### [Schemas](/documentation/cloudkit/ckrecord/reference#Schemas)

[

Designing and Creating a CloudKit Database](/documentation/cloudkit/designing-and-creating-a-cloudkit-database)

Create a schema to store your app’s objects as records in iCloud using CloudKit.

[

API Reference

Managing iCloud Containers with CloudKit Database App](/documentation/cloudkit/managing-icloud-containers-with-cloudkit-database-app)

Inspect and modify the schema and data for your app’s iCloud container.

[`class CKRecordZone`](/documentation/cloudkit/ckrecordzone)

A database partition that contains related records.

[`class CKRecord`](/documentation/cloudkit/ckrecord)

A collection of key-value pairs that store your app’s data.

[`class CKAsset`](/documentation/cloudkit/ckasset)

An external file that belongs to a record.

[

Integrating a Text-Based Schema into Your Workflow](/documentation/cloudkit/integrating-a-text-based-schema-into-your-workflow)

Define and update your schema with the CloudKit Schema Language.

Current page is CKRecord.Reference

## Creating a Reference
# init(recordID:action:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.Reference](/documentation/cloudkit/ckrecord/reference)
-   init(recordID:action:)

Initializer

# init(recordID:action:)

Creates a reference object that points to the record with the specified ID.

```
init(
    recordID: CKRecord.ID,
    action: CKRecord.ReferenceAction
)
```

## [Parameters](/documentation/cloudkit/ckrecord/reference/init\(recordid:action:\)#parameters)

`recordID`

The ID of the target record. This method throws an exception if you specify `nil` for this parameter.

`action`

The ownership option use between the target record and any records that incorporate this reference object. If you specify the [`CKRecord.ReferenceAction.deleteSelf`](/documentation/cloudkit/ckrecord/referenceaction/deleteself) option, the record that the `recordID` parameter references becomes the owner of (or acts as the parent of) any objects that use this reference object. For a list of possible values, see [`CKRecord.ReferenceAction`](/documentation/cloudkit/ckrecord/referenceaction).

## [Return Value](/documentation/cloudkit/ckrecord/reference/init\(recordid:action:\)#return-value)

An initialized reference object that points to the specified record, or `nil` if CloudKit can’t initialize the reference.

## [Discussion](/documentation/cloudkit/ckrecord/reference/init\(recordid:action:\)#Discussion)

Use this method when you have only the ID of the record for the target of a link. You might use this method if you save only the ID of the record to a local data cache.

When you create a reference object for use in a search predicate, the predicate ignores the value in the `action` parameter. Search predicates use only the ID of the record during their comparison.

## [See Also](/documentation/cloudkit/ckrecord/reference/init\(recordid:action:\)#see-also)

### [Creating a Reference](/documentation/cloudkit/ckrecord/reference/init\(recordid:action:\)#Creating-a-Reference)

[`convenience init(record: CKRecord, action: CKRecord.ReferenceAction)`](/documentation/cloudkit/ckrecord/reference/init\(record:action:\))

Creates a reference object that points to the specified record object.

[`typealias Action`](/documentation/cloudkit/ckrecord/reference/action-swift.typealias)

A type that represents additional actions that occur when deleting references.

Current page is init(recordID:action:)

# init(record:action:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.Reference](/documentation/cloudkit/ckrecord/reference)
-   init(record:action:)

Initializer

# init(record:action:)

Creates a reference object that points to the specified record object.

```
convenience init(
    record: CKRecord,
    action: CKRecord.ReferenceAction
)
```

## [Parameters](/documentation/cloudkit/ckrecord/reference/init\(record:action:\)#parameters)

`record`

The target record of the reference.

`action`

The ownership options to use for the records. If you specify the [`CKRecord.ReferenceAction.deleteSelf`](/documentation/cloudkit/ckrecord/referenceaction/deleteself) option, the object that the `recordID` parameter references becomes the owner of (or acts as the parent of) any objects that use this reference object. For a list of possible values, see [`CKRecord.ReferenceAction`](/documentation/cloudkit/ckrecord/referenceaction).

## [Return Value](/documentation/cloudkit/ckrecord/reference/init\(record:action:\)#return-value)

An initialized reference object that points to the specified record, or `nil` if CloudKit can’t initialize the reference.

## [Discussion](/documentation/cloudkit/ckrecord/reference/init\(record:action:\)#Discussion)

Use this method to initialize a reference to a local record object. The local record can be one that you create or one that you fetch from the server.

When you create a reference object for use in a search predicate, the predicate ignores the value in the `action` parameter. Search predicates use only the ID of the record during their comparison.

## [See Also](/documentation/cloudkit/ckrecord/reference/init\(record:action:\)#see-also)

### [Creating a Reference](/documentation/cloudkit/ckrecord/reference/init\(record:action:\)#Creating-a-Reference)

[`init(recordID: CKRecord.ID, action: CKRecord.ReferenceAction)`](/documentation/cloudkit/ckrecord/reference/init\(recordid:action:\))

Creates a reference object that points to the record with the specified ID.

[`typealias Action`](/documentation/cloudkit/ckrecord/reference/action-swift.typealias)

A type that represents additional actions that occur when deleting references.

Current page is init(record:action:)

# CKRecord.Reference.Action | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.Reference](/documentation/cloudkit/ckrecord/reference)
-   CKRecord.Reference.Action

Type Alias

# CKRecord.Reference.Action

A type that represents additional actions that occur when deleting references.

```
typealias Action = CKRecord.ReferenceAction
```

## [See Also](/documentation/cloudkit/ckrecord/reference/action-swift.typealias#see-also)

### [Creating a Reference](/documentation/cloudkit/ckrecord/reference/action-swift.typealias#Creating-a-Reference)

[`init(recordID: CKRecord.ID, action: CKRecord.ReferenceAction)`](/documentation/cloudkit/ckrecord/reference/init\(recordid:action:\))

Creates a reference object that points to the record with the specified ID.

[`convenience init(record: CKRecord, action: CKRecord.ReferenceAction)`](/documentation/cloudkit/ckrecord/reference/init\(record:action:\))

Creates a reference object that points to the specified record object.

Current page is CKRecord.Reference.Action

## Getting the Reference Attributes
# action | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.Reference](/documentation/cloudkit/ckrecord/reference)
-   action

Instance Property

# action

The ownership behavior for the records.

```
var action: CKRecord.ReferenceAction { get }
```

## [Discussion](/documentation/cloudkit/ckrecord/reference/action-swift.property#Discussion)

The value in this property determines which action, if any, to take when deleting the target of the reference object — that is, the object that the [`recordID`](/documentation/cloudkit/ckrecord/reference/recordid) property points to. When this property is [`CKRecord.ReferenceAction.deleteSelf`](/documentation/cloudkit/ckrecord/referenceaction/deleteself), deleting the target object deletes any records that contain that reference in one of their fields. When this property is [`CKRecord.ReferenceAction.none`](/documentation/cloudkit/ckrecord/referenceaction/none), deleting the target object doesn’t delete any additional objects.

## [See Also](/documentation/cloudkit/ckrecord/reference/action-swift.property#see-also)

### [Getting the Reference Attributes](/documentation/cloudkit/ckrecord/reference/action-swift.property#Getting-the-Reference-Attributes)

[`var recordID: CKRecord.ID`](/documentation/cloudkit/ckrecord/reference/recordid)

The ID of the referenced record.

[`enum ReferenceAction`](/documentation/cloudkit/ckrecord/referenceaction)

Constants that indicate the behavior when deleting a referenced record.

Current page is action

# recordID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.Reference](/documentation/cloudkit/ckrecord/reference)
-   recordID

Instance Property

# recordID

The ID of the referenced record.

```
@NSCopying
var recordID: CKRecord.ID { get }
```

## [Discussion](/documentation/cloudkit/ckrecord/reference/recordid#Discussion)

Use the ID in this property to fetch the record on the other end of the link.

## [See Also](/documentation/cloudkit/ckrecord/reference/recordid#see-also)

### [Getting the Reference Attributes](/documentation/cloudkit/ckrecord/reference/recordid#Getting-the-Reference-Attributes)

[`var action: CKRecord.ReferenceAction`](/documentation/cloudkit/ckrecord/reference/action-swift.property)

The ownership behavior for the records.

[`enum ReferenceAction`](/documentation/cloudkit/ckrecord/referenceaction)

Constants that indicate the behavior when deleting a referenced record.

Current page is recordID

##  CKRecord.ReferenceAction
# CKRecord.ReferenceAction | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   CKRecord.ReferenceAction

Enumeration

# CKRecord.ReferenceAction

Constants that indicate the behavior when deleting a referenced record.

```
enum ReferenceAction
```

## [Topics](/documentation/cloudkit/ckrecord/referenceaction#topics)

### [Deletion Reference Actions](/documentation/cloudkit/ckrecord/referenceaction#Deletion-Reference-Actions)

[`case none`](/documentation/cloudkit/ckrecord/referenceaction/none)

A reference action that has no cascading behavior.

[`case deleteSelf`](/documentation/cloudkit/ckrecord/referenceaction/deleteself)

A reference action that cascades deletions.

### [Initializers](/documentation/cloudkit/ckrecord/referenceaction#Initializers)

[`init?(rawValue: UInt)`](/documentation/cloudkit/ckrecord/referenceaction/init\(rawvalue:\))

## [Relationships](/documentation/cloudkit/ckrecord/referenceaction#relationships)

### [Conforms To](/documentation/cloudkit/ckrecord/referenceaction#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/ckrecord/referenceaction#see-also)

### [Getting the Reference Attributes](/documentation/cloudkit/ckrecord/referenceaction#Getting-the-Reference-Attributes)

[`var action: CKRecord.ReferenceAction`](/documentation/cloudkit/ckrecord/reference/action-swift.property)

The ownership behavior for the records.

[`var recordID: CKRecord.ID`](/documentation/cloudkit/ckrecord/reference/recordid)

The ID of the referenced record.

Current page is CKRecord.ReferenceAction

# CKRecord.ReferenceAction.none | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.ReferenceAction](/documentation/cloudkit/ckrecord/referenceaction)
-   CKRecord.ReferenceAction.none

Case

# CKRecord.ReferenceAction.none

A reference action that has no cascading behavior.

```
case none
```

## [Discussion](/documentation/cloudkit/ckrecord/referenceaction/none#Discussion)

No action occurs when you delete a record that the current record references. Deleting a parent record doesn’t delete that record’s children. The `CKReference` object still contains the ID of the deleted record and doesn’t update.

## [See Also](/documentation/cloudkit/ckrecord/referenceaction/none#see-also)

### [Deletion Reference Actions](/documentation/cloudkit/ckrecord/referenceaction/none#Deletion-Reference-Actions)

[`case deleteSelf`](/documentation/cloudkit/ckrecord/referenceaction/deleteself)

A reference action that cascades deletions.

Current page is CKRecord.ReferenceAction.none

# CKRecord.ReferenceAction.deleteSelf | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.ReferenceAction](/documentation/cloudkit/ckrecord/referenceaction)
-   CKRecord.ReferenceAction.deleteSelf

Case

# CKRecord.ReferenceAction.deleteSelf

A reference action that cascades deletions.

```
case deleteSelf
```

## [Discussion](/documentation/cloudkit/ckrecord/referenceaction/deleteself#Discussion)

CloudKit deletes any records that contain `CKReference` objects pointing to the current record. The deletion of the additional records can trigger further deletions as the action cascades. The deletions are asynchronous in the default zone and immediate in a custom zone.

## [See Also](/documentation/cloudkit/ckrecord/referenceaction/deleteself#see-also)

### [Deletion Reference Actions](/documentation/cloudkit/ckrecord/referenceaction/deleteself#Deletion-Reference-Actions)

[`case none`](/documentation/cloudkit/ckrecord/referenceaction/none)

A reference action that has no cascading behavior.

Current page is CKRecord.ReferenceAction.deleteSelf

# init(rawValue:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.ReferenceAction](/documentation/cloudkit/ckrecord/referenceaction)
-   init(rawValue:)

Initializer

# init(rawValue:)

```
init?(rawValue: UInt)
```

Current page is init(rawValue:)

---
# setParent(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   setParent(\_:)

Instance Method

# setParent(\_:)

Creates and sets a reference object for a parent from its record.

```
func setParent(_ parentRecord: CKRecord?)
```

## [Parameters](/documentation/cloudkit/ckrecord/setparent\(_:\)-23du1#parameters)

`parentRecord`

A record that you want to set as the parent to this record.

## [Discussion](/documentation/cloudkit/ckrecord/setparent\(_:\)-23du1#Discussion)

This method creates and sets a [`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference) object that points to the record you provide. The resulting `CKReference` has an action of [`CKRecord.ReferenceAction.none`](/documentation/cloudkit/ckrecord/referenceaction/none).

## [See Also](/documentation/cloudkit/ckrecord/setparent\(_:\)-23du1#see-also)

### [Sharing Records](/documentation/cloudkit/ckrecord/setparent\(_:\)-23du1#Sharing-Records)

[`var parent: CKRecord.Reference?`](/documentation/cloudkit/ckrecord/parent)

A reference to the record’s parent record.

[`var share: CKRecord.Reference?`](/documentation/cloudkit/ckrecord/share)

A reference to the share object that determines the share status of the record.

[`class Reference`](/documentation/cloudkit/ckrecord/reference)

A relationship between two records in a record zone.

[`func setParent(CKRecord.ID?)`](/documentation/cloudkit/ckrecord/setparent\(_:\)-7egcx)

Creates and sets a reference object for a parent from the parent’s record ID.

[`enum SystemFieldKey`](/documentation/cloudkit/ckrecord/systemfieldkey)

Possible values for types of system field keys on records.

Current page is setParent(\_:)

---
# setParent(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   setParent(\_:)

Instance Method

# setParent(\_:)

Creates and sets a reference object for a parent from the parent’s record ID.

```
func setParent(_ parentRecordID: CKRecord.ID?)
```

## [Parameters](/documentation/cloudkit/ckrecord/setparent\(_:\)-7egcx#parameters)

`parentRecordID`

The [`CKRecord.ID`](/documentation/cloudkit/ckrecord/id) object for the record that you want to set as this record’s parent.

## [Discussion](/documentation/cloudkit/ckrecord/setparent\(_:\)-7egcx#Discussion)

This method creates and sets a [`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference) object that points to the record you provide. The resulting `CKReference` has an action of [`CKRecord.ReferenceAction.none`](/documentation/cloudkit/ckrecord/referenceaction/none).

## [See Also](/documentation/cloudkit/ckrecord/setparent\(_:\)-7egcx#see-also)

### [Sharing Records](/documentation/cloudkit/ckrecord/setparent\(_:\)-7egcx#Sharing-Records)

[`var parent: CKRecord.Reference?`](/documentation/cloudkit/ckrecord/parent)

A reference to the record’s parent record.

[`var share: CKRecord.Reference?`](/documentation/cloudkit/ckrecord/share)

A reference to the share object that determines the share status of the record.

[`class Reference`](/documentation/cloudkit/ckrecord/reference)

A relationship between two records in a record zone.

[`func setParent(CKRecord?)`](/documentation/cloudkit/ckrecord/setparent\(_:\)-23du1)

Creates and sets a reference object for a parent from its record.

[`enum SystemFieldKey`](/documentation/cloudkit/ckrecord/systemfieldkey)

Possible values for types of system field keys on records.

Current page is setParent(\_:)

---
# CKRecord.SystemFieldKey | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   CKRecord.SystemFieldKey

Enumeration

# CKRecord.SystemFieldKey

Possible values for types of system field keys on records.

```
enum SystemFieldKey
```

## [Overview](/documentation/cloudkit/ckrecord/systemfieldkey#overview)

Use the values [`share`](/documentation/cloudkit/ckrecord/systemfieldkey/share) and [`parent`](/documentation/cloudkit/ckrecord/systemfieldkey/parent) when creating an [`NSPredicate`](/documentation/Foundation/NSPredicate) for a [`CKQuery`](/documentation/cloudkit/ckquery) to reference a record’s [`share`](/documentation/cloudkit/ckrecord/share) or [`parent`](/documentation/cloudkit/ckrecord/parent) property, respectively.

## [Topics](/documentation/cloudkit/ckrecord/systemfieldkey#topics)

### [Types of Shared Records](/documentation/cloudkit/ckrecord/systemfieldkey#Types-of-Shared-Records)

[`static let parent: CKRecord.FieldKey`](/documentation/cloudkit/ckrecord/systemfieldkey/parent)

A value that represents the parent property of a record.

[`static let share: CKRecord.FieldKey`](/documentation/cloudkit/ckrecord/systemfieldkey/share)

A value that represents the share property of a record.

### [Type Properties](/documentation/cloudkit/ckrecord/systemfieldkey#Type-Properties)

[`static var creationDate: CKRecord.FieldKey`](/documentation/cloudkit/ckrecord/systemfieldkey/creationdate)

[`static var creatorUserRecordID: CKRecord.FieldKey`](/documentation/cloudkit/ckrecord/systemfieldkey/creatoruserrecordid)

[`static var lastModifiedUserRecordID: CKRecord.FieldKey`](/documentation/cloudkit/ckrecord/systemfieldkey/lastmodifieduserrecordid)

[`static var modificationDate: CKRecord.FieldKey`](/documentation/cloudkit/ckrecord/systemfieldkey/modificationdate)

[`static var recordID: CKRecord.FieldKey`](/documentation/cloudkit/ckrecord/systemfieldkey/recordid)

## [See Also](/documentation/cloudkit/ckrecord/systemfieldkey#see-also)

### [Sharing Records](/documentation/cloudkit/ckrecord/systemfieldkey#Sharing-Records)

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

Current page is CKRecord.SystemFieldKey

## Types of Shared Records
# parent | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.SystemFieldKey](/documentation/cloudkit/ckrecord/systemfieldkey)
-   parent

Type Property

# parent

A value that represents the parent property of a record.

```
static let parent: CKRecord.FieldKey
```

## [See Also](/documentation/cloudkit/ckrecord/systemfieldkey/parent#see-also)

### [Types of Shared Records](/documentation/cloudkit/ckrecord/systemfieldkey/parent#Types-of-Shared-Records)

[`static let share: CKRecord.FieldKey`](/documentation/cloudkit/ckrecord/systemfieldkey/share)

A value that represents the share property of a record.

Current page is parent

# share | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.SystemFieldKey](/documentation/cloudkit/ckrecord/systemfieldkey)
-   share

Type Property

# share

A value that represents the share property of a record.

```
static let share: CKRecord.FieldKey
```

## [See Also](/documentation/cloudkit/ckrecord/systemfieldkey/share#see-also)

### [Types of Shared Records](/documentation/cloudkit/ckrecord/systemfieldkey/share#Types-of-Shared-Records)

[`static let parent: CKRecord.FieldKey`](/documentation/cloudkit/ckrecord/systemfieldkey/parent)

A value that represents the parent property of a record.

Current page is share

##  Type Properties

# creationDate | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.SystemFieldKey](/documentation/cloudkit/ckrecord/systemfieldkey)
-   creationDate

Type Property

# creationDate

```
@backDeployed(before: macOS 14.0, iOS 17.0, tvOS 17.0, watchOS 10.0)
static var creationDate: CKRecord.FieldKey { get }
```

Current page is creationDate

# creatorUserRecordID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.SystemFieldKey](/documentation/cloudkit/ckrecord/systemfieldkey)
-   creatorUserRecordID

Type Property

# creatorUserRecordID

```
@backDeployed(before: macOS 14.0, iOS 17.0, tvOS 17.0, watchOS 10.0)
static var creatorUserRecordID: CKRecord.FieldKey { get }
```

Current page is creatorUserRecordID

# lastModifiedUserRecordID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.SystemFieldKey](/documentation/cloudkit/ckrecord/systemfieldkey)
-   lastModifiedUserRecordID

Type Property

# lastModifiedUserRecordID

```
@backDeployed(before: macOS 14.0, iOS 17.0, tvOS 17.0, watchOS 10.0)
static var lastModifiedUserRecordID: CKRecord.FieldKey { get }
```

Current page is lastModifiedUserRecordID

# modificationDate | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.SystemFieldKey](/documentation/cloudkit/ckrecord/systemfieldkey)
-   modificationDate

Type Property

# modificationDate

```
@backDeployed(before: macOS 14.0, iOS 17.0, tvOS 17.0, watchOS 10.0)
static var modificationDate: CKRecord.FieldKey { get }
```

Current page is modificationDate

# recordID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecord](/documentation/cloudkit/ckrecord)
-   [CKRecord.SystemFieldKey](/documentation/cloudkit/ckrecord/systemfieldkey)
-   recordID

Type Property

# recordID

```
@backDeployed(before: macOS 14.0, iOS 17.0, tvOS 17.0, watchOS 10.0)
static var recordID: CKRecord.FieldKey { get }
```

Current page is recordID

---
