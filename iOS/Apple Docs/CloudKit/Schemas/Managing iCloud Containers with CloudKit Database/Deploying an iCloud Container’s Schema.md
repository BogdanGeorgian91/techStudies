# Deploying an iCloud Container’s Schema | Apple Developer Documentation

-   [CloudKit](/documentation/cloudkit)
-   [Managing iCloud Containers with CloudKit Database App](/documentation/cloudkit/managing-icloud-containers-with-cloudkit-database-app)
-   Deploying an iCloud Container’s Schema

Article

# Deploying an iCloud Container’s Schema

Reset your container’s state during development and deploy your container’s schema to production.

## [Overview](/documentation/cloudkit/deploying-an-icloud-container-s-schema#overview)

During initial development of your app, you create your schema and add records for testing in the development environment. Apps in the App Store can access only the production environment. Before you publish your app, you must deploy the development schema to the production environment to copy over its record types, fields, and indexes.

As you continue to develop your app, you can add record types and fields to the development environment. To prevent conflicts, you can’t delete record types or fields that are already in production. Every time you deploy the development schema, its additive changes merge into the production schema. For testing purposes, your app in development can access either the development or the production environment.

![A diagram showing a development workflow for iterating a schema in the development environment before deploying it to the production environment.](https://docs-assets.developer.apple.com/published/125907201ea106ea204b12da247d319c/media-3562414.png)

### [Reset the development environment](/documentation/cloudkit/deploying-an-icloud-container-s-schema#Reset-the-development-environment)

You can reset the development environment in the CloudKit Database app between runs of your app to remove all records. If your schema isn’t in production, resetting the development environment also deletes all record types. Otherwise, the development schema reverts to the state of the production environment.

To reset the development environment to the current state of production:

1.  Sign in to CloudKit Console at [https://icloud.developer.apple.com/](https://icloud.developer.apple.com/).
    
2.  Select the CloudKit Database app.
    
3.  In the top section, choose your app’s container from the list.
    
4.  On the left of the screen, select Reset Environment.
    
5.  In the dialog that appears, read the warning, select the checkbox, and click Reset.
    

![A screenshot showing the details of a selected container in the CloudKit Database app. The screenshot highlights the Reset Environment menu item on the left.](https://docs-assets.developer.apple.com/published/39744312eb9d5a9e4b2cb40a1d84d9a3/media-3699106%402x.png)

### [Deploy the development schema](/documentation/cloudkit/deploying-an-icloud-container-s-schema#Deploy-the-development-schema)

After you finalize your schema and test your app in the development environment, you’re ready to deploy the schema to production in the CloudKit Database app. Deploying the schema copies its record types, fields, and indexes to the production environment, but doesn’t copy any records. After deployment, you can populate the production environment with records as necessary, and test your app there.

To deploy the development schema to production:

1.  Sign in to CloudKit Console at [https://icloud.developer.apple.com/](https://icloud.developer.apple.com/).
    
2.  Select the CloudKit Database app.
    
3.  In the top section, choose your container from the list.
    
4.  On the left, select Deploy Schema Changes.
    
5.  Review the pending deployment changes and click Deploy.
    

![A screenshot showing the details of a selected container in the CloudKit Database app. The screenshot highlights the Deploy Schema Changes menu item on the left.](https://docs-assets.developer.apple.com/published/1b7c0751f347b1886d15c760f3725967/media-3699097%402x.png)

## [See Also](/documentation/cloudkit/deploying-an-icloud-container-s-schema#see-also)

### [Container Management](/documentation/cloudkit/deploying-an-icloud-container-s-schema#Container-Management)

[

Inspecting and Editing an iCloud Container’s Schema](/documentation/cloudkit/inspecting-and-editing-an-icloud-container-s-schema)

Review and edit the schema for your app’s container using the CloudKit Database app.

[

Handling an iCloud Container’s Data](/documentation/cloudkit/handling-an-icloud-container-s-data)

Inspect and manage your app’s iCloud container data using the CloudKit Database app.

[

Obtaining an API Token for an iCloud Container](/documentation/cloudkit/obtaining-an-api-token-for-an-icloud-container)

Generate an API token to access CloudKit web services or use CloudKit JS.

Current page is Deploying an iCloud Container’s Schema