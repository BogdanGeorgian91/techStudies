# CKFetchShareMetadataOperation

An operation that fetches metadata for one or more shares.

```
class CKFetchShareMetadataOperation
```

## [Overview](/documentation/cloudkit/ckfetchsharemetadataoperation#overview)

Use this operation to fetch the metadata for one or more shares. A share’s metadata contains the share and details about the user’s participation. Fetch metadata when you want to manually accept participation in a share using [`CKAcceptSharesOperation`](/documentation/cloudkit/ckacceptsharesoperation).

For a shared record hierarchy, the fetched metadata includes the record ID of the share’s root record. Set [`shouldFetchRootRecord`](/documentation/cloudkit/ckfetchsharemetadataoperation/shouldfetchrootrecord) to [`true`](/documentation/swift/true) to fetch the entire root record. You can further customize this behavior using [`rootRecordDesiredKeys`](/documentation/cloudkit/ckfetchsharemetadataoperation/rootrecorddesiredkeys-3xrex) to specify which fields you want to include in your fetch. This functionality isn’t applicable for a shared record zone because, unlike a shared record hierarchy, it doesn’t have a nominated root record.

To run the operation, add it to any container’s operation queue. Returned metadata includes the ID of the container that stores the share. The operation executes its callbacks on a private serial queue.

The operation calls [`perShareMetadataBlock`](/documentation/cloudkit/ckfetchsharemetadataoperation/persharemetadatablock) once for each URL you provide, and CloudKit returns the metadata, or an error if the fetch fails. CloudKit also batches per-URL errors. If the operation completes with errors, it returns a [`partialFailure`](/documentation/cloudkit/ckerror/partialfailure) error. The error stores individual errors in its [`userInfo`](/documentation/Foundation/NSError/userInfo) dictionary. Use the [`CKPartialErrorsByItemIDKey`](/documentation/cloudkit/ckpartialerrorsbyitemidkey) key to extract them.

When all of the following conditions are true, CloudKit returns a [`participantMayNeedVerification`](/documentation/cloudkit/ckerror/participantmayneedverification) error:

-   There are pending participants that don’t have matched iCloud accounts.
    
-   The current user has an active iCloud account and isn’t an existing participant (pending or otherwise).
    

On receipt of this error, call [`open(_:options:completionHandler:)`](/documentation/UIKit/UIApplication/open\(_:options:completionHandler:\)) with the share’s URL to allow CloudKit to verify the user.

The following example demonstrates how to create the operation, configure it, and then execute it using the default container’s operation queue:

```
func fetchShareMetadata(for shareURLs: [URL],
    completion: @escaping (Result<[URL: CKShare.Metadata], Error>) -> Void) {
        
    var cache = [URL: CKShare.Metadata]()
        
    // Create the fetch operation using the share URLs that
    // the caller provides to the method.
    let operation = CKFetchShareMetadataOperation(shareURLs: shareURLs)
        
    // To reduce network requests, request that CloudKit 
    // includes the root record in the metadata it returns.
    operation.shouldFetchRootRecord = true
        
    // Cache the metadata that CloudKit returns using the
    // share URL. This implementation ignores per-metadata
    // fetch errors and handles any errors in the completion
    // closure instead.
    operation.perShareMetadataBlock = { url, metadata, _ in
        guard let metadata = metadata else { return }
        cache[url] = metadata
    }
        
    // If the operation fails, return the error to the caller.
    // Otherwise, return the array of participants.
    operation.fetchShareMetadataCompletionBlock = { error in
        if let error = error {
            completion(.failure(error))
        } else {
            completion(.success(cache))
        }
    }
        
    // Set an appropriate QoS and add the operation to the
    // container's queue to execute it.
    operation.qualityOfService = .userInitiated
    CKContainer.default().add(operation)
}
```

## [Topics](/documentation/cloudkit/ckfetchsharemetadataoperation#topics)

### [Creating an Operation](/documentation/cloudkit/ckfetchsharemetadataoperation#Creating-an-Operation)

[`init()`](/documentation/cloudkit/ckfetchsharemetadataoperation/init\(\))

Creates an empty fetch share metadata operation.

[`convenience init(shareURLs: [URL])`](/documentation/cloudkit/ckfetchsharemetadataoperation/init\(shareurls:\))

Creates an operation for fetching the metadata for the specified shares.

### [Configuring the Operation](/documentation/cloudkit/ckfetchsharemetadataoperation#Configuring-the-Operation)

[`var shareURLs: [URL]?`](/documentation/cloudkit/ckfetchsharemetadataoperation/shareurls)

The URLs of the shares to fetch.

[`var shouldFetchRootRecord: Bool`](/documentation/cloudkit/ckfetchsharemetadataoperation/shouldfetchrootrecord)

A Boolean value that indicates whether to retrieve the root record.

### [Processing the Operation’s Results](/documentation/cloudkit/ckfetchsharemetadataoperation#Processing-the-Operations-Results)

