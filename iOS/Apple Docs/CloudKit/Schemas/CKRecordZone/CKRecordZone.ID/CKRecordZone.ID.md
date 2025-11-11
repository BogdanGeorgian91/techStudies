# CKRecordZone.ID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   CKRecordZone.ID

Class

# CKRecordZone.ID

An object that uniquely identifies a record zone in a database.

```
class ID
```

## [Overview](/documentation/cloudkit/ckrecordzone/id#overview)

Zones are a mechanism for grouping related records together. You create zone ID objects when you want to fetch an existing zone object or create a new zone with a specific name.

A record zone ID distinguishes one zone from another by a name string and the ID of the user who creates the zone. Both strings must be ASCII strings that don’t exceed 255 characters. When creating your own record zone ID objects, you can use names that have more meaning to your app or to the user, providing each zone name is unique within the specified database. The owner name must be either the current user name or the name of another user. Get the current user name from [`CKCurrentUserDefaultName`](/documentation/cloudkit/ckcurrentuserdefaultname) or by calling [`fetchUserRecordID(completionHandler:)`](/documentation/cloudkit/ckcontainer/fetchuserrecordid\(completionhandler:\)).

When creating new record zones, make the name string in the record zone ID unique in the target database. Public databases don’t support custom zones, and only the user who owns the database can create zones in private databases.

Don’t create subclasses of this class.

### [Interacting with Record Zone IDs](/documentation/cloudkit/ckrecordzone/id#Interacting-with-Record-Zone-IDs)

After you create a record zone ID, interactions with it typically include:

-   Creating a [`CKRecord.ID`](/documentation/cloudkit/ckrecord/id) object so that you can fetch or create records in that zone.
    
-   Retrieving an existing [`CKRecordZone`](/documentation/cloudkit/ckrecordzone) object from the database.
    

You don’t need to create a record zone ID to create a record zone. The [`CKRecordZone`](/documentation/cloudkit/ckrecordzone) class has initialization methods that create a record zone ID using the name string you provide.

#### [Creating Record Zone IDs for Records](/documentation/cloudkit/ckrecordzone/id#Creating-Record-Zone-IDs-for-Records)

To create a new record in a custom zone, create a record zone ID that specifies the zone name. Use the record zone ID to create a [`CKRecord.ID`](/documentation/cloudkit/ckrecord/id), and then use the record ID to create the record.

#### [Fetching a Record Zone Object from the Database](/documentation/cloudkit/ckrecordzone/id#Fetching-a-Record-Zone-Object-from-the-Database)

To fetch a record zone from the database, use a [`CKFetchRecordZonesOperation`](/documentation/cloudkit/ckfetchrecordzonesoperation) object or the [`fetch(withRecordZoneID:completionHandler:)`](/documentation/cloudkit/ckdatabase/fetch\(withrecordzoneid:completionhandler:\)) method of [`CKDatabase`](/documentation/cloudkit/ckdatabase). Both techniques accept a record zone ID that you provide and retrieve the corresponding record zone object asynchronously. If you use the operation object, you can retrieve multiple record zones at the same time.

## [Topics](/documentation/cloudkit/ckrecordzone/id#topics)

### [Creating a Record Zone ID](/documentation/cloudkit/ckrecordzone/id#Creating-a-Record-Zone-ID)

[`convenience init(zoneName: String, ownerName: String)`](/documentation/cloudkit/ckrecordzone/id/init\(zonename:ownername:\)-22irr)

Creates a record zone ID with the specified name and owner.

### [Getting the Record Zone ID Attributes](/documentation/cloudkit/ckrecordzone/id#Getting-the-Record-Zone-ID-Attributes)

[`var zoneName: String`](/documentation/cloudkit/ckrecordzone/id/zonename)

The unique name of the record zone.

[`var ownerName: String`](/documentation/cloudkit/ckrecordzone/id/ownername)

The ID of the user who owns the record zone.

### [Accessing the Default Zone](/documentation/cloudkit/ckrecordzone/id#Accessing-the-Default-Zone)

[``static let `default`: CKRecordZone.ID``](/documentation/cloudkit/ckrecordzone/id/default)

The default zone ID.

[`static let defaultZoneName: String`](/documentation/cloudkit/ckrecordzone/id/defaultzonename)

The name of the default zone.

### [Initializers](/documentation/cloudkit/ckrecordzone/id#Initializers)

[`convenience init(zoneName: String, ownerName: String?)`](/documentation/cloudkit/ckrecordzone/id/init\(zonename:ownername:\)-2hzo4)Deprecated

## [Relationships](/documentation/cloudkit/ckrecordzone/id#relationships)

### [Inherits From](/documentation/cloudkit/ckrecordzone/id#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckrecordzone/id#conforms-to)

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

## [See Also](/documentation/cloudkit/ckrecordzone/id#see-also)

### [Creating a Record Zone](/documentation/cloudkit/ckrecordzone/id#Creating-a-Record-Zone)

[`init(zoneName: String)`](/documentation/cloudkit/ckrecordzone/init\(zonename:\))

Creates a record zone object with the specified zone name.

[`init(zoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckrecordzone/init\(zoneid:\))

Creates a record zone object with the specified zone ID.

Current page is CKRecordZone.ID