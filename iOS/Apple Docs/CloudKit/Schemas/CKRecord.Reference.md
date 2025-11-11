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

---
# Creating a Reference
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

---
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

---

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