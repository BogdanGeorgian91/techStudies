# CKModifySubscriptionsOperation | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKModifySubscriptionsOperation

Class

# CKModifySubscriptionsOperation

An operation for modifying one or more subscriptions.

```
class CKModifySubscriptionsOperation
```

## [Overview](/documentation/cloudkit/ckmodifysubscriptionsoperation#overview)

After you create or change the configuration of a subscription, use this operation to save those changes to the server. You can also use this operation to permanently delete subscriptions.

If you assign a handler to the [`completionBlock`](/documentation/Foundation/Operation/completionBlock) property, the operation calls it after it executes and passes it the results. Use the handler to perform any housekeeping tasks for the operation. The handler you specify should manage any failures, whether due to an error or an explicit cancellation.

## [Topics](/documentation/cloudkit/ckmodifysubscriptionsoperation#topics)

### [Creating a Modify Subscriptions Operation](/documentation/cloudkit/ckmodifysubscriptionsoperation#Creating-a-Modify-Subscriptions-Operation)

[`convenience init(subscriptionsToSave: [CKSubscription]?, subscriptionIDsToDelete: [CKSubscription.ID]?)`](/documentation/cloudkit/ckmodifysubscriptionsoperation/init\(subscriptionstosave:subscriptionidstodelete:\))

Creates an operation for saving and deleting the specified subscriptions.

[`init()`](/documentation/cloudkit/ckmodifysubscriptionsoperation/init\(\))

Creates an empty modify subscriptions operation.

### [Configuring the Modify Subscriptions Operation](/documentation/cloudkit/ckmodifysubscriptionsoperation#Configuring-the-Modify-Subscriptions-Operation)

[`var subscriptionsToSave: [CKSubscription]?`](/documentation/cloudkit/ckmodifysubscriptionsoperation/subscriptionstosave)

The subscriptions to save to the database.

[`var subscriptionIDsToDelete: [CKSubscription.ID]?`](/documentation/cloudkit/ckmodifysubscriptionsoperation/subscriptionidstodelete-3534e)

The IDs of the subscriptions that you want to delete.

### [Processing the Modify Subscription Results](/documentation/cloudkit/ckmodifysubscriptionsoperation#Processing-the-Modify-Subscription-Results)

[`var modifySubscriptionsCompletionBlock: (([CKSubscription]?, [CKSubscription.ID]?, (any Error)?) -> Void)?`](/documentation/cloudkit/ckmodifysubscriptionsoperation/modifysubscriptionscompletionblock-7l56)

The closure to execute after the operation modifies the subscriptions.

Deprecated

### [Instance Properties](/documentation/cloudkit/ckmodifysubscriptionsoperation#Instance-Properties)

