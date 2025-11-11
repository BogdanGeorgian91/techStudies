
# CKFetchShareParticipantsOperation

An operation that converts user identities into share participants.

```
class CKFetchShareParticipantsOperation
```

## [Overview](/documentation/cloudkit/ckfetchshareparticipantsoperation#overview)

Participants are a fundamental part of sharing in CloudKit. A participant provides information about a user and their participation in a share, which includes their identity, acceptance status, role, and permissions. The acceptance status manages the user’s visibilty of the shared records. The role and permissions control what actions the user can perform on those records.

You don’t create participants. Instead, create an instance of [`CKUserIdentity.LookupInfo`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class) for each user. Provide the user’s email address or phone number, and then use this operation to convert them into participants that you can add to a share. CloudKit limits the number of participants in a share to 100, and each participant must have an active iCloud account.

CloudKit queries iCloud for corresponding accounts as part of the operation. If it doesn’t find an account, the server updates the participant’s [`userIdentity`](/documentation/cloudkit/ckshare/participant/useridentity) to reflect that by setting the [`hasiCloudAccount`](/documentation/cloudkit/ckuseridentity/hasicloudaccount) property to [`false`](/documentation/swift/false). CloudKit associates a participant with their iCloud account when they accept the share.

Anyone with the URL of a public share can become a participant in that share. For a private share, the owner manages its participants. A participant can’t accept a private share unless the owner adds them first.

To run the operation, add it to the container’s operation queue. The operation executes its callbacks on a private serial queue.

The following example demonstrates how to create the operation, configure it, and then execute it using the default container’s operation queue:

```
func fetchParticipants(for lookupInfos: [CKUserIdentity.LookupInfo], 
    completion: @escaping (Result<[CKShare.Participant], Error>) -> Void) {


    var participants = [CKShare.Participant]()
        
    // Create the operation using the lookup objects
    // that the caller provides to the method.
    let operation = CKFetchShareParticipantsOperation(
        userIdentityLookupInfos: lookupInfos)
        
    // Collect the participants as CloudKit generates them.
    operation.shareParticipantFetchedBlock = { participant in
        participants.append(participant)
    }
        
    // If the operation fails, return the error to the caller.
    // Otherwise, return the array of participants.
    operation.fetchShareParticipantsCompletionBlock = { error in
        if let error = error {
            completion(.failure(error))
        } else {
            completion(.success(participants))
        }
    }
        
    // Set an appropriate QoS and add the operation to the
    // container's queue to execute it.
    operation.qualityOfService = .userInitiated
    CKContainer.default().add(operation)
}
```

The operation calls [`shareParticipantFetchedBlock`](/documentation/cloudkit/ckfetchshareparticipantsoperation/shareparticipantfetchedblock) once for each item you provide, and CloudKit returns the participant, or an error if it can’t generate a particpant. CloudKit also batches per-participant errors. If the operation completes with errors, it returns a [`partialFailure`](/documentation/cloudkit/ckerror/partialfailure) error. The error stores the individual errors in its [`userInfo`](/documentation/Foundation/NSError/userInfo) dictionary. Use the [`CKPartialErrorsByItemIDKey`](/documentation/cloudkit/ckpartialerrorsbyitemidkey) key to extract them.

## [Topics](/documentation/cloudkit/ckfetchshareparticipantsoperation#topics)

### [Creating an Operation](/documentation/cloudkit/ckfetchshareparticipantsoperation#Creating-an-Operation)

[`init()`](/documentation/cloudkit/ckfetchshareparticipantsoperation/init\(\))

Creates an empty operation.

[`convenience init(userIdentityLookupInfos: [CKUserIdentity.LookupInfo])`](/documentation/cloudkit/ckfetchshareparticipantsoperation/init\(useridentitylookupinfos:\))

Creates an operation for generating share participants from the specified user data.

### [Configuring the Operation](/documentation/cloudkit/ckfetchshareparticipantsoperation#Configuring-the-Operation)

[`var userIdentityLookupInfos: [CKUserIdentity.LookupInfo]?`](/documentation/cloudkit/ckfetchshareparticipantsoperation/useridentitylookupinfos)

