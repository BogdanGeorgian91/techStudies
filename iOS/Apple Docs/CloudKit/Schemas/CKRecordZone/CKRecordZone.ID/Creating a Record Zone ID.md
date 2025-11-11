# init(zoneName:ownerName:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   [CKRecordZone.ID](/documentation/cloudkit/ckrecordzone/id)
-   init(zoneName:ownerName:)

Initializer

# init(zoneName:ownerName:)

Creates a record zone ID with the specified name and owner.

```
convenience init(
    zoneName: String = CKRecordZone.ID.defaultZoneName,
    ownerName: String = CKCurrentUserDefaultName
)
```

## [Parameters](/documentation/cloudkit/ckrecordzone/id/init\(zonename:ownername:\)-22irr#parameters)

`zoneName`

The name that identifies the record zone. Zone names consist of up to 255 ASCII characters, and don’t start with an underscore. To specify the default zone of the current database, use [`defaultZoneName`](/documentation/cloudkit/ckrecordzone/id/defaultzonename). This parameter must not be `nil` or an empty string.

`ownerName`

The user who creates the record zone. To specify the current user, use [`CKCurrentUserDefaultName`](/documentation/cloudkit/ckcurrentuserdefaultname). If you provide `nil` or an empty string for this parameter, the method throws an exception.

## [Return Value](/documentation/cloudkit/ckrecordzone/id/init\(zonename:ownername:\)-22irr#return-value)

A new record zone ID, or `nil` if CloudKit can’t create the record zone ID.

Current page is init(zoneName:ownerName:)