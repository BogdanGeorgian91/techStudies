# CKUserIdentity | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKUserIdentity

Class

# CKUserIdentity

The identity of a user.

```
class CKUserIdentity
```

## [Overview](/documentation/cloudkit/ckuseridentity#overview)

A user identity provides identifiable data about an iCloud user, including their name, user record ID, and an email address or phone number. CloudKit retrieves this information from the user’s iCloud account. A user must give their consent to be discoverable before CloudKit can provide this data to your app. For more information, see [`requestApplicationPermission(_:completionHandler:)`](/documentation/cloudkit/ckcontainer/requestapplicationpermission\(_:completionhandler:\)).

You don’t create instances of this class. Instead, CloudKit provides them in certain contexts. A share’s owner has a user identity, as does each of its participants. When creating participants, CloudKit tries to find iCloud accounts it can use to populate their identities. If CloudKit doesn’t find an account, it sets the identity’s [`hasiCloudAccount`](/documentation/cloudkit/ckuseridentity/hasicloudaccount) property to [`false`](/documentation/swift/false).

You can also discover the identities of your app’s users by executing one of the user discovery operations: [`CKDiscoverAllUserIdentitiesOperation`](/documentation/cloudkit/ckdiscoveralluseridentitiesoperation) and [`CKDiscoverUserIdentitiesOperation`](/documentation/cloudkit/ckdiscoveruseridentitiesoperation). Identities that CloudKit discovers using [`CKDiscoverAllUserIdentitiesOperation`](/documentation/cloudkit/ckdiscoveralluseridentitiesoperation) correspond to entries in the device’s Contacts database. These identities contain the identifiers of their Contact records, which you can use to fetch those records from the Contacts database. For more information, see [`contactIdentifiers`](/documentation/cloudkit/ckuseridentity/contactidentifiers).

## [Topics](/documentation/cloudkit/ckuseridentity#topics)

### [Accessing iCloud Information](/documentation/cloudkit/ckuseridentity#Accessing-iCloud-Information)

[`var hasiCloudAccount: Bool`](/documentation/cloudkit/ckuseridentity/hasicloudaccount)

A Boolean value that indicates whether the user has an iCloud account.

[`var lookupInfo: CKUserIdentity.LookupInfo?`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.property)

The lookup info for retrieving the user identity.

[`class LookupInfo`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class)

The criteria to use when searching for discoverable iCloud users.

### [Accessing User Information](/documentation/cloudkit/ckuseridentity#Accessing-User-Information)

[`var userRecordID: CKRecord.ID?`](/documentation/cloudkit/ckuseridentity/userrecordid)

The user record ID for the corresponding user record.

[`var contactIdentifiers: [String]`](/documentation/cloudkit/ckuseridentity/contactidentifiers)

Identifiers that match contacts in the local Contacts database.

Deprecated

[`var nameComponents: PersonNameComponents?`](/documentation/cloudkit/ckuseridentity/namecomponents)

The user’s name.

## [Relationships](/documentation/cloudkit/ckuseridentity#relationships)

### [Inherits From](/documentation/cloudkit/ckuseridentity#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckuseridentity#conforms-to)

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

## [See Also](/documentation/cloudkit/ckuseridentity#see-also)

### [User discovery](/documentation/cloudkit/ckuseridentity#User-discovery)

[`class LookupInfo`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class)

The criteria to use when searching for discoverable iCloud users.

Current page is CKUserIdentity

---
  
Accessing iCloud Information

# hasiCloudAccount | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKUserIdentity](/documentation/cloudkit/ckuseridentity)
-   hasiCloudAccount

Instance Property

# hasiCloudAccount

A Boolean value that indicates whether the user has an iCloud account.

```
var hasiCloudAccount: Bool { get }
```

## [Discussion](/documentation/cloudkit/ckuseridentity/hasicloudaccount#Discussion)

