# CKAcceptSharesOperation

An operation that confirms a user’s participation in a share.

```
class CKAcceptSharesOperation
```

## [Overview](/documentation/cloudkit/ckacceptsharesoperation#overview)

Use this operation to accept participation in one or more shares. You create the operation with an array of share metadatas, which CloudKit provides to your app when the user taps or clicks a share’s [`url`](/documentation/cloudkit/ckshare/url). The method CloudKit calls varies by platform and app configuration. For more information, see [`CKShare.Metadata`](/documentation/cloudkit/ckshare/metadata). You can also fetch a share’s metadata using [`CKFetchShareMetadataOperation`](/documentation/cloudkit/ckfetchsharemetadataoperation).

If there are several metadatas, group them by their [`containerIdentifier`](/documentation/cloudkit/ckshare/metadata/containeridentifier) and create an operation for each container. Then add the operation to each container’s operation queue to run it. The operation executes its callbacks on a private serial queue.

The operation calls [`perShareCompletionBlock`](/documentation/cloudkit/ckacceptsharesoperation/persharecompletionblock) once for each metadata you provide. CloudKit returns the metadata and its related share, or an error if it can’t accept the share. CloudKit also batches per-metadata errors. If the operation completes with errors, it returns a [`partialFailure`](/documentation/cloudkit/ckerror/partialfailure) error. The error stores individual errors in its [`userInfo`](/documentation/Foundation/NSError/userInfo) dictionary. Use the [`CKPartialErrorsByItemIDKey`](/documentation/cloudkit/ckpartialerrorsbyitemidkey) key to extract them.

After CloudKit applies all record changes, the operation calls [`acceptSharesCompletionBlock`](/documentation/cloudkit/ckacceptsharesoperation/acceptsharescompletionblock). When the closure executes, the server may continue processing residual tasks of the operation, such as creating the record zone in the user’s private database.

The following example demonstrates how to accept a share that CloudKit provides to your window scene delegate. It shows how to create the operation, configure it, and execute it in the correct container:

```
func windowScene(_ windowScene: UIWindowScene, 
    userDidAcceptCloudKitShareWith cloudKitShareMetadata: CKShare.Metadata) {
    
    // Accept the share. If successful, schedule a fetch of the 
    // share's root record.
    acceptShare(metadata: cloudKitShareMetadata) { [weak self] result in
        switch result {
        case .success(let recordID):
            self?.fetchRootRecordAndNotifyObservers(recordID)
        case .failure(let error):
            // Handle the error...
        }
    }
}
    
func acceptShare(metadata: CKShare.Metadata, 
    completion: @escaping (Result<CKRecord.ID, Error>) -> Void) {
    
    // Create a reference to the share's container so the operation
    // executes in the correct context. 
    let container = CKContainer(identifier: metadata.containerIdentifier)
    
    // Create the operation using the metadata the caller provides.
    let operation = CKAcceptSharesOperation(shareMetadatas: [metadata])
        
    var rootRecordID: CKRecord.ID!
    // If CloudKit accepts the share, cache the root record's ID. 
    // The completion closure handles any errors.
    operation.perShareCompletionBlock = { metadata, share, error in
        if let _ = share, error == nil {
            rootRecordID = hierarchicalRootRecordID 
        }
    }


    // If the operation fails, return the error to the caller.
    // Otherwise, return the record ID of the share's root record.
    operation.acceptSharesCompletionBlock = { error in
        if let error = error {
            completion(.failure(error))
        } else {
            completion(.success(rootRecordID))
        }
    }


    // Set an appropriate QoS and add the operation to the
    // container's queue to execute it.
    operation.qualityOfService = .utility
    container.add(operation)
}
```

## [Topics](/documentation/cloudkit/ckacceptsharesoperation#topics)

### [Creating a Share Accept Operation](/documentation/cloudkit/ckacceptsharesoperation#Creating-a-Share-Accept-Operation)

[`init()`](/documentation/cloudkit/ckacceptsharesoperation/init\(\))

