# CKRecordZone.EncryptionScope | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   CKRecordZone.EncryptionScope

Enumeration

# CKRecordZone.EncryptionScope

```
enum EncryptionScope
```

## [Topics](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.enum#topics)

### [Enumeration Cases](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.enum#Enumeration-Cases)

[`case perRecord`](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.enum/perrecord)

Zone uses per-record encryption keys for any encrypted values on a record or share.

[`case perZone`](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.enum/perzone)

Zone uses per-zone encryption keys for encrypted values across all records and the zone-wide share, if present.

### [Initializers](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.enum#Initializers)

[`init?(rawValue: Int)`](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.enum/init\(rawvalue:\))

## [Relationships](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.enum#relationships)

### [Conforms To](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.enum#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

Current page is CKRecordZone.EncryptionScope

---
### Enumeration Cases

#### CKRecordZone.EncryptionScope.perRecord | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   [CKRecordZone.EncryptionScope](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.enum)
-   CKRecordZone.EncryptionScope.perRecord

Case

#### CKRecordZone.EncryptionScope.perRecord

Zone uses per-record encryption keys for any encrypted values on a record or share.

```
case perRecord
```

#### [Discussion](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.enum/perrecord#discussion)

This is the default encryption scope for a record zone.

Current page is CKRecordZone.EncryptionScope.perRecord

---
#### CKRecordZone.EncryptionScope.perZone | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   [CKRecordZone.EncryptionScope](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.enum)
-   CKRecordZone.EncryptionScope.perZone

Case

#### CKRecordZone.EncryptionScope.perZone

Zone uses per-zone encryption keys for encrypted values across all records and the zone-wide share, if present.

```
case perZone
```

#### [Discussion](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.enum/perzone#discussion)

This is an optional optimization that can reduce the overall storage used by encryption keys in a zone. Note that:

-   Record zones using per-zone encryption only support zone-wide sharing.
    
-   Encryption scope can only be assigned at zone creation and cannot be changed for the lifetime of the zone.
    
-   The server will not return zones using per-zone encryption to device OS versions older than the corresponding API availability version.
    
-   An older OS trying to overwrite an existing zone using per-zone encryption due to a naming collision will result in a `.serverRejectedRequest` error.
    
-   On device OS upgrade, your application is responsible for fetching database changes via `CKFetchDatabaseChangesOperation` with a nil sync token to ensure it has received all the zones available to it from the server.
    

Current page is CKRecordZone.EncryptionScope.perZone

### initializers
# init(rawValue:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   [CKRecordZone.EncryptionScope](/documentation/cloudkit/ckrecordzone/encryptionscope-swift.enum)
-   init(rawValue:)

Initializer

# init(rawValue:)

```
init?(rawValue: Int)
```

Current page is init(rawValue:)