[`true`](/documentation/swift/true) if the user identity has an iCloud account; otherwise, [`false`](/documentation/swift/false).

## [See Also](/documentation/cloudkit/ckuseridentity/hasicloudaccount#see-also)

### [Accessing iCloud Information](/documentation/cloudkit/ckuseridentity/hasicloudaccount#Accessing-iCloud-Information)

[`var lookupInfo: CKUserIdentity.LookupInfo?`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.property)

The lookup info for retrieving the user identity.

[`class LookupInfo`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class)

The criteria to use when searching for discoverable iCloud users.

Current page is hasiCloudAccount

# lookupInfo | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKUserIdentity](/documentation/cloudkit/ckuseridentity)
-   lookupInfo

Instance Property

# lookupInfo

The lookup info for retrieving the user identity.

```
@NSCopying
var lookupInfo: CKUserIdentity.LookupInfo? { get }
```

## [Discussion](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.property#Discussion)

Use this property’s value to retrieve the user identity when using the [`CKDiscoverUserIdentitiesOperation`](/documentation/cloudkit/ckdiscoveruseridentitiesoperation) and [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) operations.

## [See Also](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.property#see-also)

### [Accessing iCloud Information](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.property#Accessing-iCloud-Information)

[`var hasiCloudAccount: Bool`](/documentation/cloudkit/ckuseridentity/hasicloudaccount)

A Boolean value that indicates whether the user has an iCloud account.

[`class LookupInfo`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class)

The criteria to use when searching for discoverable iCloud users.

Current page is lookupInfo

---
**# CKUserIdentity.LookupInfo | Apple Developer Documentation

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

Current page is CKUserIdentity.LookupInfo**

  
Creating a Lookup Info

# init(emailAddress:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKUserIdentity](/documentation/cloudkit/ckuseridentity)
-   [CKUserIdentity.LookupInfo](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class)
-   init(emailAddress:)

Initializer

# init(emailAddress:)

Creates a lookup info for the specified email address.

```
init(emailAddress: String)
```

## [Parameters](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(emailaddress:\)#parameters)

`emailAddress`

The email address for looking up the user identity.

## [Discussion](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(emailaddress:\)#Discussion)

After you create a lookup info, use the [`CKDiscoverUserIdentitiesOperation`](/documentation/cloudkit/ckdiscoveruseridentitiesoperation) operation or the [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) operation to retrieve the corresponding user identity.

## [See Also](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(emailaddress:\)#see-also)

### [Creating a Lookup Info](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(emailaddress:\)#Creating-a-Lookup-Info)

[`init(phoneNumber: String)`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(phonenumber:\))

Creates a lookup info for the specified phone number.

[`init(userRecordID: CKRecord.ID)`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(userrecordid:\))

Creates a lookup info for the specified user record ID.

Current page is init(emailAddress:)

# init(phoneNumber:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKUserIdentity](/documentation/cloudkit/ckuseridentity)
-   [CKUserIdentity.LookupInfo](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class)
-   init(phoneNumber:)

Initializer

# init(phoneNumber:)

Creates a lookup info for the specified phone number.

```
init(phoneNumber: String)
```

## [Parameters](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(phonenumber:\)#parameters)

`phoneNumber`

The phone number for looking up the user identity.

## [Discussion](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(phonenumber:\)#Discussion)

After you create a lookup info, use the [`CKDiscoverUserIdentitiesOperation`](/documentation/cloudkit/ckdiscoveruseridentitiesoperation) operation or the [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) operation to retrieve the corresponding user identity.

## [See Also](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(phonenumber:\)#see-also)

### [Creating a Lookup Info](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(phonenumber:\)#Creating-a-Lookup-Info)

[`init(emailAddress: String)`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(emailaddress:\))

Creates a lookup info for the specified email address.

[`init(userRecordID: CKRecord.ID)`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(userrecordid:\))

Creates a lookup info for the specified user record ID.

Current page is init(phoneNumber:)

