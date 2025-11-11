# CKOperationGroup

An explicit association between two or more operations.

```
class CKOperationGroup
```

## [Overview](/documentation/cloudkit/ckoperationgroup#overview)

In certain situations, you might want to perform several CloudKit operations together. Grouping operations in CloudKit doesn’t ensure atomicity.

For example, when building a Calendar app, you group the following actions:

-   Fetch records from CloudKit, which consists of numerous queries that fetch both new records and records with changes.
    
-   Perform incremental fetches of records in response to a push notification.
    
-   Update several records when the user saves a calendar event.
    

Associate operation groups with operations by setting their [`group`](/documentation/cloudkit/ckoperation/group) property. Create a new operation group for each distinct user interaction.

## [Topics](/documentation/cloudkit/ckoperationgroup#topics)

### [Creating an Operation Group](/documentation/cloudkit/ckoperationgroup#Creating-an-Operation-Group)

[`init()`](/documentation/cloudkit/ckoperationgroup/init\(\))

Creates an operation group.

[`init(coder: NSCoder)`](/documentation/cloudkit/ckoperationgroup/init\(coder:\))

Creates an operation group from a serialized instance.

### [Configuring an Operation Group](/documentation/cloudkit/ckoperationgroup#Configuring-an-Operation-Group)

[`var defaultConfiguration: CKOperation.Configuration!`](/documentation/cloudkit/ckoperationgroup/defaultconfiguration)

The default configuration for operations in the group.

[`var expectedReceiveSize: CKOperationGroup.TransferSize`](/documentation/cloudkit/ckoperationgroup/expectedreceivesize)

The estimated size of traffic to download from CloudKit.

[`var expectedSendSize: CKOperationGroup.TransferSize`](/documentation/cloudkit/ckoperationgroup/expectedsendsize)

The estimated size of traffic to upload to CloudKit.

[`var name: String?`](/documentation/cloudkit/ckoperationgroup/name)

The operation group’s name.

[`var operationGroupID: String`](/documentation/cloudkit/ckoperationgroup/operationgroupid)

The operation group’s unique identifier.

[`var quantity: Int`](/documentation/cloudkit/ckoperationgroup/quantity)

The number of operations in the operation group.

[`enum TransferSize`](/documentation/cloudkit/ckoperationgroup/transfersize)

Constants that represent possible data transfer sizes.

## [Relationships](/documentation/cloudkit/ckoperationgroup#relationships)

### [Inherits From](/documentation/cloudkit/ckoperationgroup#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckoperationgroup#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
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

# init(coder:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperationGroup](/documentation/cloudkit/ckoperationgroup)
-   init(coder:)

Initializer

# init(coder:)

Creates an operation group from a serialized instance.

```
init(coder aDecoder: NSCoder)
```

## [Parameters](/documentation/cloudkit/ckoperationgroup/init\(coder:\)#parameters)

`aDecoder`

The coder to use when deserializing the group.

## [See Also](/documentation/cloudkit/ckoperationgroup/init\(coder:\)#see-also)

### [Creating an Operation Group](/documentation/cloudkit/ckoperationgroup/init\(coder:\)#Creating-an-Operation-Group)

[`init()`](/documentation/cloudkit/ckoperationgroup/init\(\))

Creates an operation group.

Current page is init(coder:)

---
  
Configuring an Operation Group

# defaultConfiguration | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperationGroup](/documentation/cloudkit/ckoperationgroup)
-   defaultConfiguration

Instance Property

# defaultConfiguration

The default configuration for operations in the group.

```
@NSCopying
var defaultConfiguration: CKOperation.Configuration! { get set }
```

## [Discussion](/documentation/cloudkit/ckoperationgroup/defaultconfiguration#Discussion)

If an operation in the group has its own configuration, that configuration’s values override the default configuration’s values. For more information, see [`CKOperation.Configuration`](/documentation/cloudkit/ckoperation/configuration-swift.class).
# expectedReceiveSize | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperationGroup](/documentation/cloudkit/ckoperationgroup)
-   expectedReceiveSize

Instance Property

# expectedReceiveSize

The estimated size of traffic to download from CloudKit.

```
var expectedReceiveSize: CKOperationGroup.TransferSize { get set }
```

## [Discussion](/documentation/cloudkit/ckoperationgroup/expectedreceivesize#Discussion)

This property informs the system about the amount of data your app can transfer. An order-of-magnitude estimate is better than no estimate, and accuracy helps performance. The system checks this value when it schedules discretionary network requests.

## [See Also](/documentation/cloudkit/ckoperationgroup/expectedreceivesize#see-also)

### [Configuring an Operation Group](/documentation/cloudkit/ckoperationgroup/expectedreceivesize#Configuring-an-Operation-Group)

[`var defaultConfiguration: CKOperation.Configuration!`](/documentation/cloudkit/ckoperationgroup/defaultconfiguration)

The default configuration for operations in the group.

[`var expectedSendSize: CKOperationGroup.TransferSize`](/documentation/cloudkit/ckoperationgroup/expectedsendsize)

The estimated size of traffic to upload to CloudKit.

[`var name: String?`](/documentation/cloudkit/ckoperationgroup/name)

The operation group’s name.

[`var operationGroupID: String`](/documentation/cloudkit/ckoperationgroup/operationgroupid)

The operation group’s unique identifier.

[`var quantity: Int`](/documentation/cloudkit/ckoperationgroup/quantity)

The number of operations in the operation group.

