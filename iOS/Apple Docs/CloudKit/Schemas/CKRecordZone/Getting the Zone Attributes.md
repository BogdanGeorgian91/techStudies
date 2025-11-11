# zoneID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   zoneID

Instance Property

# zoneID

The unique ID of the zone.

```
@NSCopying
var zoneID: CKRecordZone.ID { get }
```

## [Discussion](/documentation/cloudkit/ckrecordzone/zoneid#Discussion)

The zone ID contains the name of the zone and the name of the user who owns the zone. Use this property to access both of those values.

## [See Also](/documentation/cloudkit/ckrecordzone/zoneid#see-also)

### [Getting the Zone Attributes](/documentation/cloudkit/ckrecordzone/zoneid#Getting-the-Zone-Attributes)

[`var capabilities: CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.property)

The capabilities that the zone supports.

[`struct Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct)

The capabilities that a record zone supports.

Current page is zoneID

---
# capabilities | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   capabilities

Instance Property

# capabilities

The capabilities that the zone supports.

```
var capabilities: CKRecordZone.Capabilities { get }
```

## [Discussion](/documentation/cloudkit/ckrecordzone/capabilities-swift.property#Discussion)

The server determines the capabilities of the zone and sets the value of this property when you save the record zone. Always check this property before performing tasks that require a specific capability.

Default zones don’t support any special capabilities. Custom zones in a private database support the options that [`CKRecordZone.Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct) provides.

## [See Also](/documentation/cloudkit/ckrecordzone/capabilities-swift.property#see-also)

### [Getting the Zone Attributes](/documentation/cloudkit/ckrecordzone/capabilities-swift.property#Getting-the-Zone-Attributes)

[`var zoneID: CKRecordZone.ID`](/documentation/cloudkit/ckrecordzone/zoneid)

The unique ID of the zone.

[`struct Capabilities`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct)

The capabilities that a record zone supports.

Current page is capabilities