# init(userRecordID:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKUserIdentity](/documentation/cloudkit/ckuseridentity)
-   [CKUserIdentity.LookupInfo](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class)
-   init(userRecordID:)

Initializer

# init(userRecordID:)

Creates a lookup info for the specified user record ID.

```
init(userRecordID: CKRecord.ID)
```

## [Parameters](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(userrecordid:\)#parameters)

`userRecordID`

The user record ID for looking up the user identity.

## [Discussion](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(userrecordid:\)#Discussion)

After you create a lookup info, use the [`CKDiscoverUserIdentitiesOperation`](/documentation/cloudkit/ckdiscoveruseridentitiesoperation) operation or the [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) operation to retrieve the corresponding user identity.

## [See Also](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(userrecordid:\)#see-also)

### [Creating a Lookup Info](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(userrecordid:\)#Creating-a-Lookup-Info)

[`init(emailAddress: String)`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(emailaddress:\))

Creates a lookup info for the specified email address.

[`init(phoneNumber: String)`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/init\(phonenumber:\))

Creates a lookup info for the specified phone number.

Current page is init(userRecordID:)

  
Creating Multiple Lookup Infos

# lookupInfos(withEmails:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKUserIdentity](/documentation/cloudkit/ckuseridentity)
-   [CKUserIdentity.LookupInfo](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class)
-   lookupInfos(withEmails:)

Type Method

# lookupInfos(withEmails:)

Returns an array of lookup infos for the specifed email addresses.

```
class func lookupInfos(withEmails emails: [String]) -> [CKUserIdentity.LookupInfo]
```

## [Parameters](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withemails:\)#parameters)

`emails`

The email addresses for looking up the user identities.

## [Discussion](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withemails:\)#Discussion)

Use the values that this method returns in an [`CKDiscoverUserIdentitiesOperation`](/documentation/cloudkit/ckdiscoveruseridentitiesoperation) operation or an [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) operation to retrieve the corresponding user identities.

## [See Also](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withemails:\)#see-also)

### [Creating Multiple Lookup Infos](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withemails:\)#Creating-Multiple-Lookup-Infos)

[`class func lookupInfos(withPhoneNumbers: [String]) -> [CKUserIdentity.LookupInfo]`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withphonenumbers:\))

Returns an array of lookup infos for the specifed phone numbers.

[`class func lookupInfos(with: [CKRecord.ID]) -> [CKUserIdentity.LookupInfo]`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(with:\))

Returns an array of lookup infos for the specifed user record IDs.

Current page is lookupInfos(withEmails:)

# lookupInfos(withPhoneNumbers:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKUserIdentity](/documentation/cloudkit/ckuseridentity)
-   [CKUserIdentity.LookupInfo](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class)
-   lookupInfos(withPhoneNumbers:)

Type Method

# lookupInfos(withPhoneNumbers:)

Returns an array of lookup infos for the specifed phone numbers.

```
class func lookupInfos(withPhoneNumbers phoneNumbers: [String]) -> [CKUserIdentity.LookupInfo]
```

## [Parameters](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withphonenumbers:\)#parameters)

`phoneNumbers`

The phone numbers for looking up the user identities.

## [Discussion](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withphonenumbers:\)#Discussion)

Use the values that this method returns in an [`CKDiscoverUserIdentitiesOperation`](/documentation/cloudkit/ckdiscoveruseridentitiesoperation) operation or an [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) operation to retrieve the corresponding user identities.

## [See Also](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withphonenumbers:\)#see-also)

### [Creating Multiple Lookup Infos](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withphonenumbers:\)#Creating-Multiple-Lookup-Infos)

[`class func lookupInfos(withEmails: [String]) -> [CKUserIdentity.LookupInfo]`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withemails:\))

Returns an array of lookup infos for the specifed email addresses.

[`class func lookupInfos(with: [CKRecord.ID]) -> [CKUserIdentity.LookupInfo]`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(with:\))

