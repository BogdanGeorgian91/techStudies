# CKRecordZone | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKRecordZone

Class

# CKRecordZone

A database partition that contains related records.

```
class CKRecordZone
```

## [Overview](/documentation/cloudkit/ckrecordzone#overview)

Zones are an important part of how you organize your data. The public and private databases each have a single default zone. In the private database, you can use [`CKRecordZone`](/documentation/cloudkit/ckrecordzone) objects to create additional custom zones as necessary. Use custom zones to arrange and encapsulate groups of related records in the private database. Custom zones support other capabilities too, such as the ability to write multiple records as a single atomic transaction.

Treat each custom zone as a single unit of data that is separate from every other zone in the database. Inside the zone, you add records as you would anywhere else. You can also create links between the records inside a zone by using the [`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference) class. However, the [`CKRecord.Reference`](/documentation/cloudkit/ckrecord/reference) class doesn’t support cross-zone linking, so each reference object must point to a record in the same zone as the current record.

Use the [`CKRecordZone`](/documentation/cloudkit/ckrecordzone) class as-is and don’t subclass it.

### [Creating a Custom Record Zone](/documentation/cloudkit/ckrecordzone#Creating-a-Custom-Record-Zone)

Generally, you use instances of this class to create and manage custom zones. Although you can use this class to retrieve a database’s default zone, most operations act on records in the default zone by default, so you rarely need to specify it explicitly.

To create a custom zone, use [`CKRecordZone`](/documentation/cloudkit/ckrecordzone) to create the zone object, and then save that zone to the user’s private database using a [`CKModifyRecordZonesOperation`](/documentation/cloudkit/ckmodifyrecordzonesoperation) object. You can’t save any records in the zone until you save it to the database. When creating records, explicitly specify the zone ID if you want the records to reside in a specific zone; otherwise, they save to the default zone. You can’t create custom zones in a public database.

After creating a `CKRecordZone` object and saving it to the database, you don’t interact with the object much. Instead, most interactions occur with its corresponding [`CKRecordZone.ID`](/documentation/cloudkit/ckrecordzone/id) object, which you use to refer to the zone when creating records.

## [Topics](/documentation/cloudkit/ckrecordzone#topics)

### [Creating a Record Zone](/documentation/cloudkit/ckrecordzone#Creating-a-Record-Zone)

[`init(zoneName: String)`](/documentation/cloudkit/ckrecordzone/init\(zonename:\))

Creates a record zone object with the specified zone name.

[`init(zoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckrecordzone/init\(zoneid:\))

Creates a record zone object with the specified zone ID.

[`class ID`](/documentation/cloudkit/ckrecordzone/id)

An object that uniquely identifies a record zone in a database.

### [Getting the Default Record Zone](/documentation/cloudkit/ckrecordzone#Getting-the-Default-Record-Zone)

[``class func `default`() -> CKRecordZone``](/documentation/cloudkit/ckrecordzone/default\(\))

Returns the default record zone.

### [Getting the Zone Attributes](/documentation/cloudkit/ckrecordzone#Getting-the-Zone-Attributes)

[`var zoneID: CKRecordZone.ID`](/documentation/cloudkit/ckrecordzone/zoneid)

The unique ID of the zone.

[`var capabilities: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.property)

The capabilities that the zone supports.

[`struct Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct)

The capabilities that a record zone supports.

### [Sharing Records](/documentation/cloudkit/ckrecordzone#Sharing-Records)

[`var share: CKRecord.Reference?`](/documentation/cloudkit/ckrecordzone/share)

A reference to the record zone’s share record.

### [Instance Properties](/documentation/cloudkit/ckrecordzone#Instance-Properties)

[`var encryptionScope: CKRecordZone.EncryptionScope`](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.property)

The encryption scope determines the granularity at which encryption keys are stored within the zone.

### [Enumerations](/documentation/cloudkit/ckrecordzone#Enumerations)

[`enum EncryptionScope`](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.enum)

## [Relationships](/documentation/cloudkit/ckrecordzone#relationships)

### [Inherits From](/documentation/cloudkit/ckrecordzone#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckrecordzone#conforms-to)

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

## [See Also](/documentation/cloudkit/ckrecordzone#see-also)

### [Schemas](/documentation/cloudkit/ckrecordzone#Schemas)

[

Designing and Creating a CloudKit Database](/documentation/cloudkit/designing-and-creating-a-cloudkit-database)

Create a schema to store your app’s objects as records in iCloud using CloudKit.

[

API Reference

Managing iCloud Containers with CloudKit Database App](/documentation/cloudkit/managing-icloud-containers-with-cloudkit-database-app)

Inspect and modify the schema and data for your app’s iCloud container.

[`class CKRecord`](/documentation/cloudkit/ckrecord)

A collection of key-value pairs that store your app’s data.

[`class Reference`](/documentation/cloudkit/ckrecord/reference)

A relationship between two records in a record zone.

[`class CKAsset`](/documentation/cloudkit/ckasset)

An external file that belongs to a record.

[

Integrating a Text-Based Schema into Your Workflow](/documentation/cloudkit/integrating-a-text-based-schema-into-your-workflow)

Define and update your schema with the CloudKit Schema Language.

Current page is CKRecordZone