The user data for the participants.

### [Processing the Operation’s Results](/documentation/cloudkit/ckfetchshareparticipantsoperation#Processing-the-Operations-Results)

[`var shareParticipantFetchedBlock: ((CKShare.Participant) -> Void)?`](/documentation/cloudkit/ckfetchshareparticipantsoperation/shareparticipantfetchedblock)

The closure to execute as the operation generates individual participants.

Deprecated

[`var fetchShareParticipantsCompletionBlock: (((any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchshareparticipantsoperation/fetchshareparticipantscompletionblock)

The closure to execute when the operation finishes.

Deprecated

### [Instance Properties](/documentation/cloudkit/ckfetchshareparticipantsoperation#Instance-Properties)

[`var fetchShareParticipantsResultBlock: ((Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/ckfetchshareparticipantsoperation/fetchshareparticipantsresultblock)

[`var perShareParticipantResultBlock: ((CKUserIdentity.LookupInfo, Result<CKShare.Participant, any Error>) -> Void)?`](/documentation/cloudkit/ckfetchshareparticipantsoperation/pershareparticipantresultblock)

## [Relationships](/documentation/cloudkit/ckfetchshareparticipantsoperation#relationships)

### [Inherits From](/documentation/cloudkit/ckfetchshareparticipantsoperation#inherits-from)

-   [`CKOperation`](/documentation/cloudkit/ckoperation)

### [Conforms To](/documentation/cloudkit/ckfetchshareparticipantsoperation#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

# init(userIdentityLookupInfos:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchShareParticipantsOperation](/documentation/cloudkit/ckfetchshareparticipantsoperation)
-   init(userIdentityLookupInfos:)

Initializer

# init(userIdentityLookupInfos:)

Creates an operation for generating share participants from the specified user data.

```
convenience init(userIdentityLookupInfos: [CKUserIdentity.LookupInfo])
```

## [Parameters](/documentation/cloudkit/ckfetchshareparticipantsoperation/init\(useridentitylookupinfos:\)#parameters)

`userIdentityLookupInfos`

The user data for the participants. If you specify `nil`, you must assign a value to the [`userIdentityLookupInfos`](/documentation/cloudkit/ckfetchshareparticipantsoperation/useridentitylookupinfos) property before you execute this operation.

## [Discussion](/documentation/cloudkit/ckfetchshareparticipantsoperation/init\(useridentitylookupinfos:\)#Discussion)

After you create the operation, assign a handler to the [`fetchShareParticipantsCompletionBlock`](/documentation/cloudkit/ckfetchshareparticipantsoperation/fetchshareparticipantscompletionblock) property to process the results.

---
### Configuring the Operation

# userIdentityLookupInfos | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchShareParticipantsOperation](/documentation/cloudkit/ckfetchshareparticipantsoperation)
-   userIdentityLookupInfos

Instance Property

# userIdentityLookupInfos

The user data for the participants.

```
var userIdentityLookupInfos: [CKUserIdentity.LookupInfo]? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchshareparticipantsoperation/useridentitylookupinfos#Discussion)

Use this property to view or change the participants user data. If you intend to specify or change the value of this property, do so before you execute the operation or submit it to a queue.

Current page is userIdentityLookupInfos

  ---
  
# fetchShareParticipantsResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchShareParticipantsOperation](/documentation/cloudkit/ckfetchshareparticipantsoperation)
-   fetchShareParticipantsResultBlock

Instance Property

# fetchShareParticipantsResultBlock

```
var fetchShareParticipantsResultBlock: ((Result<Void, any Error>) -> Void)? { get set }
```

Current page is fetchShareParticipantsResultBlock

# perShareParticipantResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchShareParticipantsOperation](/documentation/cloudkit/ckfetchshareparticipantsoperation)
-   perShareParticipantResultBlock

Instance Property

# perShareParticipantResultBlock

```
var perShareParticipantResultBlock: ((CKUserIdentity.LookupInfo, Result<CKShare.Participant, any Error>) -> Void)? { get set }
```

Current page is perShareParticipantResultBlock
