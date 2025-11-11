# CKRecordZone.Capabilities | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   CKRecordZone.Capabilities

Structure

# CKRecordZone.Capabilities

The capabilities that a record zone supports.

```
struct Capabilities
```

## [Topics](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct#topics)

### [Creating Zone Capabilities](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct#Creating-Zone-Capabilities)

[`init(rawValue: UInt)`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/init\(rawvalue:\))

Creates a set of capabilities for a record zone.

### [Zone Capabilities](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct#Zone-Capabilities)

[`static var atomic: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/atomic)

A capability that allows atomic changes of multiple records.

[`static var fetchChanges: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/fetchchanges)

A capability for fetching only the changed records from a zone.

[`static var sharing: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/sharing)

A capability for sharing a specific hierarchy of records.

[`static var zoneWideSharing: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/zonewidesharing)

A capability for sharing the entire contents of a record zone.

## [Relationships](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct#relationships)

### [Conforms To](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`ExpressibleByArrayLiteral`](/documentation/Swift/ExpressibleByArrayLiteral)
-   [`OptionSet`](/documentation/Swift/OptionSet)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)
-   [`SetAlgebra`](/documentation/Swift/SetAlgebra)

## [See Also](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct#see-also)

### [Getting the Zone Attributes](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct#Getting-the-Zone-Attributes)

[`var zoneID: CKRecordZone.ID`](/documentation/cloudkit/ckrecordzone/zoneid)

The unique ID of the zone.

[`var capabilities: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.property)

The capabilities that the zone supports.

Current page is CKRecordZone.Capabilities

---
#     Creating Zone Capabilities

# init(rawValue:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   [CKRecordZone.Capabilities](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct)
-   init(rawValue:)

Initializer

# init(rawValue:)

Creates a set of capabilities for a record zone.

```
init(rawValue: UInt)
```

## [Parameters](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/init\(rawvalue:\)#parameters)

`rawValue`

An integer that represents the combined set of capabilities to create.

## [Discussion](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/init\(rawvalue:\)#Discussion)

Capabilities on record zones that you create locally aren’t valid until you save the record zone. Capabilities on record zones that you fetch from the server are always valid.

Current page is init(rawValue:)

---
  
# Zone Capabilities

# atomic | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   [CKRecordZone.Capabilities](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct)
-   atomic

Type Property

# atomic

A capability that allows atomic changes of multiple records.

```
static var atomic: CKRecordZone.Capabilities { get }
```

## [Discussion](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/atomic#Discussion)

When you use a [`CKModifyRecordsOperation`](/documentation/cloudkit/ckmodifyrecordsoperation) object to save records, if the server is unable to save the changes for one record, it doesn’t save the changes for any of the records. Combining this capability with the [`CKModifyRecordsOperation.RecordSavePolicy.ifServerRecordUnchanged`](/documentation/cloudkit/ckmodifyrecordsoperation/recordsavepolicy/ifserverrecordunchanged) policy of the operation object prevents your app from overwriting changes to a group of records if one or more of the records on the server has recent changes.

## [See Also](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/atomic#see-also)

### [Zone Capabilities](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/atomic#Zone-Capabilities)

[`static var fetchChanges: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/fetchchanges)

A capability for fetching only the changed records from a zone.

[`static var sharing: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/sharing)

A capability for sharing a specific hierarchy of records.

[`static var zoneWideSharing: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/zonewidesharing)

A capability for sharing the entire contents of a record zone.

Current page is atomic

---
# fetchChanges | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   [CKRecordZone.Capabilities](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct)
-   fetchChanges

Type Property

# fetchChanges

A capability for fetching only the changed records from a zone.

```
static var fetchChanges: CKRecordZone.Capabilities { get }
```

## [Discussion](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/fetchchanges#Discussion)

This capability makes the creation of offline caches more efficient. Instead of fetching the entire record every time, use [`CKFetchRecordZoneChangesOperation`](/documentation/cloudkit/ckfetchrecordzonechangesoperation) to fetch only the changed values, and use the data it returns to update your cache. This minimizes the amount of data you receive from the server.

## [See Also](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/fetchchanges#see-also)

### [Zone Capabilities](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/fetchchanges#Zone-Capabilities)

[`static var atomic: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/atomic)

A capability that allows atomic changes of multiple records.

[`static var sharing: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/sharing)

A capability for sharing a specific hierarchy of records.

[`static var zoneWideSharing: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/zonewidesharing)

A capability for sharing the entire contents of a record zone.

Current page is fetchChanges

---
# sharing | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   [CKRecordZone.Capabilities](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct)
-   sharing

Type Property

# sharing

A capability for sharing a specific hierarchy of records.

```
static var sharing: CKRecordZone.Capabilities { get }
```

## [Discussion](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/sharing#Discussion)

CloudKit allows you to share record hierarchies from custom record zones that you create in the user’s private database. For more information, see [Shared Records](/documentation/cloudkit/shared-records).

## [See Also](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/sharing#see-also)

### [Zone Capabilities](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/sharing#Zone-Capabilities)

[`static var atomic: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/atomic)

A capability that allows atomic changes of multiple records.

[`static var fetchChanges: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/fetchchanges)

A capability for fetching only the changed records from a zone.

[`static var zoneWideSharing: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/zonewidesharing)

A capability for sharing the entire contents of a record zone.

Current page is sharing

---
# zoneWideSharing | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   [CKRecordZone.Capabilities](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct)
-   zoneWideSharing

Type Property

# zoneWideSharing

A capability for sharing the entire contents of a record zone.

```
static var zoneWideSharing: CKRecordZone.Capabilities { get }
```

## [Discussion](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/zonewidesharing#Discussion)

CloudKit allows you to share custom record zones that you create in the user’s private database. For more information, see [Shared Records](/documentation/cloudkit/shared-records).

## [See Also](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/zonewidesharing#see-also)

### [Zone Capabilities](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/zonewidesharing#Zone-Capabilities)

[`static var atomic: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/atomic)

A capability that allows atomic changes of multiple records.

[`static var fetchChanges: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/fetchchanges)

A capability for fetching only the changed records from a zone.

[`static var sharing: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/sharing)

A capability for sharing a specific hierarchy of records.

Current page is zoneWideSharing

---
