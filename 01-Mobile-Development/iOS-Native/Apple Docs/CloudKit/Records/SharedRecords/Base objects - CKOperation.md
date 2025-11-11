# CKOperation | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKOperation

Class

# CKOperation

The abstract base class for all operations that execute in a database.

```
class CKOperation
```

## [Mentioned in](/documentation/cloudkit/ckoperation#mentions)

[

Encrypting User Data](/documentation/cloudkit/encrypting-user-data)

[

Deciding whether CloudKit is right for your app](/documentation/cloudkit/deciding-whether-cloudkit-is-right-for-your-app)

## [Overview](/documentation/cloudkit/ckoperation#overview)

All CloudKit operations descend from `CKOperation`, which provides the infrastructure for executing tasks in one of your app’s containers. Don’t subclass or create instances of this class directly. Instead, create instances of one of its concrete subclasses.

Use the properties of this class to configure the behavior of the operation before submitting it to a queue or executing it directly. CloudKit operations involve communicating with the iCloud servers to send and receive data. You can use the properties of this class to configure the behavior of those network requests to ensure the best performance for your app.

### [Long-Lived Operations](/documentation/cloudkit/ckoperation#Long-Lived-Operations)

A _long-lived operation_ is an operation that continues to run after the user closes the app. To specify a long-lived operation, set [`isLongLived`](/documentation/cloudkit/ckoperation/islonglived) to [`true`](/documentation/swift/true), provide a completion handler, and execute the operation. To get the identifiers of all running long-lived operations, use the [`fetchAllLongLivedOperationIDsWithCompletionHandler:`](/documentation/cloudkit/ckcontainer/fetchalllonglivedoperationidswithcompletionhandler:) method that [`CKContainer`](/documentation/cloudkit/ckcontainer) provides. To get a specific long-lived operation, use the [`fetchLongLivedOperationWithID:completionHandler:`](/documentation/cloudkit/ckcontainer/fetchlonglivedoperationwithid:completionhandler:) method. Make sure you set the completion handler of a long-lived operation before you execute it so that the system can notify you when it completes and you can process the results. Do not execute an operation, change it to long-lived, and execute it again as a long-lived operation.

```
container.fetchAllLongLivedOperationIDs(completionHandler: { (operationIDs, error) in
    if let error = error {
        print("Error fetching long lived operations: \(error)")
        // Handle error
        return
    }
    guard let identifiers = operationIDs else { return }
    for operationID in identifiers {
        container.fetchLongLivedOperation(withID: operationID, completionHandler: { (operation, error) in
            if let error = error {
                print("Error fetching operation: \(operationID)\n\(error)")
                // Handle error
                return
            }
            guard let operation = operation else { return }
            // Add callback handlers to operation
            container.add(operation)
        })
    }
})
```

```
[container fetchAllLongLivedOperationIDsWithCompletionHandler:^(NSArray<NSString *> *_Nullable operationIDs, NSError *_Nullable error) {
    if (error) {
        // Handle error
        return
    }
    for (NSString *operationID in operationIDs) {
        [container fetchLongLivedOperationWithID:operationID completionHandler:^(CKOperation *_Nullable operation, NSError *_Nullable error) {
            if (error) {
                // Handle error
                return
            }
            // Add callback handlers to operation
            [container addOperation:operation];
        }];
    }
}];
```

The following is the typical life cycle of a long-lived operation:

1.  The app creates a long-lived operation and executes it.
    

The daemon starts saving and sending the callbacks to the running app. 2. The app exits.

The daemon continues running the long-lived operation and saves the callbacks. 3. The app launches and fetches the long-lived operation.

If the operation is running or if it completed within the previous 24 hours, the daemon returns a proxy for the long-lived operation. If the operation completed more than 24 hours previously, the daemon may stop returning it in fetch requests. 4. The app runs the long-lived operation again.

The daemon sends the app all the saved callbacks (it doesn’t actually rerun the operation), and continues saving the callbacks and sending them to the running app. 5. The app receives the completion callback or the app cancels the operation.

The daemon stops including the operation in future fetch results.