Creates an operation for accepting shares.

[`convenience init(shareMetadatas: [CKShare.Metadata])`](/documentation/cloudkit/ckacceptsharesoperation/init\(sharemetadatas:\))

Creates an operation for accepting the specified shares.

### [Processing the Share Accept Results](/documentation/cloudkit/ckacceptsharesoperation#Processing-the-Share-Accept-Results)

[`var shareMetadatas: [CKShare.Metadata]?`](/documentation/cloudkit/ckacceptsharesoperation/sharemetadatas)

The share metadatas to process.

[`var perShareCompletionBlock: ((CKShare.Metadata, CKShare?, (any Error)?) -> Void)?`](/documentation/cloudkit/ckacceptsharesoperation/persharecompletionblock)

The block to execute as CloudKit processes individual shares.

Deprecated

[`var acceptSharesCompletionBlock: (((any Error)?) -> Void)?`](/documentation/cloudkit/ckacceptsharesoperation/acceptsharescompletionblock)

The closure to execute when the operation finishes.

Deprecated

### [Instance Properties](/documentation/cloudkit/ckacceptsharesoperation#Instance-Properties)

[`var acceptSharesResultBlock: ((Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/ckacceptsharesoperation/acceptsharesresultblock)

[`var perShareResultBlock: ((CKShare.Metadata, Result<CKShare, any Error>) -> Void)?`](/documentation/cloudkit/ckacceptsharesoperation/pershareresultblock)

## [Relationships](/documentation/cloudkit/ckacceptsharesoperation#relationships)

### [Inherits From](/documentation/cloudkit/ckacceptsharesoperation#inherits-from)

-   [`CKOperation`](/documentation/cloudkit/ckoperation)

### [Conforms To](/documentation/cloudkit/ckacceptsharesoperation#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

# init(shareMetadatas:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKAcceptSharesOperation](/documentation/cloudkit/ckacceptsharesoperation)
-   init(shareMetadatas:)

Initializer

# init(shareMetadatas:)

Creates an operation for accepting the specified shares.

```
convenience init(shareMetadatas: [CKShare.Metadata])
```

## [Parameters](/documentation/cloudkit/ckacceptsharesoperation/init\(sharemetadatas:\)#parameters)

`shareMetadatas`

The share metadatas to accept. If you specify `nil`, you must assign a value to the [`shareMetadatas`](/documentation/cloudkit/ckacceptsharesoperation/sharemetadatas) property before you execute the operation.

## [Discussion](/documentation/cloudkit/ckacceptsharesoperation/init\(sharemetadatas:\)#Discussion)

After initializing the operation, assign a handler to the [`acceptSharesCompletionBlock`](/documentation/cloudkit/ckacceptsharesoperation/acceptsharescompletionblock) property to process the results.

# shareMetadatas | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKAcceptSharesOperation](/documentation/cloudkit/ckacceptsharesoperation)
-   shareMetadatas

Instance Property

# shareMetadatas

The share metadatas to process.

```
var shareMetadatas: [CKShare.Metadata]? { get set }
```

## [Discussion](/documentation/cloudkit/ckacceptsharesoperation/sharemetadatas#Discussion)

Use this property to view or change the metadata of the shares you want to process. If you intend to specify or change the value of this property, do so before you execute the operation or submit it to a queue.
# acceptSharesResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKAcceptSharesOperation](/documentation/cloudkit/ckacceptsharesoperation)
-   acceptSharesResultBlock

Instance Property

# acceptSharesResultBlock

```
var acceptSharesResultBlock: ((Result<Void, any Error>) -> Void)? { get set }
```

Current page is acceptSharesResultBlock

# perShareResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKAcceptSharesOperation](/documentation/cloudkit/ckacceptsharesoperation)
-   perShareResultBlock

Instance Property

# perShareResultBlock

```
var perShareResultBlock: ((CKShare.Metadata, Result<CKShare, any Error>) -> Void)? { get set }
```

Current page is perShareResultBlock