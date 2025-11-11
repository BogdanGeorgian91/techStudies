# init(zoneName:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   init(zoneName:)

Initializer

# init(zoneName:)

Creates a record zone object with the specified zone name.

```
init(zoneName: String)
```

## [Parameters](/documentation/cloudkit/ckrecordzone/init\(zonename:\)#parameters)

`zoneName`

The name of the new zone. Zone names inside a user’s private database are unique, consist of up to 255 ASCII characters, and don’t start with an underscore. One way to ensure the uniqueness of zone names is to create a string from a UUID, but you can also use other techniques.

If this parameter is `nil` or is an empty string, the method throws an exception.

## [Return Value](/documentation/cloudkit/ckrecordzone/init\(zonename:\)#return-value)

The new custom zone, or `nil` if CloudKit can’t create the zone.

## [Discussion](/documentation/cloudkit/ckrecordzone/init\(zonename:\)#Discussion)

Use this method to create a new record zone. The new zone has the name you provide and the zone’s owner is the current user. After creating the zone, save it to the server using a [`CKModifyRecordZonesOperation`](/documentation/cloudkit/ckmodifyrecordzonesoperation) object or the [`save(_:completionHandler:)`](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-32ffr) method of [`CKDatabase`](/documentation/cloudkit/ckdatabase). You must save the zone to the server before you attempt to save any records to that zone.

Don’t use this method to create a `CKRecordZone` object that corresponds to a zone that already exists in the database. If the zone exists, fetch it using a [`CKFetchRecordZonesOperation`](/documentation/cloudkit/ckfetchrecordzonesoperation) object or the [`fetch(withRecordZoneID:completionHandler:)`](/documentation/cloudkit/ckdatabase/fetch\(withrecordzoneid:completionhandler:\)) method of [`CKDatabase`](/documentation/cloudkit/ckdatabase).

## [See Also](/documentation/cloudkit/ckrecordzone/init\(zonename:\)#see-also)

### [Creating a Record Zone](/documentation/cloudkit/ckrecordzone/init\(zonename:\)#Creating-a-Record-Zone)

[`init(zoneID: CKRecordZone.ID)`](/documentation/cloudkit/ckrecordzone/init\(zoneid:\))

Creates a record zone object with the specified zone ID.

[`class ID`](/documentation/cloudkit/ckrecordzone/id)

An object that uniquely identifies a record zone in a database.

Current page is init(zoneName:)

---
# init(zoneID:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   init(zoneID:)

Initializer

# init(zoneID:)

Creates a record zone object with the specified zone ID.

```
init(zoneID: CKRecordZone.ID)
```

## [Parameters](/documentation/cloudkit/ckrecordzone/init\(zoneid:\)#parameters)

`zoneID`

The ID for the new zone. This parameter must not be `nil`.

## [Return Value](/documentation/cloudkit/ckrecordzone/init\(zoneid:\)#return-value)

The custom record zone, or `nil` if CloudKit can’t create the zone.

## [Discussion](/documentation/cloudkit/ckrecordzone/init\(zoneid:\)#Discussion)

Use this method when you want to create a new record zone from the information in a zone ID. After creating the zone, save it to the server using a [`CKModifyRecordZonesOperation`](/documentation/cloudkit/ckmodifyrecordzonesoperation) object or the [`save(_:completionHandler:)`](/documentation/cloudkit/ckdatabase/save\(_:completionhandler:\)-32ffr) method of [`CKDatabase`](/documentation/cloudkit/ckdatabase).

Don’t use this method to create a [`CKRecordZone`](/documentation/cloudkit/ckrecordzone) object that corresponds to a zone that already exists in the database. If the zone exists, fetch it using a [`CKFetchRecordZonesOperation`](/documentation/cloudkit/ckfetchrecordzonesoperation) object or the [`fetch(withRecordZoneID:completionHandler:)`](/documentation/cloudkit/ckdatabase/fetch\(withrecordzoneid:completionhandler:\)) method of [`CKDatabase`](/documentation/cloudkit/ckdatabase).

## [See Also](/documentation/cloudkit/ckrecordzone/init\(zoneid:\)#see-also)

### [Creating a Record Zone](/documentation/cloudkit/ckrecordzone/init\(zoneid:\)#Creating-a-Record-Zone)

[`init(zoneName: String)`](/documentation/cloudkit/ckrecordzone/init\(zonename:\))

Creates a record zone object with the specified zone name.

[`class ID`](/documentation/cloudkit/ckrecordzone/id)

An object that uniquely identifies a record zone in a database.

Current page is init(zoneID:)