## [Topics](/documentation/cloudkit/ckoperation#topics)

### [Creating an Operation](/documentation/cloudkit/ckoperation#Creating-an-Operation)

[`init()`](/documentation/cloudkit/ckoperation/init\(\))

Creates an operation.

### [Identifying the Operation](/documentation/cloudkit/ckoperation#Identifying-the-Operation)

[`var operationID: CKOperation.ID`](/documentation/cloudkit/ckoperation/operationid-8auuc)

A unique identifier for a long-lived operation.

[`typealias ID`](/documentation/cloudkit/ckoperation/id)

A type that represents the ID of an operation.

### [Managing the Operation’s Configuration](/documentation/cloudkit/ckoperation#Managing-the-Operations-Configuration)

[`var configuration: CKOperation.Configuration!`](/documentation/cloudkit/ckoperation/configuration-swift.property)

The operation’s configuration.

[`class Configuration`](/documentation/cloudkit/ckoperation/configuration-swift.class)

An object that describes how a CloudKit operation behaves.

[`var group: CKOperationGroup?`](/documentation/cloudkit/ckoperation/group)

The operation’s group.

[`var longLivedOperationWasPersistedBlock: (() -> Void)?`](/documentation/cloudkit/ckoperation/longlivedoperationwaspersistedblock)

The closure to execute when the server begins to store callbacks for the long-lived operation.

### [Deprecated](/documentation/cloudkit/ckoperation#Deprecated)

[

API Reference

Deprecated Symbols](/documentation/cloudkit/ckoperation-deprecated-symbols)

Review unsupported symbols and their replacements.

## [Relationships](/documentation/cloudkit/ckoperation#relationships)

### [Inherits From](/documentation/cloudkit/ckoperation#inherits-from)

-   [`Operation`](/documentation/Foundation/Operation)

### [Inherited By](/documentation/cloudkit/ckoperation#inherited-by)

-   [`CKAcceptSharesOperation`](/documentation/cloudkit/ckacceptsharesoperation)
-   [`CKDatabaseOperation`](/documentation/cloudkit/ckdatabaseoperation)
-   [`CKDiscoverAllUserIdentitiesOperation`](/documentation/cloudkit/ckdiscoveralluseridentitiesoperation)
-   [`CKDiscoverUserIdentitiesOperation`](/documentation/cloudkit/ckdiscoveruseridentitiesoperation)
-   [`CKFetchShareMetadataOperation`](/documentation/cloudkit/ckfetchsharemetadataoperation)
-   [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation)
-   [`CKShareRequestAccessOperation`](/documentation/cloudkit/cksharerequestaccessoperation)

### [Conforms To](/documentation/cloudkit/ckoperation#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

Current page is CKOperation

---
  
Identifying the Operation

# operationID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperation](/documentation/cloudkit/ckoperation)
-   operationID

Instance Property

# operationID

A unique identifier for a long-lived operation.

```
var operationID: CKOperation.ID { get }
```

## [Discussion](/documentation/cloudkit/ckoperation/operationid-8auuc#Discussion)