[`enum TransferSize`](/documentation/cloudkit/ckoperationgroup/transfersize)

Constants that represent possible data transfer sizes.

Current page is expectedReceiveSize

# expectedSendSize | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperationGroup](/documentation/cloudkit/ckoperationgroup)
-   expectedSendSize

Instance Property

# expectedSendSize

The estimated size of traffic to upload to CloudKit.

```
var expectedSendSize: CKOperationGroup.TransferSize { get set }
```

## [Discussion](/documentation/cloudkit/ckoperationgroup/expectedsendsize#Discussion)

This property informs the system about the amount of data your app can transfer. An order-of-magnitude estimate is better than no estimate, and accuracy helps performance. The system checks this value when it schedules discretionary network requests.
# name | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperationGroup](/documentation/cloudkit/ckoperationgroup)
-   name

Instance Property

# name

The operation group’s name.

```
var name: String? { get set }
```

## [Discussion](/documentation/cloudkit/ckoperationgroup/name#Discussion)

The system sends the name of the operation group to CloudKit to provide aggregate reporting for [`CKOperationGroup`](/documentation/cloudkit/ckoperationgroup). The name must not include any personal data.

## [See Also](/documentation/cloudkit/ckoperationgroup/name#see-also)

### [Configuring an Operation Group](/documentation/cloudkit/ckoperationgroup/name#Configuring-an-Operation-Group)

[`var defaultConfiguration: CKOperation.Configuration!`](/documentation/cloudkit/ckoperationgroup/defaultconfiguration)

The default configuration for operations in the group.

[`var expectedReceiveSize: CKOperationGroup.TransferSize`](/documentation/cloudkit/ckoperationgroup/expectedreceivesize)

The estimated size of traffic to download from CloudKit.

[`var expectedSendSize: CKOperationGroup.TransferSize`](/documentation/cloudkit/ckoperationgroup/expectedsendsize)

The estimated size of traffic to upload to CloudKit.

[`var operationGroupID: String`](/documentation/cloudkit/ckoperationgroup/operationgroupid)

The operation group’s unique identifier.

[`var quantity: Int`](/documentation/cloudkit/ckoperationgroup/quantity)

The number of operations in the operation group.

[`enum TransferSize`](/documentation/cloudkit/ckoperationgroup/transfersize)

Constants that represent possible data transfer sizes.

Current page is name

# operationGroupID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperationGroup](/documentation/cloudkit/ckoperationgroup)
-   operationGroupID

Instance Property

# operationGroupID

The operation group’s unique identifier.

```
var operationGroupID: String { get }
```

## [Discussion](/documentation/cloudkit/ckoperationgroup/operationgroupid#Discussion)

The framework generates this value and it’s unique to this operation group. The system sends this identifier to CloudKit, which can use it to identify server-side logs for [`CKOperationGroup`](/documentation/cloudkit/ckoperationgroup).
# quantity | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperationGroup](/documentation/cloudkit/ckoperationgroup)
-   quantity

Instance Property

# quantity

The number of operations in the operation group.

```
var quantity: Int { get set }
```

## [Discussion](/documentation/cloudkit/ckoperationgroup/quantity#Discussion)

This property shows the number of operations that you expect to be in this operation group. It’s the developer’s responsibility to set this value.

# CKOperationGroup.TransferSize | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKOperationGroup](/documentation/cloudkit/ckoperationgroup)
-   CKOperationGroup.TransferSize

Enumeration

# CKOperationGroup.TransferSize

Constants that represent possible data transfer sizes.

```
enum TransferSize
```

## [Topics](/documentation/cloudkit/ckoperationgroup/transfersize#topics)

### [Transfer Sizes](/documentation/cloudkit/ckoperationgroup/transfersize#Transfer-Sizes)

[`case kilobytes`](/documentation/cloudkit/ckoperationgroup/transfersize/kilobytes)

A transfer size that represents 1 or more kilobytes.

[`case megabytes`](/documentation/cloudkit/ckoperationgroup/transfersize/megabytes)

A transfer size that represents 1 or more megabytes.

[`case gigabytes`](/documentation/cloudkit/ckoperationgroup/transfersize/gigabytes)

A transfer size that represents 1 or more gigabytes.

[`case tensOfMegabytes`](/documentation/cloudkit/ckoperationgroup/transfersize/tensofmegabytes)

A transfer size that represents tens of megabytes.

[`case tensOfGigabytes`](/documentation/cloudkit/ckoperationgroup/transfersize/tensofgigabytes)

A transfer size that represents tens of gigabytes.

[`case hundredsOfMegabytes`](/documentation/cloudkit/ckoperationgroup/transfersize/hundredsofmegabytes)

A transfer size that represents hundreds of megabytes.

[`case hundredsOfGigabytes`](/documentation/cloudkit/ckoperationgroup/transfersize/hundredsofgigabytes)

A transfer size that represents hundreds of gigabytes.

[`case unknown`](/documentation/cloudkit/ckoperationgroup/transfersize/unknown)

An unknown transfer size.

### [Initializers](/documentation/cloudkit/ckoperationgroup/transfersize#Initializers)

[`init?(rawValue: Int)`](/documentation/cloudkit/ckoperationgroup/transfersize/init\(rawvalue:\))

## [Relationships](/documentation/cloudkit/ckoperationgroup/transfersize#relationships)

### [Conforms To](/documentation/cloudkit/ckoperationgroup/transfersize#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)