[`var modifySubscriptionsResultBlock: ((Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/ckmodifysubscriptionsoperation/modifysubscriptionsresultblock)

[`var perSubscriptionDeleteBlock: ((CKSubscription.ID, Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/ckmodifysubscriptionsoperation/persubscriptiondeleteblock-5ke2l)

[`var perSubscriptionSaveBlock: ((CKSubscription.ID, Result<CKSubscription, any Error>) -> Void)?`](/documentation/cloudkit/ckmodifysubscriptionsoperation/persubscriptionsaveblock-8y9zn)

## [Relationships](/documentation/cloudkit/ckmodifysubscriptionsoperation#relationships)

### [Inherits From](/documentation/cloudkit/ckmodifysubscriptionsoperation#inherits-from)

-   [`CKDatabaseOperation`](/documentation/cloudkit/ckdatabaseoperation)

### [Conforms To](/documentation/cloudkit/ckmodifysubscriptionsoperation#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/ckmodifysubscriptionsoperation#see-also)

### [Subscription Management](/documentation/cloudkit/ckmodifysubscriptionsoperation#Subscription-Management)

[`class CKFetchSubscriptionsOperation`](/documentation/cloudkit/ckfetchsubscriptionsoperation)

An operation for fetching subscriptions.

Current page is CKModifySubscriptionsOperation

---
### Creating a Modify Subscriptions Operation

# init(subscriptionsToSave:subscriptionIDsToDelete:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifySubscriptionsOperation](/documentation/cloudkit/ckmodifysubscriptionsoperation)
-   init(subscriptionsToSave:subscriptionIDsToDelete:)

Initializer

# init(subscriptionsToSave:subscriptionIDsToDelete:)

Creates an operation for saving and deleting the specified subscriptions.

```
convenience init(
    subscriptionsToSave: [CKSubscription]? = nil,
    subscriptionIDsToDelete: [CKSubscription.ID]? = nil
)
```

## [Parameters](/documentation/cloudkit/ckmodifysubscriptionsoperation/init\(subscriptionstosave:subscriptionidstodelete:\)#parameters)

`subscriptionsToSave`

The subscriptions to save or update. You can specify `nil` for this parameter.

`subscriptionIDsToDelete`

The IDs of the subscriptions to delete. You can specify `nil` for this parameter.

## [Discussion](/documentation/cloudkit/ckmodifysubscriptionsoperation/init\(subscriptionstosave:subscriptionidstodelete:\)#Discussion)

The subscriptions that you want to save or delete must reside in the same container. CloudKit creates a subscription if you save one that doesn’t already exist. CloudKit returns an error if you try to delete a subscription that doesn’t exist.

## [See Also](/documentation/cloudkit/ckmodifysubscriptionsoperation/init\(subscriptionstosave:subscriptionidstodelete:\)#see-also)

### [Creating a Modify Subscriptions Operation](/documentation/cloudkit/ckmodifysubscriptionsoperation/init\(subscriptionstosave:subscriptionidstodelete:\)#Creating-a-Modify-Subscriptions-Operation)

[`init()`](/documentation/cloudkit/ckmodifysubscriptionsoperation/init\(\))

Creates an empty modify subscriptions operation.

Current page is init(subscriptionsToSave:subscriptionIDsToDelete:)

# init() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifySubscriptionsOperation](/documentation/cloudkit/ckmodifysubscriptionsoperation)
-   init()

Initializer

# init()

Creates an empty modify subscriptions operation.

```
init()
```

## [See Also](/documentation/cloudkit/ckmodifysubscriptionsoperation/init\(\)#see-also)

### [Creating a Modify Subscriptions Operation](/documentation/cloudkit/ckmodifysubscriptionsoperation/init\(\)#Creating-a-Modify-Subscriptions-Operation)

[`convenience init(subscriptionsToSave: [CKSubscription]?, subscriptionIDsToDelete: [CKSubscription.ID]?)`](/documentation/cloudkit/ckmodifysubscriptionsoperation/init\(subscriptionstosave:subscriptionidstodelete:\))

Creates an operation for saving and deleting the specified subscriptions.

Current page is init()

---
  
Configuring the Modify Subscriptions Operation

# subscriptionsToSave | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifySubscriptionsOperation](/documentation/cloudkit/ckmodifysubscriptionsoperation)
-   subscriptionsToSave

Instance Property

# subscriptionsToSave

The subscriptions to save to the database.

```
var subscriptionsToSave: [CKSubscription]? { get set }
```

## [Discussion](/documentation/cloudkit/ckmodifysubscriptionsoperation/subscriptionstosave#Discussion)

This property contains the subscriptions that you want to save. Its initial value is the array that you pass to the [`init(subscriptionsToSave:subscriptionIDsToDelete:)`](/documentation/cloudkit/ckmodifysubscriptionsoperation/init\(subscriptionstosave:subscriptionidstodelete:\)) method. Modify this property as necessary before you execute the operation or submit it to a queue. After CloudKit saves the subscriptions, it begins generating push notifications according to their criteria.

## [See Also](/documentation/cloudkit/ckmodifysubscriptionsoperation/subscriptionstosave#see-also)

### [Configuring the Modify Subscriptions Operation](/documentation/cloudkit/ckmodifysubscriptionsoperation/subscriptionstosave#Configuring-the-Modify-Subscriptions-Operation)

[`var subscriptionIDsToDelete: [CKSubscription.ID]?`](/documentation/cloudkit/ckmodifysubscriptionsoperation/subscriptionidstodelete-3534e)

The IDs of the subscriptions that you want to delete.

Current page is subscriptionsToSave

# subscriptionIDsToDelete | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifySubscriptionsOperation](/documentation/cloudkit/ckmodifysubscriptionsoperation)
-   subscriptionIDsToDelete

Instance Property

# subscriptionIDsToDelete

The IDs of the subscriptions that you want to delete.

```
var subscriptionIDsToDelete: [CKSubscription.ID]? { get set }
```

## [Discussion](/documentation/cloudkit/ckmodifysubscriptionsoperation/subscriptionidstodelete-3534e#Discussion)

This property contains the IDs of the subscriptions that you want to delete. Its initial value is the array that you pass to the [`init(subscriptionsToSave:subscriptionIDsToDelete:)`](/documentation/cloudkit/ckmodifysubscriptionsoperation/init\(subscriptionstosave:subscriptionidstodelete:\)) method. Modify this property as necessary before you execute the operation or submit it to a queue.

## [See Also](/documentation/cloudkit/ckmodifysubscriptionsoperation/subscriptionidstodelete-3534e#see-also)

### [Configuring the Modify Subscriptions Operation](/documentation/cloudkit/ckmodifysubscriptionsoperation/subscriptionidstodelete-3534e#Configuring-the-Modify-Subscriptions-Operation)

[`var subscriptionsToSave: [CKSubscription]?`](/documentation/cloudkit/ckmodifysubscriptionsoperation/subscriptionstosave)

The subscriptions to save to the database.

Current page is subscriptionIDsToDelete

---
### Processing the Modify Subscription Results

# modifySubscriptionsCompletionBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifySubscriptionsOperation](/documentation/cloudkit/ckmodifysubscriptionsoperation)
-   modifySubscriptionsCompletionBlock Deprecated

Instance Property

# modifySubscriptionsCompletionBlock

The closure to execute after the operation modifies the subscriptions.

```
var modifySubscriptionsCompletionBlock: (([CKSubscription]?, [CKSubscription.ID]?, (any Error)?) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/ckmodifysubscriptionsoperation/modifysubscriptionscompletionblock-7l56#Discussion)

The closure returns no value and takes the following parameters:

-   The subscriptions to save.
    
-   The IDs of the subscriptions to delete.
    
-   An error that contains information about a problem, or `nil` if CloudKit successfully modifies the subscriptions.
    

The operation executes this closure only once, and it’s your only opportunity to process the results. The closure executes on a background queue, so any tasks that require access to the main queue must dispatch accordingly.

The closure reports an error of type [`CKError.Code.partialFailure`](/documentation/cloudkit/ckerror/code/partialfailure) when it can’t modify some of the subscriptions. The [`userInfo`](/documentation/Foundation/NSError/userInfo) dictionary of the error contains a [`CKPartialErrorsByItemIDKey`](/documentation/cloudkit/ckpartialerrorsbyitemidkey) key that has a dictionary as its value. The keys of the dictionary are the IDs of the subscriptions that CloudKit can’t modify, and the corresponding values are errors that contain information about the failures.

Set this property’s value before you execute the operation or submit it to a queue.

Current page is modifySubscriptionsCompletionBlock

---
instance properties
# modifySubscriptionsResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifySubscriptionsOperation](/documentation/cloudkit/ckmodifysubscriptionsoperation)
-   modifySubscriptionsResultBlock

Instance Property

# modifySubscriptionsResultBlock

```
var modifySubscriptionsResultBlock: ((Result<Void, any Error>) -> Void)? { get set }
```

Current page is modifySubscriptionsResultBlock

# perSubscriptionDeleteBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifySubscriptionsOperation](/documentation/cloudkit/ckmodifysubscriptionsoperation)
-   perSubscriptionDeleteBlock

Instance Property

# perSubscriptionDeleteBlock

```
var perSubscriptionDeleteBlock: ((CKSubscription.ID, Result<Void, any Error>) -> Void)? { get set }
```

Current page is perSubscriptionDeleteBlock

# perSubscriptionSaveBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKModifySubscriptionsOperation](/documentation/cloudkit/ckmodifysubscriptionsoperation)
-   perSubscriptionSaveBlock

Instance Property

# perSubscriptionSaveBlock

```
var perSubscriptionSaveBlock: ((CKSubscription.ID, Result<CKSubscription, any Error>) -> Void)? { get set }
```

Current page is perSubscriptionSaveBlock



