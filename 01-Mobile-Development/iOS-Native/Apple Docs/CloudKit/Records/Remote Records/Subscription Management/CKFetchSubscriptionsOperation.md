# CKFetchSubscriptionsOperation | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKFetchSubscriptionsOperation

Class

# CKFetchSubscriptionsOperation

An operation for fetching subscriptions.

```
class CKFetchSubscriptionsOperation
```

## [Overview](/documentation/cloudkit/ckfetchsubscriptionsoperation#overview)

A fetch subscriptions operation retrieves subscriptions (with IDs you already know) from iCloud and can fetch all subscriptions for the current user.

You might fetch subscriptions so you can examine or modify their parameters — for example, to adjust the delivery options for push notifications that the subscription generates.

If you assign a handler to the [`completionBlock`](/documentation/Foundation/Operation/completionBlock) property, the operation calls it after it executes and passes it the results. Use the handler to perform any housekeeping tasks for the operation. The handler you specify should manage any failures, whether due to an error or an explicit cancellation.

## [Topics](/documentation/cloudkit/ckfetchsubscriptionsoperation#topics)

### [Creating a Fetch Subscriptions Operation](/documentation/cloudkit/ckfetchsubscriptionsoperation#Creating-a-Fetch-Subscriptions-Operation)

[`convenience init(subscriptionIDs: [CKSubscription.ID])`](/documentation/cloudkit/ckfetchsubscriptionsoperation/init\(subscriptionids:\))

Creates an operation for fetching the specified subscriptions.

[`init()`](/documentation/cloudkit/ckfetchsubscriptionsoperation/init\(\))

Creates an empty fetch subscriptions operation.

### [Getting All Subscriptions](/documentation/cloudkit/ckfetchsubscriptionsoperation#Getting-All-Subscriptions)

[`class func fetchAllSubscriptionsOperation() -> Self`](/documentation/cloudkit/ckfetchsubscriptionsoperation/fetchallsubscriptionsoperation\(\))

Returns an operation that fetches all of the user’s subscriptions.

### [Configuring the Fetch Subscriptions Operation](/documentation/cloudkit/ckfetchsubscriptionsoperation#Configuring-the-Fetch-Subscriptions-Operation)

[`var subscriptionIDs: [CKSubscription.ID]?`](/documentation/cloudkit/ckfetchsubscriptionsoperation/subscriptionids-17f4q)

The IDs of the subscriptions to fetch.

### [Processing the Fetch Subscription Results](/documentation/cloudkit/ckfetchsubscriptionsoperation#Processing-the-Fetch-Subscription-Results)

[`var fetchSubscriptionCompletionBlock: (([CKSubscription.ID : CKSubscription]?, (any Error)?) -> Void)?`](/documentation/cloudkit/ckfetchsubscriptionsoperation/fetchsubscriptioncompletionblock-6hhpi)

The block to execute with the fetch results.

### [Instance Properties](/documentation/cloudkit/ckfetchsubscriptionsoperation#Instance-Properties)

[`var fetchSubscriptionsResultBlock: ((Result<Void, any Error>) -> Void)?`](/documentation/cloudkit/ckfetchsubscriptionsoperation/fetchsubscriptionsresultblock)

[`var perSubscriptionResultBlock: ((CKSubscription.ID, Result<CKSubscription, any Error>) -> Void)?`](/documentation/cloudkit/ckfetchsubscriptionsoperation/persubscriptionresultblock)

## [Relationships](/documentation/cloudkit/ckfetchsubscriptionsoperation#relationships)

### [Inherits From](/documentation/cloudkit/ckfetchsubscriptionsoperation#inherits-from)

-   [`CKDatabaseOperation`](/documentation/cloudkit/ckdatabaseoperation)

### [Conforms To](/documentation/cloudkit/ckfetchsubscriptionsoperation#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/ckfetchsubscriptionsoperation#see-also)

### [Subscription Management](/documentation/cloudkit/ckfetchsubscriptionsoperation#Subscription-Management)

[`class CKModifySubscriptionsOperation`](/documentation/cloudkit/ckmodifysubscriptionsoperation)

An operation for modifying one or more subscriptions.

Current page is CKFetchSubscriptionsOperation

---
  
Creating a Fetch Subscriptions Operation

# init(subscriptionIDs:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchSubscriptionsOperation](/documentation/cloudkit/ckfetchsubscriptionsoperation)
-   init(subscriptionIDs:)

Initializer

# init(subscriptionIDs:)

Creates an operation for fetching the specified subscriptions.

```
convenience init(subscriptionIDs: [CKSubscription.ID])
```

## [Parameters](/documentation/cloudkit/ckfetchsubscriptionsoperation/init\(subscriptionids:\)#parameters)

`subscriptionIDs`

An array of strings where each one is an ID of a subscription that you want to retrieve. This parameter sets the [`subscriptionIDs`](/documentation/cloudkit/ckfetchsubscriptionsoperation/subscriptionids-714ct) property’s value. If you specify `nil`, you must set the `subscriptionIDs` property before you execute the operation.

## [Discussion](/documentation/cloudkit/ckfetchsubscriptionsoperation/init\(subscriptionids:\)#Discussion)

After creating the operation, assign a closure to the [`fetchSubscriptionCompletionBlock`](/documentation/cloudkit/ckfetchsubscriptionsoperation/fetchsubscriptioncompletionblock-207ep) property to process the results.

## [See Also](/documentation/cloudkit/ckfetchsubscriptionsoperation/init\(subscriptionids:\)#see-also)