Pass this property’s value to the [`fetchLongLivedOperationWithID:completionHandler:`](/documentation/cloudkit/ckcontainer/fetchlonglivedoperationwithid:completionhandler:) method to fetch the corresponding long-lived operation. For more information, see [Long-Lived Operations](/documentation/cloudkit/ckoperation#Long-Lived-Operations).

## [See Also](/documentation/cloudkit/ckoperation/operationid-8auuc#see-also)

### [Identifying the Operation](/documentation/cloudkit/ckoperation/operationid-8auuc#Identifying-the-Operation)

[`typealias ID`](/documentation/cloudkit/ckoperation/id)

A type that represents the ID of an operation.

Current page is operationID

# CKOperation.ID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperation](/documentation/cloudkit/ckoperation)
-   CKOperation.ID

Type Alias

# CKOperation.ID

A type that represents the ID of an operation.

```
typealias ID = String
```

## [See Also](/documentation/cloudkit/ckoperation/id#see-also)

### [Identifying the Operation](/documentation/cloudkit/ckoperation/id#Identifying-the-Operation)

[`var operationID: CKOperation.ID`](/documentation/cloudkit/ckoperation/operationid-8auuc)

A unique identifier for a long-lived operation.

Current page is CKOperation.ID

---
### Managing the Operation’s Configuration

# configuration | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperation](/documentation/cloudkit/ckoperation)
-   configuration

Instance Property

# configuration

The operation’s configuration.

```
@NSCopying
var configuration: CKOperation.Configuration! { get set }
```

## [See Also](/documentation/cloudkit/ckoperation/configuration-swift.property#see-also)

### [Managing the Operation’s Configuration](/documentation/cloudkit/ckoperation/configuration-swift.property#Managing-the-Operations-Configuration)

[`class Configuration`](/documentation/cloudkit/ckoperation/configuration-swift.class)

An object that describes how a CloudKit operation behaves.

[`var group: CKOperationGroup?`](/documentation/cloudkit/ckoperation/group)

The operation’s group.

[`var longLivedOperationWasPersistedBlock: (() -> Void)?`](/documentation/cloudkit/ckoperation/longlivedoperationwaspersistedblock)

The closure to execute when the server begins to store callbacks for the long-lived operation.

Current page is configuration

# CKOperation.Configuration | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperation](/documentation/cloudkit/ckoperation)
-   CKOperation.Configuration

Class

# CKOperation.Configuration

An object that describes how a CloudKit operation behaves.

```
class Configuration
```

## [Overview](/documentation/cloudkit/ckoperation/configuration-swift.class#overview)

All of the properties in `CKOperationConfiguration` have a default value. When determining which properties to apply to a CloudKit operation, consult the operation’s configuration property, as well as the [`defaultConfiguration`](/documentation/cloudkit/ckoperationgroup/defaultconfiguration) property of the group that the operation belongs to. These properties combine through the following rules:

Group default configuration value

Operation configuration value

Value applied to operation

default value

default value

default value

default value

explicit value

operation.configuration explicit value

explicit value

default value

group.defaultConfiguration explicit value

explicit value

explicit value

operation.configuration explicit value

## [Topics](/documentation/cloudkit/ckoperation/configuration-swift.class#topics)

### [Preparing a Configuration](/documentation/cloudkit/ckoperation/configuration-swift.class#Preparing-a-Configuration)

[`var allowsCellularAccess: Bool`](/documentation/cloudkit/ckoperation/configuration-swift.class/allowscellularaccess)

A Boolean value that indicates whether operations that use this configuration can send data over the cellular network.

[`var container: CKContainer?`](/documentation/cloudkit/ckoperation/configuration-swift.class/container)

The configuration’s container.

[`var isLongLived: Bool`](/documentation/cloudkit/ckoperation/configuration-swift.class/islonglived)

A Boolean value that indicates whether the operations that use this configuration are long-lived.

[`var qualityOfService: QualityOfService`](/documentation/cloudkit/ckoperation/configuration-swift.class/qualityofservice)

The priority that the system uses when it allocates resources to the operations that use this configuration.

[`var timeoutIntervalForRequest: TimeInterval`](/documentation/cloudkit/ckoperation/configuration-swift.class/timeoutintervalforrequest)

The maximum amount of time that a request can take.

[`var timeoutIntervalForResource: TimeInterval`](/documentation/cloudkit/ckoperation/configuration-swift.class/timeoutintervalforresource)

The maximum amount of time that a resource request can take.

## [Relationships](/documentation/cloudkit/ckoperation/configuration-swift.class#relationships)

### [Inherits From](/documentation/cloudkit/ckoperation/configuration-swift.class#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckoperation/configuration-swift.class#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/cloudkit/ckoperation/configuration-swift.class#see-also)

### [Managing the Operation’s Configuration](/documentation/cloudkit/ckoperation/configuration-swift.class#Managing-the-Operations-Configuration)

[`var configuration: CKOperation.Configuration!`](/documentation/cloudkit/ckoperation/configuration-swift.property)

The operation’s configuration.

[`var group: CKOperationGroup?`](/documentation/cloudkit/ckoperation/group)

The operation’s group.

[`var longLivedOperationWasPersistedBlock: (() -> Void)?`](/documentation/cloudkit/ckoperation/longlivedoperationwaspersistedblock)

The closure to execute when the server begins to store callbacks for the long-lived operation.

Current page is CKOperation.Configuration

  
Preparing a Configuration

# allowsCellularAccess | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperation](/documentation/cloudkit/ckoperation)
-   [CKOperation.Configuration](/documentation/cloudkit/ckoperation/configuration-swift.class)
-   allowsCellularAccess

Instance Property

# allowsCellularAccess

A Boolean value that indicates whether operations that use this configuration can send data over the cellular network.

```
var allowsCellularAccess: Bool { get set }
```

# container

The configuration’s container.
var container: [`CKContainer`](https://developer.apple.com/documentation/cloudkit/ckcontainer)? { get set }


# isLongLived

A Boolean value that indicates whether the operations that use this configuration are long-lived.
var isLongLived: [`Bool`](https://developer.apple.com/documentation/Swift/Bool) { get set }
# qualityOfService

The priority that the system uses when it allocates resources to the operations that use this configuration.

var qualityOfService: [`QualityOfService`](https://developer.apple.com/documentation/Foundation/QualityOfService) { get set }

# timeoutIntervalForRequest

The maximum amount of time that a request can take.

```
var timeoutIntervalForRequest: TimeInterval { get set }
```

# timeoutIntervalForResource

The maximum amount of time that a resource request can take.

```
var timeoutIntervalForResource: TimeInterval { get set }
```

# group | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperation](/documentation/cloudkit/ckoperation)
-   group

Instance Property

# group

The operation’s group.

```
var group: CKOperationGroup? { get set }
```

## [See Also](/documentation/cloudkit/ckoperation/group#see-also)

### [Managing the Operation’s Configuration](/documentation/cloudkit/ckoperation/group#Managing-the-Operations-Configuration)

[`var configuration: CKOperation.Configuration!`](/documentation/cloudkit/ckoperation/configuration-swift.property)

The operation’s configuration.

[`class Configuration`](/documentation/cloudkit/ckoperation/configuration-swift.class)

An object that describes how a CloudKit operation behaves.

[`var longLivedOperationWasPersistedBlock: (() -> Void)?`](/documentation/cloudkit/ckoperation/longlivedoperationwaspersistedblock)

The closure to execute when the server begins to store callbacks for the long-lived operation.

Current page is group

# longLivedOperationWasPersistedBlock | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperation](/documentation/cloudkit/ckoperation)
-   longLivedOperationWasPersistedBlock

Instance Property

# longLivedOperationWasPersistedBlock

The closure to execute when the server begins to store callbacks for the long-lived operation.

```
var longLivedOperationWasPersistedBlock: (() -> Void)? { get set }
```

## [Discussion](/documentation/cloudkit/ckoperation/longlivedoperationwaspersistedblock#Discussion)

If your app exits before CloudKit calls this property’s value, the system doesn’t include the operation’s ID in the results of calls to the [`fetchAllLongLivedOperationIDsWithCompletionHandler:`](/documentation/cloudkit/ckcontainer/fetchalllonglivedoperationidswithcompletionhandler:) method.

For more information, see [Long-Lived Operations](/documentation/cloudkit/ckoperation#Long-Lived-Operations).

## [See Also](/documentation/cloudkit/ckoperation/longlivedoperationwaspersistedblock#see-also)

### [Managing the Operation’s Configuration](/documentation/cloudkit/ckoperation/longlivedoperationwaspersistedblock#Managing-the-Operations-Configuration)

[`var configuration: CKOperation.Configuration!`](/documentation/cloudkit/ckoperation/configuration-swift.property)

The operation’s configuration.

[`class Configuration`](/documentation/cloudkit/ckoperation/configuration-swift.class)

An object that describes how a CloudKit operation behaves.

[`var group: CKOperationGroup?`](/documentation/cloudkit/ckoperation/group)

The operation’s group.

Current page is longLivedOperationWasPersistedBlock