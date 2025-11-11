# CKContainer

A conduit to your app’s databases.

```
class CKContainer
```

## [Overview](/documentation/cloudkit/ckcontainer#overview)

A container manages all explicit and implicit attempts to access its contents.

Every app has a default container that manages its own content. If you develop a suite of apps, you can access any containers that you have the appropriate entitlements for. Each new container distinguishes between public and private data. CloudKit always stores private data in the appropriate container directory in the user’s iCloud account.

### [Interacting with a Container](/documentation/cloudkit/ckcontainer#Interacting-with-a-Container)

A container coordinates all interactions between your app and the server. Most of these interactions involve the following tasks:

-   Determining whether the user has an iCloud account, which lets you know if you can write data to the user’s personal storage.
    
-   With the user’s permission, discovering other users who the current user knows, and making the current user’s information discoverable.
    
-   Getting the container or one of its databases to use with an operation.
    

### [Public and Private Databases](/documentation/cloudkit/ckcontainer#Public-and-Private-Databases)

Each container provides a public and a private database for storing data. The contents of the public database are accessible to all users of the app, whereas the contents of the private database are, by default, visible only to the current user. Content that is specific to a single user usually belongs in that user’s private database, whereas app-related content that you provide (or that users want to share) belongs in the public database.

The public database is always available, regardless of whether the device has an active iCloud account. When there isn’t an iCloud account, your app can fetch records from and query the public database, but it can’t save changes. Saving records to the public database requires an active iCloud account to identify the owner of those records. Access to the private database always requires an active iCloud account on the device.

### [Using iCloud](/documentation/cloudkit/ckcontainer#Using-iCloud)

Whenever possible, design your app to run gracefully with or without an active iCloud account. Even without an active iCloud account, apps can fetch records from the public database and display that information to the user. If your app requires the ability to write to the public database or requires access to the private database, notify the user of the reason and encourage them to enable iCloud. You can even provide a button that takes the user directly to Settings so that they can enable iCloud. To implement such a button, have the button’s action open the URL that the [`openSettingsURLString`](/documentation/UIKit/UIApplication/openSettingsURLString) constant provides.

### [User Records and Permissions](/documentation/cloudkit/ckcontainer#User-Records-and-Permissions)

When a user accesses a container for the first time, CloudKit assigns them a unique identifier and uses it to create two user records — one in the app’s public database and another in that user’s private database. By default, these records don’t contain any identifying personal information, but you can use the record in the user’s private database to store additional, nonsensitive information about that user. Because the public database’s user record is accessible to all users of your app, don’t use it to store information about the user.

While a user record isn’t the same as the user’s [`CKUserIdentity`](/documentation/cloudkit/ckuseridentity), the identity does provide the identifier of their user record that you can use to fetch that record from either the public database or the user’s private database. For more information, see [`userRecordID`](/documentation/cloudkit/ckuseridentity/userrecordid).

### [Testing Your Code Using the Development Container](/documentation/cloudkit/ckcontainer#Testing-Your-Code-Using-the-Development-Container)

At runtime, CloudKit uses your app’s `com.apple.developer.icloud-container-environment` entitlement to discover whether you’re using a `Development` or `Production` version of your provisioning profile. When you configure the entitlement for development, CloudKit configures the app’s containers to use the development server. The development environment is a safe place to make changes during the development process without disrupting users of your app. You can add new fields to records programmatically, and you can delete or modify fields using iCloud Dashboard.

Before shipping your app, always test your app’s behavior in the production environment. The production server generates errors when your app tries to add record types or add new fields to existing record types. Testing in the production environment helps you find and fix the places in your code where you’re making those types of changes. You can use CloudKit Dashboard to modify record types in the development environment, and then migrate those changes to the production environment.

## [Topics](/documentation/cloudkit/ckcontainer#topics)

### [Creating Containers](/documentation/cloudkit/ckcontainer#Creating-Containers)

[``class func `default`() -> CKContainer``](/documentation/cloudkit/ckcontainer/default\(\))

Returns the app’s default container.

[`init(identifier: String)`](/documentation/cloudkit/ckcontainer/init\(identifier:\))

Creates a container for the specified identifier.

### [Getting the Public and Private Databases](/documentation/cloudkit/ckcontainer#Getting-the-Public-and-Private-Databases)

[`var privateCloudDatabase: CKDatabase`](/documentation/cloudkit/ckcontainer/privateclouddatabase)

The user’s private database.

[`var publicCloudDatabase: CKDatabase`](/documentation/cloudkit/ckcontainer/publicclouddatabase)

The app’s public database.

[`var sharedCloudDatabase: CKDatabase`](/documentation/cloudkit/ckcontainer/sharedclouddatabase)

The database that contains shared data.

[`func database(with: CKDatabase.Scope) -> CKDatabase`](/documentation/cloudkit/ckcontainer/database\(with:\))

Returns the database with the specified scope.

### [Getting the Container’s Identifier](/documentation/cloudkit/ckcontainer#Getting-the-Containers-Identifier)

[`var containerIdentifier: String?`](/documentation/cloudkit/ckcontainer/containeridentifier)

The container’s unique identifier.

### [Determining the User’s iCloud Access Status](/documentation/cloudkit/ckcontainer#Determining-the-Users-iCloud-Access-Status)

