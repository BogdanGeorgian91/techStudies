# CKUserIdentity.LookupInfo | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKUserIdentity](/documentation/cloudkit/ckuseridentity)
-   CKUserIdentity.LookupInfo

Class

# CKUserIdentity.LookupInfo

The criteria to use when searching for discoverable iCloud users.

```
class LookupInfo
```

## [Overview](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class#overview)

Use this object when you want to discover the identities of your app’s users with [`CKDiscoverUserIdentitiesOperation`](/documentation/cloudkit/ckdiscoveruseridentitiesoperation), or to create a share’s participants with [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation).

You create individual instances by providing an email address, phone number, or user record ID. Alternatively, create an array of objects all at once by using one of the convenience methods, such as [`lookupInfos(withEmails:)`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withemails:\)).

## [Topics](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class#topics)

### [Creating a Lookup Info](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class#Creating-a-Lookup-Info)

[`init(emailAddress: String)`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(emailaddress:\))

Creates a lookup info for the specified email address.

[`init(phoneNumber: String)`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(phonenumber:\))

Creates a lookup info for the specified phone number.

[`init(userRecordID: CKRecord.ID)`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(userrecordid:\))

Creates a lookup info for the specified user record ID.

### [Creating Multiple Lookup Infos](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class#Creating-Multiple-Lookup-Infos)

[`class func lookupInfos(withEmails: [String]) -> [CKUserIdentity.LookupInfo]`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withemails:\))

Returns an array of lookup infos for the specifed email addresses.

[`class func lookupInfos(withPhoneNumbers: [String]) -> [CKUserIdentity.LookupInfo]`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withphonenumbers:\))

Returns an array of lookup infos for the specifed phone numbers.

[`class func lookupInfos(with: [CKRecord.ID]) -> [CKUserIdentity.LookupInfo]`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(with:\))

Returns an array of lookup infos for the specifed user record IDs.

### [Accessing the Criteria](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class#Accessing-the-Criteria)

[`var emailAddress: String?`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/emailaddress)

The user’s email address.

[`var phoneNumber: String?`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/phonenumber)

The user’s phone number.

[`var userRecordID: CKRecord.ID?`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/userrecordid)

The ID of the user record.

## [Relationships](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class#relationships)

### [Inherits From](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class#conforms-to)

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

## [See Also](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class#see-also)

### [User discovery](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class#User-discovery)

[`class CKUserIdentity`](/documentation/cloudkit/ckuseridentity)

The identity of a user.

Current page is CKUserIdentity.LookupInfo