Returns an array of lookup infos for the specifed user record IDs.

Current page is lookupInfos(withPhoneNumbers:)

# lookupInfos(with:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKUserIdentity](/documentation/cloudkit/ckuseridentity)
-   [CKUserIdentity.LookupInfo](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class)
-   lookupInfos(with:)

Type Method

# lookupInfos(with:)

Returns an array of lookup infos for the specifed user record IDs.

```
class func lookupInfos(with recordIDs: [CKRecord.ID]) -> [CKUserIdentity.LookupInfo]
```

## [Parameters](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(with:\)#parameters)

`recordIDs`

The user record IDs for looking up the user identities.

## [Discussion](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(with:\)#Discussion)

Use the values that this method returns in an [`CKDiscoverUserIdentitiesOperation`](/documentation/cloudkit/ckdiscoveruseridentitiesoperation) operation or an [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) operation to retrieve the corresponding user identities.

## [See Also](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(with:\)#see-also)

### [Creating Multiple Lookup Infos](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(with:\)#Creating-Multiple-Lookup-Infos)

[`class func lookupInfos(withEmails: [String]) -> [CKUserIdentity.LookupInfo]`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withemails:\))

Returns an array of lookup infos for the specifed email addresses.

[`class func lookupInfos(withPhoneNumbers: [String]) -> [CKUserIdentity.LookupInfo]`](/documentation/cloudkit/ckuseridentity/lookupinfo-swift.class/lookupinfos\(withphonenumbers:\))

Returns an array of lookup infos for the specifed phone numbers.

Current page is lookupInfos(with:)

### Accessing the Criteria

var emailAddress: [`String`](https://developer.apple.com/documentation/Swift/String)? { get }
var phoneNumber: [`String`](https://developer.apple.com/documentation/Swift/String)? { get }
@NSCopying var userRecordID: [`CKRecord`](https://developer.apple.com/documentation/cloudkit/ckrecord).[`ID`](https://developer.apple.com/documentation/cloudkit/ckrecord/id)? { get }

---
### Accessing User Information
# userRecordID | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKUserIdentity](/documentation/cloudkit/ckuseridentity)
-   userRecordID

Instance Property

# userRecordID

The user record ID for the corresponding user record.

```
@NSCopying
var userRecordID: CKRecord.ID? { get }
```

## [See Also](/documentation/cloudkit/ckuseridentity/userrecordid#see-also)

### [Accessing User Information](/documentation/cloudkit/ckuseridentity/userrecordid#Accessing-User-Information)

[`var contactIdentifiers: [String]`](/documentation/cloudkit/ckuseridentity/contactidentifiers)

Identifiers that match contacts in the local Contacts database.

Deprecated

[`var nameComponents: PersonNameComponents?`](/documentation/cloudkit/ckuseridentity/namecomponents)

The user’s name.

Current page is userRecordID

# nameComponents | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKUserIdentity](/documentation/cloudkit/ckuseridentity)
-   nameComponents

Instance Property

# nameComponents

The user’s name.

```
var nameComponents: PersonNameComponents? { get }
```

## [Discussion](/documentation/cloudkit/ckuseridentity/namecomponents#Discussion)

You can use this property to construct the user’s name for display. Use the components with an instance of [`PersonNameComponentsFormatter`](/documentation/Foundation/PersonNameComponentsFormatter) to create a string representation for the current locale.

## [See Also](/documentation/cloudkit/ckuseridentity/namecomponents#see-also)

### [Accessing User Information](/documentation/cloudkit/ckuseridentity/namecomponents#Accessing-User-Information)

[`var userRecordID: CKRecord.ID?`](/documentation/cloudkit/ckuseridentity/userrecordid)

The user record ID for the corresponding user record.

[`var contactIdentifiers: [String]`](/documentation/cloudkit/ckuseridentity/contactidentifiers)

Identifiers that match contacts in the local Contacts database.

Deprecated

Current page is nameComponents