### [Creating a Fetch Subscriptions Operation](/documentation/cloudkit/ckfetchsubscriptionsoperation/init\(subscriptionids:\)#Creating-a-Fetch-Subscriptions-Operation)

[`init()`](/documentation/cloudkit/ckfetchsubscriptionsoperation/init\(\))

Creates an empty fetch subscriptions operation.

Current page is init(subscriptionIDs:)

# init() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchSubscriptionsOperation](/documentation/cloudkit/ckfetchsubscriptionsoperation)
-   init()

Initializer

# init()

Creates an empty fetch subscriptions operation.

```
init()
```

## [Discussion](/documentation/cloudkit/ckfetchsubscriptionsoperation/init\(\)#Discussion)

You must set the [`subscriptionIDs`](/documentation/cloudkit/ckfetchsubscriptionsoperation/subscriptionids-714ct) property before you execute the operation.

## [See Also](/documentation/cloudkit/ckfetchsubscriptionsoperation/init\(\)#see-also)

### [Creating a Fetch Subscriptions Operation](/documentation/cloudkit/ckfetchsubscriptionsoperation/init\(\)#Creating-a-Fetch-Subscriptions-Operation)

[`convenience init(subscriptionIDs: [CKSubscription.ID])`](/documentation/cloudkit/ckfetchsubscriptionsoperation/init\(subscriptionids:\))

Creates an operation for fetching the specified subscriptions.

Current page is init()

---
  
Getting All Subscriptions

# fetchAllSubscriptionsOperation() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchSubscriptionsOperation](/documentation/cloudkit/ckfetchsubscriptionsoperation)
-   fetchAllSubscriptionsOperation()

Type Method

# fetchAllSubscriptionsOperation()

Returns an operation that fetches all of the user’s subscriptions.

```
class func fetchAllSubscriptionsOperation() -> Self
```

## [Discussion](/documentation/cloudkit/ckfetchsubscriptionsoperation/fetchallsubscriptionsoperation\(\)#Discussion)

After creating the operation, set the [`fetchSubscriptionCompletionBlock`](/documentation/cloudkit/ckfetchsubscriptionsoperation/fetchsubscriptioncompletionblock-207ep) property to process the results.

Current page is fetchAllSubscriptionsOperation()

---
  
Configuring the Fetch Subscriptions Operation

# subscriptionIDs | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchSubscriptionsOperation](/documentation/cloudkit/ckfetchsubscriptionsoperation)
-   subscriptionIDs

Instance Property

# subscriptionIDs

The IDs of the subscriptions to fetch.

```
var subscriptionIDs: [CKSubscription.ID]? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchsubscriptionsoperation/subscriptionids-17f4q#Discussion)

Use this property to view or change the IDs of the subscriptions to fetch. Each element of the array is a string that represents the ID of a subscription. If you intend to modify this property’s value, do so before you execute the operation or submit it to a queue.

If you use the [`fetchAllSubscriptionsOperation()`](/documentation/cloudkit/ckfetchsubscriptionsoperation/fetchallsubscriptionsoperation\(\)) method to create the operation, CloudKit ignores this property’s value and sets it to `nil`.

Current page is subscriptionIDs

---
  
Processing the Fetch Subscription Results

# fetchSubscriptionCompletionBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchSubscriptionsOperation](/documentation/cloudkit/ckfetchsubscriptionsoperation)
-   fetchSubscriptionCompletionBlock

Instance Property

# fetchSubscriptionCompletionBlock

The block to execute with the fetch results.

```
var fetchSubscriptionCompletionBlock: (([CKSubscription.ID : CKSubscription]?, (any Error)?) -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/ckfetchsubscriptionsoperation/fetchsubscriptioncompletionblock-6hhpi#Discussion)

The closure returns no value and takes the following parameters:

-   A dictionary with keys that are the IDs of the subscriptions you request, and values that are the corresponding subscriptions.
    
-   An error that contains information about a problem, or `nil` if the system successfully fetches the subscriptions.
    

The operation executes this closure only once, and it’s your only opportunity to process the results. The closure must be capable of executing on a background queue, so any tasks that require access to the main queue must redirect accordingly.

The closure reports an error of type [`CKError.Code.partialFailure`](/documentation/cloudkit/ckerror/code/partialfailure) when it retrieves only some of the subscriptions successfully. The [`userInfo`](/documentation/Foundation/NSError/userInfo) dictionary of the error contains a [`CKPartialErrorsByItemIDKey`](/documentation/cloudkit/ckpartialerrorsbyitemidkey) key that has a dictionary as its value. The keys of the dictionary are the IDs of the subscriptions that the operation can’t fetch, and the corresponding values are errors that contain information about the failures.

Set this property’s value before you execute the operation or submit it to a queue.

Current page is fetchSubscriptionCompletionBlock

---
instance properties

# fetchSubscriptionsResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchSubscriptionsOperation](/documentation/cloudkit/ckfetchsubscriptionsoperation)
-   fetchSubscriptionsResultBlock

Instance Property

# fetchSubscriptionsResultBlock

```
var fetchSubscriptionsResultBlock: ((Result<Void, any Error>) -> Void)? { get set }
```

Current page is fetchSubscriptionsResultBlock

# perSubscriptionResultBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKFetchSubscriptionsOperation](/documentation/cloudkit/ckfetchsubscriptionsoperation)
-   perSubscriptionResultBlock

Instance Property

# perSubscriptionResultBlock

```
var perSubscriptionResultBlock: ((CKSubscription.ID, Result<CKSubscription, any Error>) -> Void)? { get set }
```

Current page is perSubscriptionResultBlock