# Identifying an App’s Containers | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   Identifying an App’s Containers

Article

# Identifying an App’s Containers

Use Xcode’s Project navigator to find the identifiers of active CloudKit containers.

## [Overview](/documentation/cloudkit/identifying-an-app-s-containers#overview)

An app’s Xcode project manages which CloudKit containers are available to that app. When you write code that needs to provide container identifiers for all of the containers your app uses, reference the list of active containers in Xcode.

### [Identify the Containers Your App Uses](/documentation/cloudkit/identifying-an-app-s-containers#Identify-the-Containers-Your-App-Uses)

In your app’s Xcode project, select Signing & Capabilities > iCloud in the Project navigator.

![A screenshot of an Xcode project’s Signing & Capabilities pane. The project contains the iCloud capability with the CloudKit option in a selected state. There are three custom CloudKit containers — app, docs, and settings — and each is in a selected state.](https://docs-assets.developer.apple.com/published/834483e205503a63b239752195f2c377/media-3743337%402x.png)

After you identify the containers that your app uses, you can create instances of [`CKContainer`](/documentation/cloudkit/ckcontainer) in your app and interact with CloudKit data.

```
// These constants correspond to the containers you configure for your
// target in your project's Signing & Capabilities tab.
let app = CKContainer(identifier: "iCloud.com.example.MyCloudKitApp.app")
let docs = CKContainer(identifier: "iCloud.com.example.MyCloudKitApp.docs")
let settings = CKContainer(identifier: "iCloud.com.example.MyCloudKitApp.settings")
```

## [See Also](/documentation/cloudkit/identifying-an-app-s-containers#see-also)

### [Privacy](/documentation/cloudkit/identifying-an-app-s-containers#Privacy)

[

Encrypting User Data](/documentation/cloudkit/encrypting-user-data)

Deploy industry-standard security technologies using CloudKit encryption.

[

Providing User Access to CloudKit Data](/documentation/cloudkit/providing-user-access-to-cloudkit-data)

Provide users access to the data your app stores on their behalf.

[

Changing Access Controls on User Data](/documentation/cloudkit/changing-access-controls-on-user-data)

Restrict access to or remove restrictions from a user’s data at their request.

[`class CKFetchWebAuthTokenOperation`](/documentation/cloudkit/ckfetchwebauthtokenoperation)

An operation that creates an authentication token for use with CloudKit web services.

[

Responding to Requests to Delete Data](/documentation/cloudkit/responding-to-requests-to-delete-data)

Provide options for users to delete their CloudKit data from your app.

Current page is Identifying an App’s Containers