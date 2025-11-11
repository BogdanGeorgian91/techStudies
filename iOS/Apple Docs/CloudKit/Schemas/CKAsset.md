# CKAsset | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKAsset

Class

# CKAsset

An external file that belongs to a record.

```
class CKAsset
```

## [Mentioned in](/documentation/cloudkit/ckasset#mentions)

[

Encrypting User Data](/documentation/cloudkit/encrypting-user-data)

## [Overview](/documentation/cloudkit/ckasset#overview)

Use assets to incorporate external files into your app’s records, such as photos, videos, and binary files. Alternatively, use assets when a field’s value is more than a few kilobytes in size. To associate an instance of [`CKAsset`](/documentation/cloudkit/ckasset) with a record, assign it to one of its fields.

CloudKit stores an asset’s data separately from a record that references it, but maintains an association with that record. When you save a record that has an asset, CloudKit saves both the record and the asset to the server. Similarly, when you fetch the record, the server returns the record and the asset.

When you fetch a record that contains an asset, CloudKit stores the asset’s data in a staging area accessible to your app. Use the asset’s [`fileURL`](/documentation/cloudkit/ckasset/fileurl) property to access its staged location. The system regularly deletes files in the staging area to reclaim disk space. To avoid this behavior, move the data into your app’s container as soon as you fetch it.

If you don’t require the asset when retrieving records, use the operation’s `desiredKeys` property to exclude the field. For more information, see [`CKFetchRecordsOperation`](/documentation/cloudkit/ckfetchrecordsoperation), [`CKQueryOperation`](/documentation/cloudkit/ckqueryoperation), and [`CKFetchRecordZoneChangesOperation`](/documentation/cloudkit/ckfetchrecordzonechangesoperation).

If you no longer require an asset that’s on the server, you don’t delete it. Instead, orphan the asset by setting any fields that contain the asset to `nil` and then saving the record. CloudKit periodically deletes orphaned assets from the server.

## [Topics](/documentation/cloudkit/ckasset#topics)

### [Creating an Asset](/documentation/cloudkit/ckasset#Creating-an-Asset)

[`init(fileURL: URL)`](/documentation/cloudkit/ckasset/init\(fileurl:\))

Creates an asset that references a file.

### [Getting the URL of the Asset](/documentation/cloudkit/ckasset#Getting-the-URL-of-the-Asset)

[`var fileURL: URL?`](/documentation/cloudkit/ckasset/fileurl)

The URL for accessing the asset.

## [Relationships](/documentation/cloudkit/ckasset#relationships)

### [Inherits From](/documentation/cloudkit/ckasset#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckasset#conforms-to)

-   [`CKRecordValueProtocol`](/documentation/cloudkit/ckrecordvalueprotocol)
-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`Copyable`](/documentation/Swift/Copyable)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/ckasset#see-also)

### [Schemas](/documentation/cloudkit/ckasset#Schemas)

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

[`class Reference`](/documentation/cloudkit/ckrecord/reference)

A relationship between two records in a record zone.

[

Integrating a Text-Based Schema into Your Workflow](/documentation/cloudkit/integrating-a-text-based-schema-into-your-workflow)

Define and update your schema with the CloudKit Schema Language.

Current page is CKAsset

---
## Creating an Asset
# init(fileURL:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKAsset](/documentation/cloudkit/ckasset)
-   init(fileURL:)

Initializer

# init(fileURL:)

Creates an asset that references a file.

```
init(fileURL: URL)
```

## [Parameters](/documentation/cloudkit/ckasset/init\(fileurl:\)#parameters)

`fileURL`

The URL of the file that you want to store in CloudKit. The URL must be a file URL, and must not be `nil`.

## [Return Value](/documentation/cloudkit/ckasset/init\(fileurl:\)#return-value)

An asset object that represents the specified file, or `nil` if the system can’t create the asset.

## [Discussion](/documentation/cloudkit/ckasset/init\(fileurl:\)#Discussion)

Use this method to initialize new file-based assets that you want to transfer to iCloud. After saving an asset to the server, CloudKit doesn’t delete the file at the specified URL. If you no longer need the file, you must delete it yourself. When you subsequently download a record that contains an asset, CloudKit downloads its own copy of the asset data to the local device and provides you with a URL to that file.

You can assign only one record to the asset that this method returns. If you want multiple records to point to the same file, you must create separate assets for each one.

Current page is init(fileURL:)

---
## Getting the URL of the Asset
# fileURL | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKAsset](/documentation/cloudkit/ckasset)
-   fileURL

Instance Property

# fileURL

The URL for accessing the asset.

```
var fileURL: URL? { get }
```

## [Discussion](/documentation/cloudkit/ckasset/fileurl#Discussion)

After you create an asset, use the URL in this property to access the asset’s contents. The URL in this property is different from the one you specify when creating the asset.

Current page is fileURL