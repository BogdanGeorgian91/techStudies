# CKSharingSupported | Apple Developer Documentation

-   [Bundle Resources](/documentation/bundleresources)
-   [Information Property List](/documentation/bundleresources/information-property-list)
-   CKSharingSupported

Property List Key

# CKSharingSupported

A Boolean value that indicates your app supports CloudKit Sharing.

## [Details](/documentation/bundleresources/information-property-list/cksharingsupported#details)

Type

boolean

## [Discussion](/documentation/bundleresources/information-property-list/cksharingsupported#Discussion)

If your app supports CloudKit Sharing, add this key to your app’s `Info.plist` file with a value of [`true`](/documentation/swift/true). This tells the system to launch your app when the user taps or clicks a share’s URL. For example, one they receive in an email or an iMessage from the share’s owner.

Before your app launches, CloudKit verifies that the user has an active iCloud account and, for private shares, that it matches their participant details. Following successful verification, CloudKit provides the share’s metadata to your app’s scene, or application, delegate. The method it calls varies by platform and app configuration. For more information, see [`CKShare.Metadata`](/documentation/CloudKit/CKShare/Metadata).

To indicate that your app supports CloudKit Sharing:

1.  Select your project’s `Info.plist` file in the Project navigator in Xcode.
    
2.  Click the Add button (+) next to any key in the property list editor and press Return.
    
3.  Type the key name `CKSharingSupported`.
    
4.  Choose Boolean from the pop-up menu in the Type column.
    
5.  Choose YES from the pop-up menu in the Value column.
    
6.  Save your changes.
    

## [See Also](/documentation/bundleresources/information-property-list/cksharingsupported#see-also)

### [Network](/documentation/bundleresources/information-property-list/cksharingsupported#Network)

[`NSAdvertisingAttributionReportEndpoint`](/documentation/bundleresources/information-property-list/nsadvertisingattributionreportendpoint)

The URL where Private Click Measurement and SKAdNetwork send attribution information.

[`NSAppTransportSecurity`](/documentation/bundleresources/information-property-list/nsapptransportsecurity)

A description of changes made to the default security for HTTP connections.

**Name:** App Transport Security Settings

[`NSBonjourServices`](/documentation/bundleresources/information-property-list/nsbonjourservices)

Bonjour service types browsed by the app.

**Name:** Bonjour services

Current page is CKSharingSupported