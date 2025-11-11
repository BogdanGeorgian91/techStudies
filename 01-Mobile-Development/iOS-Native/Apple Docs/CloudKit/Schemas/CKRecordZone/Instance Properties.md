# encryptionScope | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   encryptionScope

Instance Property

# encryptionScope

The encryption scope determines the granularity at which encryption keys are stored within the zone.

```
var encryptionScope: CKRecordZone.EncryptionScope { get set }
```

## [Discussion](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.property#discussion)

Zone encryption scope defaults to `CKRecordZoneEncryptionScopePerRecord` and can only be modified before zone creation. Attempting to change the encryption scope of an existing zone is invalid and will result in an error.

Zones using `CKRecordZoneEncryptionScopePerZone` can only use zone-wide sharing and are not compatible with older device OS versions. Refer to `CKRecordZoneEncryptionScope` for more info.

Current page is encryptionScope