[`var perShareMetadataBlock: ((URL, CKShare.Metadata?, (any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchsharemetadataoperation/persharemetadatablock)

The closure to execute as the operation fetches individual shares.

Deprecated

[`var fetchShareMetadataCompletionBlock: (((any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchsharemetadataoperation/fetchsharemetadatacompletionblock)

The closure to execute when the operation finishes.

Deprecated

### [Instance Properties](/documentation/cloudkit/ckfetchsharemetadataoperation#Instance-Properties)

[`var fetchShareMetadataResultBlock: ((Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/ckfetchsharemetadataoperation/fetchsharemetadataresultblock)

[`var perShareMetadataResultBlock: ((URL, Result<CKShare.Metadata, any Error>) -> Void)?`](/documentation/cloudkit/ckfetchsharemetadataoperation/persharemetadataresultblock)

[`var rootRecordDesiredKeys: [CKRecord.FieldKey]?`](/documentation/cloudkit/ckfetchsharemetadataoperation/rootrecorddesiredkeys-3xrex)

The fields to return when fetching the root record.

## [Relationships](/documentation/cloudkit/ckfetchsharemetadataoperation#relationships)

### [Inherits From](/documentation/cloudkit/ckfetchsharemetadataoperation#inherits-from)

-   [`CKOperation`](/documentation/cloudkit/ckoperation)

### [Conforms To](/documentation/cloudkit/ckfetchsharemetadataoperation#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

# init() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchShareMetadataOperation](/documentation/cloudkit/ckfetchsharemetadataoperation)
-   init()

Initializer

# init()

Creates an empty fetch share metadata operation.

```
init()
```

## [See Also](/documentation/cloudkit/ckfetchsharemetadataoperation/init\(\)#see-also)

### [Creating an Operation](/documentation/cloudkit/ckfetchsharemetadataoperation/init\(\)#Creating-an-Operation)

[`convenience init(shareURLs: [URL])`](/documentation/cloudkit/ckfetchsharemetadataoperation/init\(shareurls:\))

Creates an operation for fetching the metadata for the specified shares.

Current page is init()

# init(shareURLs:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchShareMetadataOperation](/documentation/cloudkit/ckfetchsharemetadataoperation)
-   init(shareURLs:)

Initializer

# init(shareURLs:)

Creates an operation for fetching the metadata for the specified shares.

```
convenience init(shareURLs: [URL])
```

## [Parameters](/documentation/cloudkit/ckfetchsharemetadataoperation/init\(shareurls:\)#parameters)

`shareURLs`

The URLs of the shares. If you specify `nil`, you must assign a value to the [`shareURLs`](/documentation/cloudkit/ckfetchsharemetadataoperation/shareurls) property before you execute the operation.

## [Discussion](/documentation/cloudkit/ckfetchsharemetadataoperation/init\(shareurls:\)#Discussion)

After creating the operation, assign a handler to the [`fetchShareMetadataCompletionBlock`](/documentation/cloudkit/ckfetchsharemetadataoperation/fetchsharemetadatacompletionblock) property to process the results.

---
  
Configuring the Operation

# shareURLs | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchShareMetadataOperation](/documentation/cloudkit/ckfetchsharemetadataoperation)
-   shareURLs

Instance Property

# shareURLs

The URLs of the shares to fetch.

```
var shareURLs: [URL]? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchsharemetadataoperation/shareurls#Discussion)

Use this property to view or change the URLs of the shares to fetch. If you intend to specify or change this property’s value, do so before you execute the operation or submit it to a queue.

## [See Also](/documentation/cloudkit/ckfetchsharemetadataoperation/shareurls#see-also)

### [Configuring the Operation](/documentation/cloudkit/ckfetchsharemetadataoperation/shareurls#Configuring-the-Operation)

[`var shouldFetchRootRecord: Bool`](/documentation/cloudkit/ckfetchsharemetadataoperation/shouldfetchrootrecord)

A Boolean value that indicates whether to retrieve the root record.

Current page is shareURLs

# shouldFetchRootRecord | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchShareMetadataOperation](/documentation/cloudkit/ckfetchsharemetadataoperation)
-   shouldFetchRootRecord

Instance Property

# shouldFetchRootRecord

A Boolean value that indicates whether to retrieve the root record.

```
var shouldFetchRootRecord: Bool { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchsharemetadataoperation/shouldfetchrootrecord#Discussion)

For a shared record hierarchy, set this property to [`true`](/documentation/swift/true) to include the root record in the fetched share metadata. CloudKit ignores this property for a shared record zone because, unlike a shared record hierarchy, it doesn’t have a nominated root record.

The default value is [`false`](/documentation/swift/false).

---
instance properties
# fetchShareMetadataResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchShareMetadataOperation](/documentation/cloudkit/ckfetchsharemetadataoperation)
-   fetchShareMetadataResultBlock

Instance Property

# fetchShareMetadataResultBlock

```
var fetchShareMetadataResultBlock: ((Result<Void, any Error>) -> Void)? { get set }
```

Current page is fetchShareMetadataResultBlock

# perShareMetadataResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchShareMetadataOperation](/documentation/cloudkit/ckfetchsharemetadataoperation)
-   perShareMetadataResultBlock

Instance Property

# perShareMetadataResultBlock

```
var perShareMetadataResultBlock: ((URL, Result<CKShare.Metadata, any Error>) -> Void)? { get set }
```

Current page is perShareMetadataResultBlock

# rootRecordDesiredKeys | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchShareMetadataOperation](/documentation/cloudkit/ckfetchsharemetadataoperation)
-   rootRecordDesiredKeys

Instance Property

# rootRecordDesiredKeys

The fields to return when fetching the root record.

```
var rootRecordDesiredKeys: [CKRecord.FieldKey]? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchsharemetadataoperation/rootrecorddesiredkeys-3xrex#Discussion)

For a shared record hierarchy, and when [`shouldFetchRootRecord`](/documentation/cloudkit/ckfetchsharemetadataoperation/shouldfetchrootrecord) is [`true`](/documentation/swift/true), set this property to specify which of the root record’s fields the operation fetches. Use `nil` to fetch the entire record. CloudKit ignores this property for a shared record zone because, unlike a hierarchy, it doesn’t have a nominated root record.

The default value is `nil`.

Current page is rootRecordDesiredKeys