[`func accountStatus(completionHandler: (CKAccountStatus, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/accountstatus\(completionhandler:\))

Determines whether the system can access the user’s iCloud account.

[`enum CKAccountStatus`](/documentation/cloudkit/ckaccountstatus)

Constants that indicate the availability of the user’s iCloud account.

### [Requesting and Determining App Permissions](/documentation/cloudkit/ckcontainer#Requesting-and-Determining-App-Permissions)

[`func requestApplicationPermission(CKContainer.ApplicationPermissions, completionHandler: (CKContainer.ApplicationPermissionStatus, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/requestapplicationpermission\(_:completionhandler:\))

Prompts the user to authorize the specified permission.

Deprecated

[`func status(forApplicationPermission: CKContainer.ApplicationPermissions, completionHandler: (CKContainer.ApplicationPermissionStatus, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/status\(forapplicationpermission:completionhandler:\))

Determines the authorization status of the specified permission.

Deprecated

[`enum Application`](/documentation/cloudkit/ckcontainer/application)

A collection of types for app permissions.

[`struct ApplicationPermissions`](/documentation/cloudkit/ckcontainer/applicationpermissions)

Constants that represent the permissions that a user grants.

[`typealias ApplicationPermissionBlock`](/documentation/cloudkit/ckcontainer/applicationpermissionblock)

A closure that processes the outcome of a permissions request.

Deprecated

[`enum ApplicationPermissionStatus`](/documentation/cloudkit/ckcontainer/applicationpermissionstatus)

Constants that represent the status of a permission.

Deprecated

### [Performing Operations on the Container](/documentation/cloudkit/ckcontainer#Performing-Operations-on-the-Container)

[`func add(CKOperation)`](/documentation/cloudkit/ckcontainer/add\(_:\))

Adds an operation to the container’s queue.

### [Discovering User Records](/documentation/cloudkit/ckcontainer#Discovering-User-Records)

[`func discoverAllIdentities(completionHandler: ([CKUserIdentity]?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/discoverallidentities\(completionhandler:\))

Fetches all user identities that match entries in the user’s Contacts.

Deprecated

[`func discoverUserIdentity(withEmailAddress: String, completionHandler: (CKUserIdentity?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/discoveruseridentity\(withemailaddress:completionhandler:\))

Fetches the user identity for the specified email address.

Deprecated

[`func discoverUserIdentity(withPhoneNumber: String, completionHandler: (CKUserIdentity?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/discoveruseridentity\(withphonenumber:completionhandler:\))

Fetches the user identity for the specified phone number.

Deprecated

[`func discoverUserIdentity(withUserRecordID: CKRecord.ID, completionHandler: (CKUserIdentity?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/discoveruseridentity\(withuserrecordid:completionhandler:\))

Fetches the user identity for the specified user record ID.

Deprecated

[`func fetchShareParticipant(withEmailAddress: String, completionHandler: (CKShare.Participant?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/fetchshareparticipant\(withemailaddress:completionhandler:\))

Fetches the share participant with the specified email address.

[`func fetchShareParticipant(withPhoneNumber: String, completionHandler: (CKShare.Participant?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/fetchshareparticipant\(withphonenumber:completionhandler:\))

Fetches the share participant with the specified phone number.

[`func fetchShareParticipant(withUserRecordID: CKRecord.ID, completionHandler: (CKShare.Participant?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/fetchshareparticipant\(withuserrecordid:completionhandler:\))

Fetches the share participant with the specified user record ID.

[`func fetchUserRecordID(completionHandler: (CKRecord.ID?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/fetchuserrecordid\(completionhandler:\))

Fetches the user record ID of the current user.

[`let CKCurrentUserDefaultName: String`](/documentation/cloudkit/ckcurrentuserdefaultname)

A constant that provides the current user’s default name.

[`let CKOwnerDefaultName: String`](/documentation/cloudkit/ckownerdefaultname)

A constant that provides the default owner’s name.

Deprecated

### [Fetching Long-Lived Operations](/documentation/cloudkit/ckcontainer#Fetching-Long-Lived-Operations)

[`func fetchAllLongLivedOperationIDs(completionHandler: ([CKOperation.ID]?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/fetchalllonglivedoperationids\(completionhandler:\))

Fetches the IDs of any long-lived operations that are running.

[`func fetchLongLivedOperation(withID: CKOperation.ID, completionHandler: (CKOperation?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/fetchlonglivedoperation\(withid:completionhandler:\))

Fetches the long-lived operation for the specified operation ID.

### [Accessing Container Metadata](/documentation/cloudkit/ckcontainer#Accessing-Container-Metadata)

[`func fetchShareMetadata(with: URL, completionHandler: (CKShare.Metadata?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/fetchsharemetadata\(with:completionhandler:\))

Fetches the share metadata for the specified share URL.

[`func accept(CKShare.Metadata, completionHandler: (CKShare?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/accept\(_:completionhandler:\)-949ea)

Accepts the specified share metadata.

[`static let CKAccountChanged: NSNotification.Name`](/documentation/Foundation/NSNotification/Name-swift.struct/CKAccountChanged)

A notification that a container posts when the status of an iCloud account changes.

### [Instance Methods](/documentation/cloudkit/ckcontainer#Instance-Methods)

[`func accept([CKShare.Metadata]) async throws -> [CKShare.Metadata : Result<CKShare, any Error>]`](/documentation/cloudkit/ckcontainer/accept\(_:\))

[`func accept([CKShare.Metadata], completionHandler: (Result<[CKShare.Metadata : Result<CKShare, any Error>], any Error>) -> Void)`](/documentation/cloudkit/ckcontainer/accept\(_:completionhandler:\)-7s3t7)

[`func allLongLivedOperationIDs() async throws -> [CKOperation.ID]`](/documentation/cloudkit/ckcontainer/alllonglivedoperationids\(\))

[`func configuredWith<R>(configuration: CKOperation.Configuration?, group: CKOperationGroup?, body: (CKContainer) throws -> R) rethrows -> R`](/documentation/cloudkit/ckcontainer/configuredwith\(configuration:group:body:\)-40x6k)

[`func configuredWith<R>(configuration: CKOperation.Configuration?, group: CKOperationGroup?, body: (CKContainer) async throws -> R) async rethrows -> R`](/documentation/cloudkit/ckcontainer/configuredwith\(configuration:group:body:\)-4kc2l)

[`func discoverUserIdentities(forEmailAddresses: [String], completionHandler: (Result<[String : CKUserIdentity], any Error>) -> Void)`](/documentation/cloudkit/ckcontainer/discoveruseridentities\(foremailaddresses:completionhandler:\))Deprecated

[`func discoverUserIdentities(forPhoneNumbers: [String], completionHandler: (Result<[String : CKUserIdentity], any Error>) -> Void)`](/documentation/cloudkit/ckcontainer/discoveruseridentities\(forphonenumbers:completionhandler:\))Deprecated

[`func discoverUserIdentities(forUserRecordIDs: [CKRecord.ID], completionHandler: (Result<[CKRecord.ID : CKUserIdentity], any Error>) -> Void)`](/documentation/cloudkit/ckcontainer/discoveruseridentities\(foruserrecordids:completionhandler:\))Deprecated

[`func fetchShareMetadatas(for: [URL], completionHandler: (Result<[URL : Result<CKShare.Metadata, any Error>], any Error>) -> Void)`](/documentation/cloudkit/ckcontainer/fetchsharemetadatas\(for:completionhandler:\))

[`func fetchShareParticipants(forEmailAddresses: [String], completionHandler: (Result<[String : Result<CKShare.Participant, any Error>], any Error>) -> Void)`](/documentation/cloudkit/ckcontainer/fetchshareparticipants\(foremailaddresses:completionhandler:\))

[`func fetchShareParticipants(forPhoneNumbers: [String], completionHandler: (Result<[String : Result<CKShare.Participant, any Error>], any Error>) -> Void)`](/documentation/cloudkit/ckcontainer/fetchshareparticipants\(forphonenumbers:completionhandler:\))

[`func fetchShareParticipants(forUserRecordIDs: [CKRecord.ID], completionHandler: (Result<[CKRecord.ID : Result<CKShare.Participant, any Error>], any Error>) -> Void)`](/documentation/cloudkit/ckcontainer/fetchshareparticipants\(foruserrecordids:completionhandler:\))

[`func longLivedOperation(for: CKOperation.ID) async throws -> CKOperation?`](/documentation/cloudkit/ckcontainer/longlivedoperation\(for:\))

[`func requestShareAccess(for: [URL]) async throws -> [URL : Result<Void, any Error>]`](/documentation/cloudkit/ckcontainer/requestshareaccess\(for:\))

Requests share access for the specified URLs.

[`func shareMetadatas(for: [URL]) async throws -> [URL : Result<CKShare.Metadata, any Error>]`](/documentation/cloudkit/ckcontainer/sharemetadatas\(for:\))

[`func shareParticipants(for: [CKUserIdentity.LookupInfo]) async throws -> [CKUserIdentity.LookupInfo : Result<CKShare.Participant, any Error>]`](/documentation/cloudkit/ckcontainer/shareparticipants\(for:\))

Fetches share participants matching the provided lookup infos.

[`func shareParticipants(forEmailAddresses: [String]) async throws -> [String : Result<CKShare.Participant, any Error>]`](/documentation/cloudkit/ckcontainer/shareparticipants\(foremailaddresses:\))

[`func shareParticipants(forPhoneNumbers: [String]) async throws -> [String : Result<CKShare.Participant, any Error>]`](/documentation/cloudkit/ckcontainer/shareparticipants\(forphonenumbers:\))

[`func shareParticipants(forUserRecordIDs: [CKRecord.ID]) async throws -> [CKRecord.ID : Result<CKShare.Participant, any Error>]`](/documentation/cloudkit/ckcontainer/shareparticipants\(foruserrecordids:\))

[`func userIdentities(forEmailAddresses: [String]) async throws -> [String : CKUserIdentity]`](/documentation/cloudkit/ckcontainer/useridentities\(foremailaddresses:\))Deprecated

[`func userIdentities(forPhoneNumbers: [String]) async throws -> [String : CKUserIdentity]`](/documentation/cloudkit/ckcontainer/useridentities\(forphonenumbers:\))Deprecated

[`func userIdentities(forUserRecordIDs: [CKRecord.ID]) async throws -> [CKRecord.ID : CKUserIdentity]`](/documentation/cloudkit/ckcontainer/useridentities\(foruserrecordids:\))Deprecated

## [Relationships](/documentation/cloudkit/ckcontainer#relationships)

### [Inherits From](/documentation/cloudkit/ckcontainer#inherits-from)

-   [`NSObject`](/documentation/ObjectiveC/NSObject-swift.class)

### [Conforms To](/documentation/cloudkit/ckcontainer#conforms-to)

-   [`CVarArg`](/documentation/Swift/CVarArg)
-   [`CustomDebugStringConvertible`](/documentation/Swift/CustomDebugStringConvertible)
-   [`CustomStringConvertible`](/documentation/Swift/CustomStringConvertible)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`NSObjectProtocol`](/documentation/ObjectiveC/NSObjectProtocol)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

# default() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   default()

Type Method

# default()

Returns the app’s default container.

```
class func `default`() -> CKContainer
```

## [Discussion](/documentation/cloudkit/ckcontainer/default\(\)#Discussion)

Use this method to retrieve your app’s default container. This is the one you typically use to store your app’s data. If you want the container for a different app, create a container using the [`init(identifier:)`](/documentation/cloudkit/ckcontainer/init\(identifier:\)) method.

During development, the container uses the development environment. When you release your app, the container uses the production environment.

# init(identifier:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   init(identifier:)

Initializer

# init(identifier:)

Creates a container for the specified identifier.

```
init(identifier containerIdentifier: String)
```

## [Parameters](/documentation/cloudkit/ckcontainer/init\(identifier:\)#parameters)

`containerIdentifier`

The bundle identifier of the app with the container that you want to access. The bundle identifier must be in the app’s `com.apple.developer.icloud-container-identifiers` entitlement. This parameter must not be `nil`.

## [Discussion](/documentation/cloudkit/ckcontainer/init\(identifier:\)#Discussion)

The specified identifier must correspond to one of the ubiquity containers in the iCloud capabilities section of your Xcode project. Including the identifier with your app’s capabilities adds the corresponding entitlements to your app. To access your app’s default container, use the [`default()`](/documentation/cloudkit/ckcontainer/default\(\)) method instead.

# privateCloudDatabase | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   privateCloudDatabase

Instance Property

# privateCloudDatabase

The user’s private database.

```
var privateCloudDatabase: CKDatabase { get }
```

## [Discussion](/documentation/cloudkit/ckcontainer/privateclouddatabase#Discussion)

The user’s private database is only available if the device has an iCloud account. Only the user can access their private database, by default. They own all of the database’s content and can view and modify that content. Data in the private database isn’t visible in the developer portal.

Data in the private database counts toward the user’s iCloud storage quota.

If there isn’t an iCloud account on the user’s device, this property still returns a database, but any attempt to use it results in an error. To determine if there is an iCloud account on the device, use the [`accountStatus(completionHandler:)`](/documentation/cloudkit/ckcontainer/accountstatus\(completionhandler:\)) method.

# publicCloudDatabase | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   publicCloudDatabase

Instance Property

# publicCloudDatabase

The app’s public database.

```
var publicCloudDatabase: CKDatabase { get }
```

## [Discussion](/documentation/cloudkit/ckcontainer/publicclouddatabase#Discussion)

This database is available regardless of whether the user’s device has an iCloud account. The contents of the public database are readable by all users of the app, and users have write access to the records, and other objects, they create. The public database’s contents are visible in the developer portal, where you can assign roles to users and restrict access as necessary.

Data in the public database counts toward your app’s iCloud storage quota.

# sharedCloudDatabase | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   sharedCloudDatabase

Instance Property

# sharedCloudDatabase

The database that contains shared data.

```
var sharedCloudDatabase: CKDatabase { get }
```

## [Discussion](/documentation/cloudkit/ckcontainer/sharedclouddatabase#Discussion)

This database is only available if the device has an iCloud account. Permissions on the database are available only to the user according to the permissions of the enclosing [`CKShare`](/documentation/cloudkit/ckshare) instance, which represents the shared record. The current user doesn’t own the content in the shared database, and can view and modify that content only if the necessary permissions exist. Data in the shared database isn’t visible in the developer portal or to any user who doesn’t have access.

Data in the shared database counts toward your app’s iCloud storage quota.

If there isn’t an iCloud account on the user’s device, this property still returns a database, but any attempt to use it results in an error. To determine if there is an iCloud account on the device, use the [`accountStatus(completionHandler:)`](/documentation/cloudkit/ckcontainer/accountstatus\(completionhandler:\)) method.

# database(with:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   database(with:)

Instance Method

# database(with:)

Returns the database with the specified scope.

```
func database(with databaseScope: CKDatabase.Scope) -> CKDatabase
```

## [Parameters](/documentation/cloudkit/ckcontainer/database\(with:\)#parameters)

`databaseScope`

The database’s scope. See [`CKDatabase.Scope`](/documentation/cloudkit/ckdatabase/scope) for the available options.

# containerIdentifier | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   containerIdentifier

Instance Property

# containerIdentifier

The container’s unique identifier.

```
var containerIdentifier: String? { get }
```

## [Discussion](/documentation/cloudkit/ckcontainer/containeridentifier#Discussion)

Use this property’s value to distinguish different containers in your app.

Current page is containerIdentifier

---
  
Determining the User’s iCloud Access Status

# accountStatus(completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   accountStatus(completionHandler:)

Instance Method

# accountStatus(completionHandler:)

Determines whether the system can access the user’s iCloud account.

```
func accountStatus(completionHandler: @escaping (CKAccountStatus, (any Error)?) -> Void)
```

```
func accountStatus() async throws -> CKAccountStatus
```

## [Parameters](/documentation/cloudkit/ckcontainer/accountstatus\(completionhandler:\)#parameters)

`completionHandler`

The handler to execute when the call completes.

## [Discussion](/documentation/cloudkit/ckcontainer/accountstatus\(completionhandler:\)#Discussion)

The closure has no return value and takes the following parameters:

-   The status of the user’s iCloud account.
    
-   An error that describes the failure, or `nil` if the system successfully determines the status.
    

This method determines the status of the user’s iCloud account asynchronously, passing the results to the closure that you provide. Call this method before accessing the private database to determine whether that database is available. While your app is running, use the [`CKAccountChanged`](/documentation/Foundation/NSNotification/Name-swift.struct/CKAccountChanged) notification to detect account changes, and call this method again to determine the status of the new account.

# CKAccountStatus | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKAccountStatus

Enumeration

# CKAccountStatus

Constants that indicate the availability of the user’s iCloud account.

```
enum CKAccountStatus
```

## [Topics](/documentation/cloudkit/ckaccountstatus#topics)

### [Account Statuses](/documentation/cloudkit/ckaccountstatus#Account-Statuses)

[`case available`](/documentation/cloudkit/ckaccountstatus/available)

The user’s iCloud account is available.

[`case couldNotDetermine`](/documentation/cloudkit/ckaccountstatus/couldnotdetermine)

CloudKit can’t determine the status of the user’s iCloud account.

[`case noAccount`](/documentation/cloudkit/ckaccountstatus/noaccount)

The device doesn’t have an iCloud account.

[`case restricted`](/documentation/cloudkit/ckaccountstatus/restricted)

The system denies access to the user’s iCloud account.

[`case temporarilyUnavailable`](/documentation/cloudkit/ckaccountstatus/temporarilyunavailable)

The user’s iCloud account is temporarily unavailable.

### [Initializers](/documentation/cloudkit/ckaccountstatus#Initializers)

[`init?(rawValue: Int)`](/documentation/cloudkit/ckaccountstatus/init\(rawvalue:\))

## [Relationships](/documentation/cloudkit/ckaccountstatus#relationships)

### [Conforms To](/documentation/cloudkit/ckaccountstatus#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

# CKContainer.Application | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   CKContainer.Application

Enumeration

# CKContainer.Application

A collection of types for app permissions.

```
enum Application
```

## [Topics](/documentation/cloudkit/ckcontainer/application#topics)

### [Container Application Types](/documentation/cloudkit/ckcontainer/application#Container-Application-Types)

[`typealias Permissions`](/documentation/cloudkit/ckcontainer/application/permissions)

A type that represents the permissions that a user grants.

[`typealias PermissionBlock`](/documentation/cloudkit/ckcontainer/application/permissionblock)

A type that represents a handler that processes the outcome of a permission’s request.

[`typealias PermissionStatus`](/documentation/cloudkit/ckcontainer/application/permissionstatus)

A type that represents the status of a permission.

## [See Also](/documentation/cloudkit/ckcontainer/application#see-also)

### [Requesting and Determining App Permissions](/documentation/cloudkit/ckcontainer/application#Requesting-and-Determining-App-Permissions)

[`func requestApplicationPermission(CKContainer.ApplicationPermissions, completionHandler: (CKContainer.ApplicationPermissionStatus, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/requestapplicationpermission\(_:completionhandler:\))

Prompts the user to authorize the specified permission.

Deprecated

[`func status(forApplicationPermission: CKContainer.ApplicationPermissions, completionHandler: (CKContainer.ApplicationPermissionStatus, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/status\(forapplicationpermission:completionhandler:\))

Determines the authorization status of the specified permission.

Deprecated

[`struct ApplicationPermissions`](/documentation/cloudkit/ckcontainer/applicationpermissions)

Constants that represent the permissions that a user grants.

[`typealias ApplicationPermissionBlock`](/documentation/cloudkit/ckcontainer/applicationpermissionblock)

A closure that processes the outcome of a permissions request.

Deprecated

[`enum ApplicationPermissionStatus`](/documentation/cloudkit/ckcontainer/applicationpermissionstatus)

Constants that represent the status of a permission.

Deprecated

Current page is CKContainer.Application

typealias Permissions = [`CKContainer`](https://developer.apple.com/documentation/cloudkit/ckcontainer).[`ApplicationPermissions`](https://developer.apple.com/documentation/cloudkit/ckcontainer/applicationpermissions)

typealias PermissionBlock = [`CKContainer`](https://developer.apple.com/documentation/cloudkit/ckcontainer).[`ApplicationPermissionBlock`](https://developer.apple.com/documentation/cloudkit/ckcontainer/applicationpermissionblock)

typealias PermissionStatus = [`CKContainer`](https://developer.apple.com/documentation/cloudkit/ckcontainer).[`ApplicationPermissionStatus`](https://developer.apple.com/documentation/cloudkit/ckcontainer/applicationpermissionstatus)

---
# CKContainer.ApplicationPermissions | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   CKContainer.ApplicationPermissions

Structure

# CKContainer.ApplicationPermissions

Constants that represent the permissions that a user grants.

```
struct ApplicationPermissions
```

## [Topics](/documentation/cloudkit/ckcontainer/applicationpermissions#topics)

### [Creating Permissions](/documentation/cloudkit/ckcontainer/applicationpermissions#Creating-Permissions)

[`init(rawValue: UInt)`](/documentation/cloudkit/ckcontainer/applicationpermissions/init\(rawvalue:\))

Creates a premission with the specified raw value.

### [Accessing Permissions](/documentation/cloudkit/ckcontainer/applicationpermissions#Accessing-Permissions)

[`static var userDiscoverability: CKContainer.ApplicationPermissions`](/documentation/cloudkit/ckcontainer/applicationpermissions/userdiscoverability)

The user is discoverable using their email address.

Deprecated

## [Relationships](/documentation/cloudkit/ckcontainer/applicationpermissions#relationships)

### [Conforms To](/documentation/cloudkit/ckcontainer/applicationpermissions#conforms-to)

-   [`BitwiseCopyable`](/documentation/Swift/BitwiseCopyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`ExpressibleByArrayLiteral`](/documentation/Swift/ExpressibleByArrayLiteral)
-   [`OptionSet`](/documentation/Swift/OptionSet)
-   [`RawRepresentable`](/documentation/Swift/RawRepresentable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)
-   [`SetAlgebra`](/documentation/Swift/SetAlgebra)

# add(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   add(\_:)

Instance Method

# add(\_:)

Adds an operation to the container’s queue.

```
func add(_ operation: CKOperation)
```

## [Parameters](/documentation/cloudkit/ckcontainer/add\(_:\)#parameters)

`operation`

The operation to add to the queue. Make sure you fully configure the operation and have it ready to execute. Don’t change the operation’s configuration after you queue it.

## [Discussion](/documentation/cloudkit/ckcontainer/add\(_:\)#Discussion)

This method adds the operation to a queue that the container manages. The queue’s operations execute on background threads concurrently, and with default priorities. When you add an operation to the queue, its container becomes the current container.

Current page is add(\_:)

---
### Discovering User Records

# fetchShareParticipant(withEmailAddress:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   fetchShareParticipant(withEmailAddress:completionHandler:)

Instance Method
# fetchShareParticipant(withEmailAddress:completionHandler:)

Fetches the share participant with the specified email address.

```
func fetchShareParticipant(
    withEmailAddress emailAddress: String,
    completionHandler: @escaping (CKShare.Participant?, (any Error)?) -> Void
)
```

```
func shareParticipant(forEmailAddress emailAddress: String) async throws -> CKShare.Participant
```

## [Parameters](/documentation/cloudkit/ckcontainer/fetchshareparticipant\(withemailaddress:completionhandler:\)#parameters)

`emailAddress`

The share participant’s email address.

`completionHandler`

The handler to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckcontainer/fetchshareparticipant\(withemailaddress:completionhandler:\)#Discussion)

The closure doesn’t return a value and takes the following parameters:

-   The share participant, or `nil` if CloudKit can’t find the participant.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully retrieves the participant.
    

This method searches for the share participant asynchronously and with a low priority. If you want the task to execute with a higher priority, create an instance of [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) and configure it to use the necessary priority.
# fetchShareParticipant(withPhoneNumber:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   fetchShareParticipant(withPhoneNumber:completionHandler:)

Instance Method
# fetchShareParticipant(withPhoneNumber:completionHandler:)

Fetches the share participant with the specified phone number.

```
func fetchShareParticipant(
    withPhoneNumber phoneNumber: String,
    completionHandler: @escaping (CKShare.Participant?, (any Error)?) -> Void
)
```

```
func shareParticipant(forPhoneNumber phoneNumber: String) async throws -> CKShare.Participant
```

## [Parameters](/documentation/cloudkit/ckcontainer/fetchshareparticipant\(withphonenumber:completionhandler:\)#parameters)

`phoneNumber`

The share participant’s phone number.

`completionHandler`

The handler to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckcontainer/fetchshareparticipant\(withphonenumber:completionhandler:\)#Discussion)

The closure doesn’t return a value and takes the following parameters:

-   The share participant, or `nil` if CloudKit can’t find the participant.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully retrieves the participant.
    

This method searches for the share participant asynchronously and with a low priority. If you want the task to execute with a higher priority, create an instance of [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) and configure it to use the necessary priority.
# fetchShareParticipant(withUserRecordID:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   fetchShareParticipant(withUserRecordID:completionHandler:)

Instance Method

# fetchShareParticipant(withUserRecordID:completionHandler:)

Fetches the share participant with the specified user record ID.

```
func fetchShareParticipant(
    withUserRecordID userRecordID: CKRecord.ID,
    completionHandler: @escaping (CKShare.Participant?, (any Error)?) -> Void
)
```

```
func shareParticipant(forUserRecordID userRecordID: CKRecord.ID) async throws -> CKShare.Participant
```

## [Parameters](/documentation/cloudkit/ckcontainer/fetchshareparticipant\(withuserrecordid:completionhandler:\)#parameters)

`userRecordID`

The share participant’s user record ID.

`completionHandler`

The handler to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckcontainer/fetchshareparticipant\(withuserrecordid:completionhandler:\)#Discussion)

The closure doesn’t return a value and takes the following parameters:

-   The share participant, or `nil` if CloudKit can’t find the participant.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully retrieves the participant.
    

This method searches for the share participant asynchronously and with a low priority. If you want the task to execute with a higher priority, create an instance of [`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) and configure it to use the necessary priority.

# fetchUserRecordID(completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   fetchUserRecordID(completionHandler:)

Instance Method

# fetchUserRecordID(completionHandler:)

Fetches the user record ID of the current user.

```
func fetchUserRecordID(completionHandler: @escaping (CKRecord.ID?, (any Error)?) -> Void)
```

```
func userRecordID() async throws -> CKRecord.ID
```

## [Parameters](/documentation/cloudkit/ckcontainer/fetchuserrecordid\(completionhandler:\)#parameters)

`completionHandler`

The handler to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckcontainer/fetchuserrecordid\(completionhandler:\)#Discussion)

The closure doesn’t return a value and takes the following parameters:

-   The user record ID, or `nil` if the user disables iCloud Drive or the device doesn’t have an iCloud account.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully retrieves the user record ID.
    

CloudKit returns a [`CKError.Code.notAuthenticated`](/documentation/cloudkit/ckerror/code/notauthenticated) error when any of the following conditions are met:

-   The device has an iCloud account but the user disables iCloud Drive.
-   The device has an iCloud account with restricted access.
-   The device doesn’t have an iCloud account.


### [Discovering User Records](/documentation/cloudkit/ckcontainer/fetchuserrecordid\(completionhandler:\)#Discovering-User-Records)

[`func discoverAllIdentities(completionHandler: ([CKUserIdentity]?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/discoverallidentities\(completionhandler:\))

Fetches all user identities that match entries in the user’s Contacts.

Deprecated

[`func discoverUserIdentity(withEmailAddress: String, completionHandler: (CKUserIdentity?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/discoveruseridentity\(withemailaddress:completionhandler:\))

Fetches the user identity for the specified email address.

Deprecated

[`func discoverUserIdentity(withPhoneNumber: String, completionHandler: (CKUserIdentity?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/discoveruseridentity\(withphonenumber:completionhandler:\))

Fetches the user identity for the specified phone number.

Deprecated

[`func discoverUserIdentity(withUserRecordID: CKRecord.ID, completionHandler: (CKUserIdentity?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/discoveruseridentity\(withuserrecordid:completionhandler:\))

Fetches the user identity for the specified user record ID.

Deprecated

[`func fetchShareParticipant(withEmailAddress: String, completionHandler: (CKShare.Participant?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/fetchshareparticipant\(withemailaddress:completionhandler:\))

Fetches the share participant with the specified email address.

[`func fetchShareParticipant(withPhoneNumber: String, completionHandler: (CKShare.Participant?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/fetchshareparticipant\(withphonenumber:completionhandler:\))

Fetches the share participant with the specified phone number.

[`func fetchShareParticipant(withUserRecordID: CKRecord.ID, completionHandler: (CKShare.Participant?, (any Error)?) -> Void)`](/documentation/cloudkit/ckcontainer/fetchshareparticipant\(withuserrecordid:completionhandler:\))

Fetches the share participant with the specified user record ID.

[`let CKCurrentUserDefaultName: String`](/documentation/cloudkit/ckcurrentuserdefaultname)

A constant that provides the current user’s default name.

[`let CKOwnerDefaultName: String`](/documentation/cloudkit/ckownerdefaultname)

A constant that provides the default owner’s name.

Deprecated

Current page is fetchUserRecordID(completionHandler:)

# CKCurrentUserDefaultName | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   CKCurrentUserDefaultName

Global Variable

# CKCurrentUserDefaultName

A constant that provides the current user’s default name.

```
let CKCurrentUserDefaultName: String
```
# fetchAllLongLivedOperationIDs(completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   fetchAllLongLivedOperationIDs(completionHandler:)

Instance Method

# fetchAllLongLivedOperationIDs(completionHandler:)

Fetches the IDs of any long-lived operations that are running.

```
@preconcurrency
func fetchAllLongLivedOperationIDs(completionHandler: @escaping ([CKOperation.ID]?, (any Error)?) -> Void)
```

## [Parameters](/documentation/cloudkit/ckcontainer/fetchalllonglivedoperationids\(completionhandler:\)#parameters)

`completionHandler`

The closure to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckcontainer/fetchalllonglivedoperationids\(completionhandler:\)#Discussion)

The closure doesn’t return a value and takes the following parameters:

-   The IDs of all of the long-lived operations that are running.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully retrieves the IDs.
    

A long-lived operation is one that continues to run after the user closes your app. When a long-lived operation completes, or your app or the system cancels it, it’s no longer active and CloudKit doesn’t include its ID in `outstandingOperationsByIDs`. An operation is complete when the system calls its completion handler.

Use the [`fetchLongLivedOperationWithID:completionHandler:`](/documentation/cloudkit/ckcontainer/fetchlonglivedoperationwithid:completionhandler:) method to fetch the operation for a specific ID.
# fetchLongLivedOperation(withID:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   fetchLongLivedOperation(withID:completionHandler:)

Instance Method

# fetchLongLivedOperation(withID:completionHandler:)

Fetches the long-lived operation for the specified operation ID.

```
@preconcurrency
func fetchLongLivedOperation(
    withID operationID: CKOperation.ID,
    completionHandler: @escaping (CKOperation?, (any Error)?) -> Void
)
```

## [Parameters](/documentation/cloudkit/ckcontainer/fetchlonglivedoperation\(withid:completionhandler:\)#parameters)

`operationID`

The operation’s ID.

`completionHandler`

The closure to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckcontainer/fetchlonglivedoperation\(withid:completionhandler:\)#Discussion)

The closure doesn’t return a value and takes the following parameters:

-   The long-lived operation. If the operation completes, or your app or the system cancels it, this parameter is `nil`.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully retrieves the operation.
    

A long-lived operation is one that continues to run after the user closes your app. When a long-lived operation completes, the system calls its completion block to notify you.
# fetchShareMetadata(with:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   fetchShareMetadata(with:completionHandler:)

Instance Method

# fetchShareMetadata(with:completionHandler:)

Fetches the share metadata for the specified share URL.

```
func fetchShareMetadata(
    with url: URL,
    completionHandler: @escaping (CKShare.Metadata?, (any Error)?) -> Void
)
```

```
func shareMetadata(for url: URL) async throws -> CKShare.Metadata
```

## [Parameters](/documentation/cloudkit/ckcontainer/fetchsharemetadata\(with:completionhandler:\)#parameters)

`url`

The share URL that CloudKit uses to locate the metadata.

`completionHandler`

The handler to execute with the fetch results.

## [Discussion](/documentation/cloudkit/ckcontainer/fetchsharemetadata\(with:completionhandler:\)#Discussion)

The closure doesn’t return a value and takes the following parameters:
-   The share metadata, or `nil` if CloudKit can’t find the metadata.
-   An error if a problem occurs, or `nil` if CloudKit successfully retrieves the metadata.
# accept(\_:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   accept(\_:completionHandler:)

Instance Method

# accept(\_:completionHandler:)

Accepts the specified share metadata.

```
func accept(
    _ metadata: CKShare.Metadata,
    completionHandler: @escaping (CKShare?, (any Error)?) -> Void
)
```

```
func accept(_ metadata: CKShare.Metadata) async throws -> CKShare
```

## [Parameters](/documentation/cloudkit/ckcontainer/accept\(_:completionhandler:\)-949ea#parameters)

`metadata`

The metadata of the share to accept.

`completionHandler`

The handler to execute when the process finishes.

## [Discussion](/documentation/cloudkit/ckcontainer/accept\(_:completionhandler:\)-949ea#Discussion)

The closure doesn’t return a value and takes the following parameters:

-   The correspinding share, or `nil` if CloudKit can’t accept the metadata.
    
-   An error if a problem occurs, or `nil` if CloudKit successfully accepts the metadata.
    
# CKAccountChanged | Apple Developer Documentation

-   [Foundation](/documentation/foundation)
-   [NSNotification](/documentation/foundation/nsnotification)
-   [NSNotification.Name](/documentation/foundation/nsnotification/name-swift.struct)
-   CKAccountChanged

Type Property

# CKAccountChanged

A notification that a container posts when the status of an iCloud account changes.

```
static let CKAccountChanged: NSNotification.Name
```

## [Discussion](/documentation/foundation/nsnotification/name-swift.struct/ckaccountchanged#Discussion)

Create an instance of [`CKContainer`](/documentation/CloudKit/CKContainer) to receive this notification. The container posts the notification using an arbitrary queue. Use the [`accountStatus(completionHandler:)`](/documentation/CloudKit/CKContainer/accountStatus\(completionHandler:\)) method to obtain the account’s status.

Current page is CKAccountChanged

---
  
Instance Methods

# accept(\_:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   accept(\_:)

Instance Method

# accept(\_:)

```
func accept(_ metadatas: [CKShare.Metadata]) async throws -> [CKShare.Metadata : Result<CKShare, any Error>]
```

Current page is accept(\_:)

# accept(\_:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   accept(\_:completionHandler:)

Instance Method

# accept(\_:completionHandler:)

```
@preconcurrency
func accept(
    _ metadatas: [CKShare.Metadata],
    completionHandler: @escaping (Result<[CKShare.Metadata : Result<CKShare, any Error>], any Error>) -> Void
)
```

Current page is accept(\_:completionHandler:)

# allLongLivedOperationIDs() | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   allLongLivedOperationIDs()

Instance Method

# allLongLivedOperationIDs()

```
func allLongLivedOperationIDs() async throws -> [CKOperation.ID]
```

Current page is allLongLivedOperationIDs()

# configuredWith(configuration:group:body:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   configuredWith(configuration:group:body:)

Instance Method

# configuredWith(configuration:group:body:)

```
@discardableResult
func configuredWith<R>(
    configuration: CKOperation.Configuration? = nil,
    group: CKOperationGroup? = nil,
    body: (CKContainer) throws -> R
) rethrows -> R
```

Current page is configuredWith(configuration:group:body:)

# configuredWith(configuration:group:body:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   configuredWith(configuration:group:body:)

Instance Method

# configuredWith(configuration:group:body:)

```
@discardableResult @preconcurrency
func configuredWith<R>(
    configuration: CKOperation.Configuration? = nil,
    group: CKOperationGroup? = nil,
    body: (CKContainer) async throws -> R
) async rethrows -> R
```

Current page is configuredWith(configuration:group:body:)

# fetchShareMetadatas(for:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   fetchShareMetadatas(for:completionHandler:)

Instance Method

# fetchShareMetadatas(for:completionHandler:)

```
@preconcurrency
func fetchShareMetadatas(
    for urls: [URL],
    completionHandler: @escaping (Result<[URL : Result<CKShare.Metadata, any Error>], any Error>) -> Void
)
```

Current page is fetchShareMetadatas(for:completionHandler:)

# fetchShareParticipants(forEmailAddresses:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   fetchShareParticipants(forEmailAddresses:completionHandler:)

Instance Method

# fetchShareParticipants(forEmailAddresses:completionHandler:)

```
@preconcurrency
func fetchShareParticipants(
    forEmailAddresses emails: [String],
    completionHandler: @escaping (Result<[String : Result<CKShare.Participant, any Error>], any Error>) -> Void
)
```

Current page is fetchShareParticipants(forEmailAddresses:completionHandler:)

# fetchShareParticipants(forUserRecordIDs:completionHandler:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   fetchShareParticipants(forUserRecordIDs:completionHandler:)

Instance Method

# fetchShareParticipants(forUserRecordIDs:completionHandler:)

```
@preconcurrency
func fetchShareParticipants(
    forUserRecordIDs userRecordIDs: [CKRecord.ID],
    completionHandler: @escaping (Result<[CKRecord.ID : Result<CKShare.Participant, any Error>], any Error>) -> Void
)
```

Current page is fetchShareParticipants(forUserRecordIDs:completionHandler:)

# longLivedOperation(for:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   longLivedOperation(for:)

Instance Method

# longLivedOperation(for:)

```
func longLivedOperation(for operationID: CKOperation.ID) async throws -> CKOperation?
```

Current page is longLivedOperation(for:)

# requestShareAccess(for:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   requestShareAccess(for:)

Instance Method

# requestShareAccess(for:)

Requests share access for the specified URLs.

```
func requestShareAccess(for urls: [URL]) async throws -> [URL : Result<Void, any Error>]
```

## [Discussion](/documentation/cloudkit/ckcontainer/requestshareaccess\(for:\)#discussion)

[`CKShareRequestAccessOperation`](/documentation/cloudkit/cksharerequestaccessoperation) is the more configurable, [`CKOperation`](/documentation/cloudkit/ckoperation)\-based alternative to this function

Current page is requestShareAccess(for:)

# shareMetadatas(for:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   shareMetadatas(for:)

Instance Method

# shareMetadatas(for:)

```
func shareMetadatas(for urls: [URL]) async throws -> [URL : Result<CKShare.Metadata, any Error>]
```

Current page is shareMetadatas(for:)

# shareParticipants(for:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   shareParticipants(for:)

Instance Method

# shareParticipants(for:)

Fetches share participants matching the provided lookup infos.

```
func shareParticipants(for lookupInfos: [CKUserIdentity.LookupInfo]) async throws -> [CKUserIdentity.LookupInfo : Result<CKShare.Participant, any Error>]
```

## [Discussion](/documentation/cloudkit/ckcontainer/shareparticipants\(for:\)#discussion)

[`CKFetchShareParticipantsOperation`](/documentation/cloudkit/ckfetchshareparticipantsoperation) is the more configurable, [`CKOperation`](/documentation/cloudkit/ckoperation)\-based alternative to this function

Current page is shareParticipants(for:)

# shareParticipants(forEmailAddresses:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   shareParticipants(forEmailAddresses:)

Instance Method

# shareParticipants(forEmailAddresses:)

```
func shareParticipants(forEmailAddresses emails: [String]) async throws -> [String : Result<CKShare.Participant, any Error>]
```

Current page is shareParticipants(forEmailAddresses:)

# shareParticipants(forUserRecordIDs:) | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [CKContainer](/documentation/cloudkit/ckcontainer)
-   shareParticipants(forUserRecordIDs:)

Instance Method

# shareParticipants(forUserRecordIDs:)

```
func shareParticipants(forUserRecordIDs userRecordIDs: [CKRecord.ID]) async throws -> [CKRecord.ID : Result<CKShare.Participant, any Error>]
```

Current page is shareParticipants(forUserRecordIDs:)


