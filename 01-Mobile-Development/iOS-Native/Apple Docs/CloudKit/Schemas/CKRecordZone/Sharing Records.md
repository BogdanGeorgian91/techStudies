# share | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKRecordZone](/documentation/cloudkit/ckrecordzone)
-   share

Instance Property

# share

A reference to the record zone’s share record.

```
@NSCopying
var share: CKRecord.Reference? { get }
```

## [Discussion](/documentation/cloudkit/ckrecordzone/share#Discussion)

CloudKit sets this property only for fetched record zones that contain a share record; otherwise, it’s `nil`.

To share a record zone, create a share record using the [`init(recordZoneID:)`](/documentation/cloudkit/ckshare/init\(recordzoneid:\)) method and then save it to the server. Shared record zones must have the [`zoneWideSharing`](/documentation/cloudkit/ckrecordzone/capabilities-swift.struct/zonewidesharing) capability, which CloudKit enables by default for new custom record zones in the user’s private database.

A record zone, and the records it contains, can take part in only a single share. CloudKit returns an error if you attempt to share an already-shared record zone, or if that record zone contains previously shared records.

Record zone sharing errors include the following:

-   [`CKError.Code.serverRecordChanged`](/documentation/cloudkit/ckerror/code/serverrecordchanged), which CloudKit returns if you try to share an already-shared record zone.
    
-   [`CKError.Code.serverRejectedRequest`](/documentation/cloudkit/ckerror/code/serverrejectedrequest), which CloudKit returns if you try to share a record hierarchy from an already-shared record zone.
    
-   [`CKError.Code.invalidArguments`](/documentation/cloudkit/ckerror/code/invalidarguments), which CloudKit returns if you try to share a record zone that contains one or more shared hierarchies.
    

Current page is share