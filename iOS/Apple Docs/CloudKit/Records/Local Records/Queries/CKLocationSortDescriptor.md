# CKLocationSortDescriptor

An object for sorting records that contain location data.

```
class CKLocationSortDescriptor
```

## [Overview](/documentation/cloudkit/cklocationsortdescriptor#overview)

You can add a location sort descriptor to your queries when searching for records. At creation time, you must provide the sort descriptor with a key that has a [`CLLocation`](/documentation/CoreLocation/CLLocation) object as its value. The sort descriptor uses the value of that key to perform the sort.

CloudKit computes distance by drawing a direct line between the two locations that follows the curvature of the Earth. Distances don’t account for altitude changes between the two locations.

## [Topics](/documentation/cloudkit/cklocationsortdescriptor#topics)

### [Creating a Location Sort Descriptor](/documentation/cloudkit/cklocationsortdescriptor#Creating-a-Location-Sort-Descriptor)

[`init(key: String, relativeLocation: CLLocation)`](/documentation/cloudkit/cklocationsortdescriptor/init\(key:relativelocation:\))

Creates a location sort descriptor using the specified key and relative location.

[`init(coder: NSCoder)`](/documentation/cloudkit/cklocationsortdescriptor/init\(coder:\))

Creates a location sort descriptor from a serialized instance.

### [Accessing the Location Value](/documentation/cloudkit/cklocationsortdescriptor#Accessing-the-Location-Value)

[`var relativeLocation: CLLocation`](/documentation/cloudkit/cklocationsortdescriptor/relativelocation)

The reference location for sorting records.

## [Relationships](/documentation/cloudkit/cklocationsortdescriptor#relationships)

### [Inherits From](/documentation/cloudkit/cklocationsortdescriptor#inherits-from)

-   [`NSSortDescriptor`](/documentation/Foundation/NSSortDescriptor)

### [Conforms To](/documentation/cloudkit/cklocationsortdescriptor#conforms-to)

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

---
### Creating a Location Sort Descriptor
# init(key:relativeLocation:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKLocationSortDescriptor](/documentation/cloudkit/cklocationsortdescriptor)
-   init(key:relativeLocation:)

Initializer

# init(key:relativeLocation:)

Creates a location sort descriptor using the specified key and relative location.

```
init(
    key: String,
    relativeLocation: CLLocation
)
```

## [Parameters](/documentation/cloudkit/cklocationsortdescriptor/init\(key:relativelocation:\)#parameters)

`key`

The name of the key with a [`CLLocation`](/documentation/CoreLocation/CLLocation) object as its value. The key must belong to the records you’re sorting. The sort descriptor uses this key to retrieve the corresponding value from the record.

`relativeLocation`

The reference location when sorting. CloudKit sorts records according to their distance from this location.

## [Discussion](/documentation/cloudkit/cklocationsortdescriptor/init\(key:relativelocation:\)#Discussion)

During sorting, the sort descriptor computes the distance between the value in the `relativeLocation` parameter and the location value in the specified key of each record. It then sorts the records in ascending order using the distance between the two points. You can’t change the sort order.


# init(coder:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKLocationSortDescriptor](/documentation/cloudkit/cklocationsortdescriptor)
-   init(coder:)

Initializer

# init(coder:)

Creates a location sort descriptor from a serialized instance.

```
init(coder aDecoder: NSCoder)
```

## [Parameters](/documentation/cloudkit/cklocationsortdescriptor/init\(coder:\)#parameters)

`aDecoder`

The coder to use when deserializing the location sort descriptor.

## [See Also](/documentation/cloudkit/cklocationsortdescriptor/init\(coder:\)#see-also)

### [Creating a Location Sort Descriptor](/documentation/cloudkit/cklocationsortdescriptor/init\(coder:\)#Creating-a-Location-Sort-Descriptor)

[`init(key: String, relativeLocation: CLLocation)`](/documentation/cloudkit/cklocationsortdescriptor/init\(key:relativelocation:\))

Creates a location sort descriptor using the specified key and relative location.

Current page is init(coder:)

---
### Accessing the Location Value

# relativeLocation | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKLocationSortDescriptor](/documentation/cloudkit/cklocationsortdescriptor)
-   relativeLocation

Instance Property

# relativeLocation

The reference location for sorting records.

```
@NSCopying
var relativeLocation: CLLocation { get }
```

Current page is relativeLocation
