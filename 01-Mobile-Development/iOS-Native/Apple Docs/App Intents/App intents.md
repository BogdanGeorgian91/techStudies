#  | Apple Developer Documentation

Collection

-   [App Intents](/documentation/appintents)
-   App intents

API Collection

# App intents

Define the custom actions your app exposes to the system, and incorporate support for existing SiriKit intents.

## [Overview](/documentation/appintents/app-intents#Overview)

Use app intents to express your app’s capabilities to the system. An app intent includes the code you need to perform an action, and expresses the data you require from the system. The system exposes your actions directly from the Shortcuts app and in system experiences like Siri.

To define an action, create a type that adopts the [`AppIntent`](/documentation/appintents/appintent) protocol, or a related protocol that provides the specific behavior you need. Annotate any key properties with the `@Parameter` property wrapper to let the system know you need the associated information to perform the action.

For more information about features App Intents enables, see [Making actions and content discoverable and widely available](/documentation/appintents/making-actions-and-content-discoverable-and-widely-available).

## [Topics](/documentation/appintents/app-intents#topics)

### [Actions](/documentation/appintents/app-intents#Actions)

[`protocol AppIntent`](/documentation/appintents/appintent)

An interface for providing an app-specific capability that people invoke from system experiences like Siri and the Shortcuts app.

[`protocol AudioPlaybackIntent`](/documentation/appintents/audioplaybackintent)

An App Intent that plays, pauses, or otherwise modifies audio playback state when it executes.

[`protocol AudioRecordingIntent`](/documentation/appintents/audiorecordingintent)

An app intent that starts, stops or otherwise modifies audio recording state.

[`protocol AudioStartingIntent`](/documentation/appintents/audiostartingintent)

An App Intent that plays, pauses, or otherwise modifies audio playback state when it executes.

Deprecated

[`protocol CameraCaptureIntent`](/documentation/appintents/cameracaptureintent)

Designates intent that will launch an activity that uses device’s camera to capture photos or videos. Marking your intent with this protocol makes it available as a possible action for Camera quick action.

[`protocol DeleteIntent`](/documentation/appintents/deleteintent)

Delete the associated entity(s).

[`protocol DeprecatedAppIntent`](/documentation/appintents/deprecatedappintent)

An app intent that marks an action as deprecated and informs people which action to use instead.

[`protocol ForegroundContinuableIntent`](/documentation/appintents/foregroundcontinuableintent)

A protocol you use for app intents which begin their work with the app in the background but may request to continue in the foreground.

Deprecated

[`protocol OpenIntent`](/documentation/appintents/openintent)

Open the associated item.

[`struct OpenURLIntent`](/documentation/appintents/openurlintent)

An intent that opens a universal link.

[`protocol PlayVideoIntent`](/documentation/appintents/playvideointent)

An intent that looks for videos based on a search term, then plays the content.

[`protocol ProgressReportingIntent`](/documentation/appintents/progressreportingintent)

An intent that reports progress to the system during its execution

[`protocol PushToTalkTransmissionIntent`](/documentation/appintents/pushtotalktransmissionintent)

An intent that begins or ends an audio transmission with the Push to Talk framework.

[`protocol URLRepresentableIntent`](/documentation/appintents/urlrepresentableintent)

An app intent with a URL representation.

[`protocol SetValueIntent`](/documentation/appintents/setvalueintent)

An intent that contains a value which can be set.

[`protocol ShowInAppSearchResultsIntent`](/documentation/appintents/showinappsearchresultsintent)

An app intent that takes a person to search results for a specified search term.

[`protocol SystemIntent`](/documentation/appintents/systemintent)

Designates intent types provided by App Intents.

### [Controls, widgets, and Live Activities](/documentation/appintents/app-intents#Controls-widgets-and-Live-Activities)

[`protocol ControlConfigurationIntent`](/documentation/appintents/controlconfigurationintent)

An interface for configuring a Control Center module.

[`protocol LiveActivityStartingIntent`](/documentation/appintents/liveactivitystartingintent)

An intent that starts, pauses, or otherwise modifies a Live Activity.

Deprecated

[`protocol LiveActivityIntent`](/documentation/appintents/liveactivityintent)

An intent that starts, pauses, or otherwise modifies a Live Activity when it runs.

[`protocol WidgetConfigurationIntent`](/documentation/appintents/widgetconfigurationintent)

An interface for configuring a WidgetKit widget.

### [Siri and Apple Intelligence](/documentation/appintents/app-intents#Siri-and-Apple-Intelligence)

[

Integrating actions with Siri and Apple Intelligence](/documentation/appintents/integrating-actions-with-siri-and-apple-intelligence)

Create app intents, entities, and enumerations that conform to assistant schemas to tap into the enhanced action capabilities of Siri and Apple Intelligence.

[

API Reference

App intent domains](/documentation/appintents/app-intent-domains)

Make your app’s actions and content available to Siri and Apple Intelligence with assistant schemas.

### [SiriKit intent migration](/documentation/appintents/app-intents#SiriKit-intent-migration)

[`protocol CustomIntentMigratedAppIntent`](/documentation/appintents/customintentmigratedappintent)

An interface for replacing a custom SiriKit intent that allows existing shortcuts and donations to continue working.

### [Dependency management](/documentation/appintents/app-intents#Dependency-management)

[`class AppDependencyManager`](/documentation/appintents/appdependencymanager)

An object that manages the registration and initialization of an app intent’s dependencies.

[`class AppDependency`](/documentation/appintents/appdependency)

A property wrapper that resolves a registered dependency at runtime.

### [Supplementary content](/documentation/appintents/app-intents#Supplementary-content)

[`protocol AppIntentsPackage`](/documentation/appintents/appintentspackage)

A type that describes app intent definitions that aren’t part of an app bundle and their dependencies.

[`struct IntentDescription`](/documentation/appintents/intentdescription)

The human-readable description and metadata for an app intent.

[`struct IntentDialog`](/documentation/appintents/intentdialog)

The text you want the system to display, or speak, when requesting a value, asking for disambiguation, or confirming an action.

[`struct IntentDeprecation`](/documentation/appintents/intentdeprecation)

[`class IntentProjection`](/documentation/appintents/intentprojection)

Projections for an app intent that returns non-optional values for parameters.

[`struct IntentSystemContext`](/documentation/appintents/intentsystemcontext)

Information that the system makes available to an app intent while it performs its action.

### [Results](/documentation/appintents/app-intents#Results)

[`protocol IntentResult`](/documentation/appintents/intentresult)

A type that contains the result of performing an action, and includes optional information to deliver back to the initiator.

[`struct IntentResultContainer`](/documentation/appintents/intentresultcontainer)

An object that represents the output of a completed intent.

[`protocol OpensIntent`](/documentation/appintents/opensintent)

The result of performing an action that delivers an app intent back to the initiator of the action.

[`protocol ProvidesDialog`](/documentation/appintents/providesdialog)

The result of performing an action that delivers a dialog back to the initiator of the action.

[`protocol ReturnsValue`](/documentation/appintents/returnsvalue)

The result of performing an action that delivers a value back to the initiator.

[`protocol ShowsSnippetView`](/documentation/appintents/showssnippetview)

The result of performing an action that delivers a view back to the initiator of the action.

[`protocol ResultsCollection`](/documentation/appintents/resultscollection)

A protocol representing a collection of returned items with support for sectioning.

### [Extensions](/documentation/appintents/app-intents#Extensions)

[`protocol AppIntentsExtension`](/documentation/appintents/appintentsextension)

An interface for managing an extension’s configuration.

---
# AppIntent | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   AppIntent

Protocol

# AppIntent

An interface for providing an app-specific capability that people invoke from system experiences like Siri and the Shortcuts app.

```
protocol AppIntent : PersistentlyIdentifiable, _SupportsAppDependencies, Sendable
```

## [Mentioned in](/documentation/appintents/appintent#mentions)

[

Making actions and content discoverable and widely available](/documentation/appintents/making-actions-and-content-discoverable-and-widely-available)

[

Responding to the Action button on Apple Watch Ultra](/documentation/appintents/actionbuttonarticle)

[

Creating your first app intent](/documentation/appintents/creating-your-first-app-intent)

[

Integrating actions with Siri and Apple Intelligence](/documentation/appintents/integrating-actions-with-siri-and-apple-intelligence)

[

Adding parameters to an app intent](/documentation/appintents/adding-parameters-to-an-app-intent)

## [Overview](/documentation/appintents/appintent#overview)

To expose your app’s functionality to system experiences like Siri or the Shortcuts app, and to support interactivity in widgets, you need to implement the `AppIntent` protocol. Use it to provide phrases that can launch the functionality, describe the needed data for the functionality you make available, and implement the method that performs the functionality.

The system instantiates an app intent you create parameter-less using the [`init()`](/documentation/appintents/appintent/init\(\)) initializer whenever a person invokes it through a system service like Siri, Shortcuts, and so on. If available, the system sets parameters based on user input or other available sources. With set parameters, the system attempts to resolve them in the order of their declaration in the `AppIntent` body. After it resolves all parameters, the system calls [`perform()`](/documentation/appintents/appintent/perform\(\)) to perform the app intent with its configured parameters. Note that the system retains the app intent and its output only for the duration of the invocation.

### [Implement the AppIntent Protocol](/documentation/appintents/appintent#Implement-the-AppIntent-Protocol)

Declare a custom intent type by defining a structure that conforms to the `AppIntent` protocol:

```
struct OrderSoupIntent: AppIntent {
   static var title = LocalizedStringResource("Order Soup")
   static var description = IntentDescription("Orders a soup from your favorite restaurant.")
}
```

Then, declare the AppIntent’s parameters. When you implement an `AppIntent` type, parameters must be declared with the `@Parameter` property wrapper. For more information about declaring parameters, see [Adding parameters to an app intent](/documentation/appintents/adding-parameters-to-an-app-intent).

```
struct OrderSoupIntent: AppIntent {
   @Parameter(title: "Soup")
   var soup: Soup


   @Parameter(title: "Quantity")
   var quantity: Int?
}
```

Next, implement the required [`perform()`](/documentation/appintents/appintent/perform\(\))function: Validate your intent’s parameters, execute the intent, and return an [`IntentResult`](/documentation/appintents/intentresult) that represents the output of a completed intent; for example, a [`PerformResult`](/documentation/appintents/appintent/performresult).

```
struct OrderSoupIntent: AppIntent {
    @Parameter(title: "Soup")
    var soup: Soup


    @Parameter(title: "Quantity")
    var quantity: Int?


    static var parameterSummary: some ParameterSummary {
        Summary("Order \(\.$soup)") {
            \.$quantity
        }
    }


    func perform() async throws -> some IntentResult {
        guard let quantity = quantity, quantity < 10 else {
            throw $quantity.needsValue
        }
        soup.order(quantity: quantity)
        return .result()
    }
}
```

## [Topics](/documentation/appintents/appintent#topics)

### [Creating an app intent](/documentation/appintents/appintent#Creating-an-app-intent)

[`init()`](/documentation/appintents/appintent/init\(\))

Creates an app intent.

**Required**

### [Specifying the authentication policy](/documentation/appintents/appintent#Specifying-the-authentication-policy)

[`static var authenticationPolicy: IntentAuthenticationPolicy`](/documentation/appintents/appintent/authenticationpolicy)

A property that defines the authentication policy that indicates whether this app intent requires the device to be unlocked or otherwise authenticated.

**Required** Default implementation provided.

[`enum IntentAuthenticationPolicy`](/documentation/appintents/intentauthenticationpolicy)

An enumeration that describes the authentication policy to use when running an app intent.

### [Configuring the metadata](/documentation/appintents/appintent#Configuring-the-metadata)

[`static var title: LocalizedStringResource`](/documentation/appintents/appintent/title)

A short, localized, human-readable string that describes the app intent using a verb and a noun in title case.

**Required** Default implementation provided.

[`static var description: IntentDescription?`](/documentation/appintents/appintent/description)

A description of the app intent that the system shows to people.

**Required** Default implementation provided.

[`static var openAppWhenRun: Bool`](/documentation/appintents/appintent/openappwhenrun)

A boolean property that tells the system to consider the app intent even if its app is not in the foreground.

**Required** Default implementations provided.

Deprecated

[`static var isDiscoverable: Bool`](/documentation/appintents/appintent/isdiscoverable)

A boolean value that determines whether system features such as Shortcuts and Spotlight can discover this app intent.

**Required** Default implementation provided.

### [Performing the action](/documentation/appintents/appintent#Performing-the-action)

[`func perform() async throws -> Self.PerformResult`](/documentation/appintents/appintent/perform\(\))

Performs the intent after resolving the provided parameters.

**Required** Default implementations provided.

[`var systemContext: IntentSystemContext`](/documentation/appintents/appintent/systemcontext)

Context information that’s available while the system performs the app intent’s action.

[`associatedtype PerformResult : IntentResult`](/documentation/appintents/appintent/performresult)

**Required**

### [Requesting confirmation](/documentation/appintents/appintent#Requesting-confirmation)

[`func requestConfirmation() async throws`](/documentation/appintents/appintent/requestconfirmation\(\))

Requests user confirmation before performing the app intent.

[`func requestConfirmation(conditions: ConfirmationConditions, actionName: ConfirmationActionName, dialog: IntentDialog) async throws`](/documentation/appintents/appintent/requestconfirmation\(conditions:actionname:dialog:\))

Requests user confirmation before performing the app intent.

[`func requestConfirmation<Content>(conditions: ConfirmationConditions, actionName: ConfirmationActionName, dialog: IntentDialog?, showDialogAsPrompt: Bool, content: () -> Content) async throws`](/documentation/appintents/appintent/requestconfirmation\(conditions:actionname:dialog:showdialogasprompt:content:\))

Request user confirmation before performing the app intent.

[`func requestConfirmation<Result>(result: Result, confirmationActionName: ConfirmationActionName, showPrompt: Bool) async throws`](/documentation/appintents/appintent/requestconfirmation\(result:confirmationactionname:showprompt:\))

Requests user confirmation before performing the app intent.

Deprecated

[`func requestConfirmation<Result>(output: Result, confirmationActionName: ConfirmationActionName, showPrompt: Bool) async throws`](/documentation/appintents/appintent/requestconfirmation\(output:confirmationactionname:showprompt:\))Deprecated

### [Donating the intent to the system](/documentation/appintents/appintent#Donating-the-intent-to-the-system)

[`func donate() async throws -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(\)-1e60c)

Donates the intent to the transcript.

[`func donate() -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(\)-jp6k)

Donates the intent to the transcript.

[`func donate(result: some IntentResult) async throws -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(result:\)-36cia)

Donates the intent and optional result to the transcript.

[`func donate(result: some IntentResult) -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(result:\)-9b25i)

Donates the intent and optional result to the transcript.

[`func callAsFunction(donate: Bool) async throws -> Self.PerformResult.Value`](/documentation/appintents/appintent/callasfunction\(donate:\)-3qvbt)

[`func callAsFunction(donate: Bool) async throws`](/documentation/appintents/appintent/callasfunction\(donate:\)-7v1om)

### [Summarizing the parameters](/documentation/appintents/appintent#Summarizing-the-parameters)

[`associatedtype SummaryContent : ParameterSummary`](/documentation/appintents/appintent/summarycontent)

The type of parameter summary representing this intent.

**Required**

[`static var parameterSummary: Self.SummaryContent`](/documentation/appintents/appintent/parametersummary)

Defines the summary of this intent in relation to how its parameters are populated.

**Required** Default implementation provided.

[`static var parameterSummary: some ParameterSummary`](/documentation/appintents/appintent/parametersummary-4vgic)

[`enum ParameterSummaryBuilder`](/documentation/appintents/parametersummarybuilder)

A result builder that allows you to declaratively describe a parameter summary.

[`typealias Parameter`](/documentation/appintents/appintent/parameter)

[`typealias Case`](/documentation/appintents/appintent/case)

[`typealias DefaultCase`](/documentation/appintents/appintent/defaultcase)

[`typealias Summary`](/documentation/appintents/appintent/summary)

[`typealias Switch`](/documentation/appintents/appintent/switch)

[`typealias When`](/documentation/appintents/appintent/when)

### [Instance Methods](/documentation/appintents/appintent#Instance-Methods)

[`func continueInForeground(IntentDialog?, alwaysConfirm: Bool) async throws`](/documentation/appintents/appintent/continueinforeground\(_:alwaysconfirm:\))

A method you call to ask a person to continue an action in the foreground.

[`func needsToContinueInForegroundError(IntentDialog?, alwaysConfirm: Bool) -> AppIntentError`](/documentation/appintents/appintent/needstocontinueinforegrounderror\(_:alwaysconfirm:\))

A method you call to ask a person to continue an intent’s action in the foreground after it encounters an error.

[`func requestChoice(between: [IntentChoiceOption], dialog: IntentDialog?) async throws -> IntentChoiceOption`](/documentation/appintents/appintent/requestchoice\(between:dialog:\))

Pauses the app intent to request a person to choose from several options.

[`func requestChoice<Content>(between: [IntentChoiceOption], dialog: IntentDialog?, content: () -> Content) async throws -> IntentChoiceOption`](/documentation/appintents/appintent/requestchoice\(between:dialog:content:\))

Pauses the app intent to request a person to choose from several options.

[`func requestChoice<Content>(between: [IntentChoiceOption], dialog: IntentDialog?, view: Content) async throws -> IntentChoiceOption`](/documentation/appintents/appintent/requestchoice\(between:dialog:view:\))

Pauses the app intent to request a person to choose from several options.

[`func requestConfirmation<Snippet>(conditions: ConfirmationConditions, actionName: ConfirmationActionName, dialog: IntentDialog?, showDialogAsPrompt: Bool, snippetIntent: Snippet) async throws`](/documentation/appintents/appintent/requestconfirmation\(conditions:actionname:dialog:showdialogasprompt:snippetintent:\)-3vewj)

[`func requestConfirmation<Snippet>(conditions: ConfirmationConditions, actionName: ConfirmationActionName, dialog: IntentDialog?, showDialogAsPrompt: Bool, snippetIntent: Snippet) async throws -> Snippet.PerformResult.Value`](/documentation/appintents/appintent/requestconfirmation\(conditions:actionname:dialog:showdialogasprompt:snippetintent:\)-jxb8)

Requests user confirmation before performing the app intent.

### [Type Aliases](/documentation/appintents/appintent#Type-Aliases)

[`typealias Option`](/documentation/appintents/appintent/option)

A convenience type alias that represents a choice option within the scope of an app intent.

### [Type Properties](/documentation/appintents/appintent#Type-Properties)

[`static var supportedModes: IntentModes`](/documentation/appintents/appintent/supportedmodes)

Defines the supported modes that describe the behavior of your app intent.

**Required** Default implementation provided.

## [Relationships](/documentation/appintents/appintent#relationships)

### [Inherits From](/documentation/appintents/appintent#inherits-from)

-   [`PersistentlyIdentifiable`](/documentation/appintents/persistentlyidentifiable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

### [Inherited By](/documentation/appintents/appintent#inherited-by)

-   [`AssistantIntent`](/documentation/appintents/assistantintent)
-   [`AssistantSchemaIntent`](/documentation/appintents/assistantschemaintent)
-   [`AudioPlaybackIntent`](/documentation/appintents/audioplaybackintent)
-   [`AudioRecordingIntent`](/documentation/appintents/audiorecordingintent)
-   [`AudioStartingIntent`](/documentation/appintents/audiostartingintent)
-   [`CameraCaptureIntent`](/documentation/appintents/cameracaptureintent)
-   [`ControlConfigurationIntent`](/documentation/appintents/controlconfigurationintent)
-   [`CustomIntentMigratedAppIntent`](/documentation/appintents/customintentmigratedappintent)
-   [`DeleteIntent`](/documentation/appintents/deleteintent)
-   [`DeprecatedAppIntent`](/documentation/appintents/deprecatedappintent)
-   [`ForegroundContinuableIntent`](/documentation/appintents/foregroundcontinuableintent)
-   [`LiveActivityIntent`](/documentation/appintents/liveactivityintent)
-   [`LiveActivityStartingIntent`](/documentation/appintents/liveactivitystartingintent)
-   [`OpenIntent`](/documentation/appintents/openintent)
-   [`PauseWorkoutIntent`](/documentation/appintents/pauseworkoutintent)
-   [`PlayVideoIntent`](/documentation/appintents/playvideointent)
-   [`PredictableIntent`](/documentation/appintents/predictableintent)
-   [`ProgressReportingIntent`](/documentation/appintents/progressreportingintent)
-   [`PushToTalkTransmissionIntent`](/documentation/appintents/pushtotalktransmissionintent)
-   [`ResumeWorkoutIntent`](/documentation/appintents/resumeworkoutintent)
-   [`SetFocusFilterIntent`](/documentation/appintents/setfocusfilterintent)
-   [`SetValueIntent`](/documentation/appintents/setvalueintent)
-   [`ShowInAppSearchResultsIntent`](/documentation/appintents/showinappsearchresultsintent)
-   [`SnippetIntent`](/documentation/appintents/snippetintent)
-   [`StartDiveIntent`](/documentation/appintents/startdiveintent)
-   [`StartWorkoutIntent`](/documentation/appintents/startworkoutintent)
-   [`SystemIntent`](/documentation/appintents/systemintent)
-   [`TargetContentProvidingIntent`](/documentation/appintents/targetcontentprovidingintent)
-   [`UISceneAppIntent`](/documentation/appintents/uisceneappintent)
-   [`URLRepresentableIntent`](/documentation/appintents/urlrepresentableintent)
-   [`UndoableIntent`](/documentation/appintents/undoableintent)
-   [`WidgetConfigurationIntent`](/documentation/appintents/widgetconfigurationintent)

### [Conforming Types](/documentation/appintents/appintent#conforming-types)

-   [`EmptySnippetIntent`](/documentation/appintents/emptysnippetintent)
-   [`OpenURLIntent`](/documentation/appintents/openurlintent)

## [See Also](/documentation/appintents/appintent#see-also)

### [Actions](/documentation/appintents/appintent#Actions)

[`protocol AudioPlaybackIntent`](/documentation/appintents/audioplaybackintent)

An App Intent that plays, pauses, or otherwise modifies audio playback state when it executes.

[`protocol AudioRecordingIntent`](/documentation/appintents/audiorecordingintent)

An app intent that starts, stops or otherwise modifies audio recording state.

[`protocol AudioStartingIntent`](/documentation/appintents/audiostartingintent)

An App Intent that plays, pauses, or otherwise modifies audio playback state when it executes.

Deprecated

[`protocol CameraCaptureIntent`](/documentation/appintents/cameracaptureintent)

Designates intent that will launch an activity that uses device’s camera to capture photos or videos. Marking your intent with this protocol makes it available as a possible action for Camera quick action.

[`protocol DeleteIntent`](/documentation/appintents/deleteintent)

Delete the associated entity(s).

[`protocol DeprecatedAppIntent`](/documentation/appintents/deprecatedappintent)

An app intent that marks an action as deprecated and informs people which action to use instead.

[`protocol ForegroundContinuableIntent`](/documentation/appintents/foregroundcontinuableintent)

A protocol you use for app intents which begin their work with the app in the background but may request to continue in the foreground.

Deprecated

[`protocol OpenIntent`](/documentation/appintents/openintent)

Open the associated item.

[`struct OpenURLIntent`](/documentation/appintents/openurlintent)

An intent that opens a universal link.

[`protocol PlayVideoIntent`](/documentation/appintents/playvideointent)

An intent that looks for videos based on a search term, then plays the content.

[`protocol ProgressReportingIntent`](/documentation/appintents/progressreportingintent)

An intent that reports progress to the system during its execution

[`protocol PushToTalkTransmissionIntent`](/documentation/appintents/pushtotalktransmissionintent)

An intent that begins or ends an audio transmission with the Push to Talk framework.

[`protocol URLRepresentableIntent`](/documentation/appintents/urlrepresentableintent)

An app intent with a URL representation.

[`protocol SetValueIntent`](/documentation/appintents/setvalueintent)

An intent that contains a value which can be set.

[`protocol ShowInAppSearchResultsIntent`](/documentation/appintents/showinappsearchresultsintent)

An app intent that takes a person to search results for a specified search term.

Current page is AppIntent

#   Creating an app intent

# init() | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   init()

Initializer

# init()

Creates an app intent.

```
init()
```

**Required**

Current page is init()

# Specifying the authentication policy
# authenticationPolicy | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   authenticationPolicy

Type Property

# authenticationPolicy

A property that defines the authentication policy that indicates whether this app intent requires the device to be unlocked or otherwise authenticated.

```
static var authenticationPolicy: IntentAuthenticationPolicy { get }
```

**Required** Default implementation provided.

## [Default Implementations](/documentation/appintents/appintent/authenticationpolicy#default-implementations)

### [AppIntent Implementations](/documentation/appintents/appintent/authenticationpolicy#AppIntent-Implementations)

[`static var authenticationPolicy: IntentAuthenticationPolicy`](/documentation/appintents/appintent/authenticationpolicy-1r9kh)

A property that defines the authentication policy that indicates whether this app intent requires the device to be unlocked or otherwise authenticated.

# IntentAuthenticationPolicy | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   IntentAuthenticationPolicy

Enumeration

# IntentAuthenticationPolicy

An enumeration that describes the authentication policy to use when running an app intent.

```
enum IntentAuthenticationPolicy
```

## [Topics](/documentation/appintents/intentauthenticationpolicy#topics)

### [Authentication policies](/documentation/appintents/intentauthenticationpolicy#Authentication-policies)

[`case alwaysAllowed`](/documentation/appintents/intentauthenticationpolicy/alwaysallowed)

A policy that allows the app intent to always run, even on a locked device.

[`case requiresAuthentication`](/documentation/appintents/intentauthenticationpolicy/requiresauthentication)

A policy that requires the user to authenticate.

[`case requiresLocalDeviceAuthentication`](/documentation/appintents/intentauthenticationpolicy/requireslocaldeviceauthentication)

A policy that requires the user to authenticate on the local device.

## [Relationships](/documentation/appintents/intentauthenticationpolicy#relationships)

### [Conforms To](/documentation/appintents/intentauthenticationpolicy#conforms-to)

-   [`Copyable`](/documentation/Swift/Copyable)
-   [`Equatable`](/documentation/Swift/Equatable)
-   [`Hashable`](/documentation/Swift/Hashable)
-   [`Sendable`](/documentation/Swift/Sendable)
-   [`SendableMetatype`](/documentation/Swift/SendableMetatype)

## [See Also](/documentation/appintents/intentauthenticationpolicy#see-also)

### [Specifying the authentication policy](/documentation/appintents/intentauthenticationpolicy#Specifying-the-authentication-policy)

[`static var authenticationPolicy: IntentAuthenticationPolicy`](/documentation/appintents/appintent/authenticationpolicy)

A property that defines the authentication policy that indicates whether this app intent requires the device to be unlocked or otherwise authenticated.

**Required** Default implementation provided.

Current page is IntentAuthenticationPolicy

  
# Configuring the metadata

# title | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   title

Type Property

# title

A short, localized, human-readable string that describes the app intent using a verb and a noun in title case.

```
static var title: LocalizedStringResource { get }
```

**Required** Default implementation provided.

## [Mentioned in](/documentation/appintents/appintent/title#mentions)

[

Responding to the Action button on Apple Watch Ultra](/documentation/appintents/actionbuttonarticle)

[

Creating your first app intent](/documentation/appintents/creating-your-first-app-intent)

## [Default Implementations](/documentation/appintents/appintent/title#default-implementations)

### [AppIntent Implementations](/documentation/appintents/appintent/title#AppIntent-Implementations)

[`static var title: LocalizedStringResource`](/documentation/appintents/appintent/title-4a26m)

A short, localized, human-readable string that describes the app intent using a verb and a noun in title case.

## [See Also](/documentation/appintents/appintent/title#see-also)

### [Configuring the metadata](/documentation/appintents/appintent/title#Configuring-the-metadata)

[`static var description: IntentDescription?`](/documentation/appintents/appintent/description)

A description of the app intent that the system shows to people.

**Required** Default implementation provided.

[`static var openAppWhenRun: Bool`](/documentation/appintents/appintent/openappwhenrun)

A boolean property that tells the system to consider the app intent even if its app is not in the foreground.

**Required** Default implementations provided.

Deprecated

[`static var isDiscoverable: Bool`](/documentation/appintents/appintent/isdiscoverable)

A boolean value that determines whether system features such as Shortcuts and Spotlight can discover this app intent.

**Required** Default implementation provided.

Current page is title

# description | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   description

Type Property

# description

A description of the app intent that the system shows to people.

```
static var description: IntentDescription? { get }
```

**Required** Default implementation provided.

## [Mentioned in](/documentation/appintents/appintent/description#mentions)

[

Creating your first app intent](/documentation/appintents/creating-your-first-app-intent)

## [Default Implementations](/documentation/appintents/appintent/description#default-implementations)

### [AppIntent Implementations](/documentation/appintents/appintent/description#AppIntent-Implementations)

[`static var description: IntentDescription?`](/documentation/appintents/appintent/description-9po8e)

A description of the app intent that the system shows to people.

## [See Also](/documentation/appintents/appintent/description#see-also)

### [Configuring the metadata](/documentation/appintents/appintent/description#Configuring-the-metadata)

[`static var title: LocalizedStringResource`](/documentation/appintents/appintent/title)

A short, localized, human-readable string that describes the app intent using a verb and a noun in title case.

**Required** Default implementation provided.

[`static var openAppWhenRun: Bool`](/documentation/appintents/appintent/openappwhenrun)

A boolean property that tells the system to consider the app intent even if its app is not in the foreground.

**Required** Default implementations provided.

Deprecated

[`static var isDiscoverable: Bool`](/documentation/appintents/appintent/isdiscoverable)

A boolean value that determines whether system features such as Shortcuts and Spotlight can discover this app intent.

**Required** Default implementation provided.

Current page is description

# isDiscoverable | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   isDiscoverable

Type Property

# isDiscoverable

A boolean value that determines whether system features such as Shortcuts and Spotlight can discover this app intent.

```
static var isDiscoverable: Bool { get }
```

**Required** Default implementation provided.

## [Discussion](/documentation/appintents/appintent/isdiscoverable#discussion)

App Intents must be discoverable to support App Shortcuts. If your app intent isn’t discoverable, people can use it only when it’s directly connected by a button in a SwiftUI app or a widget.

This property is `true` by default.

## [Default Implementations](/documentation/appintents/appintent/isdiscoverable#default-implementations)

### [AppIntent Implementations](/documentation/appintents/appintent/isdiscoverable#AppIntent-Implementations)

[`static var isDiscoverable: Bool`](/documentation/appintents/appintent/isdiscoverable-95nxm)

A boolean value that determines whether system features such as Shortcuts and Spotlight can discover this app intent.

## [See Also](/documentation/appintents/appintent/isdiscoverable#see-also)

### [Configuring the metadata](/documentation/appintents/appintent/isdiscoverable#Configuring-the-metadata)

[`static var title: LocalizedStringResource`](/documentation/appintents/appintent/title)

A short, localized, human-readable string that describes the app intent using a verb and a noun in title case.

**Required** Default implementation provided.

[`static var description: IntentDescription?`](/documentation/appintents/appintent/description)

A description of the app intent that the system shows to people.

**Required** Default implementation provided.

[`static var openAppWhenRun: Bool`](/documentation/appintents/appintent/openappwhenrun)

A boolean property that tells the system to consider the app intent even if its app is not in the foreground.

**Required** Default implementations provided.

Deprecated

Current page is isDiscoverable

#   Performing the action
# perform() | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   perform()

Instance Method

# perform()

Performs the intent after resolving the provided parameters.

```
func perform() async throws -> Self.PerformResult
```

**Required** Default implementations provided.

## [Mentioned in](/documentation/appintents/appintent/perform\(\)#mentions)

[

Responding to the Action button on Apple Watch Ultra](/documentation/appintents/actionbuttonarticle)

[

Creating your first app intent](/documentation/appintents/creating-your-first-app-intent)

[

Integrating custom data types into your intents](/documentation/appintents/integrating-custom-types-into-your-intents)

## [Discussion](/documentation/appintents/appintent/perform\(\)#discussion)

In the body of this function, validate your parameters and provide the system with information about needed parameter values or user clarification.

## [Default Implementations](/documentation/appintents/appintent/perform\(\)#default-implementations)

### [AppIntent Implementations](/documentation/appintents/appintent/perform\(\)#AppIntent-Implementations)

[`func perform() async throws -> Self.NeverResult`](/documentation/appintents/appintent/perform\(\)-2523c)

[`func perform() async throws -> Never`](/documentation/appintents/appintent/perform\(\)-28etm)

[`func perform() async throws -> Never`](/documentation/appintents/appintent/perform\(\)-5jtv1)

[`func perform() async throws -> some IntentResult`](/documentation/appintents/appintent/perform\(\)-1zvwi)

[`func perform() async throws -> some IntentResult`](/documentation/appintents/appintent/perform\(\)-5wt2l)

[`func perform() async throws -> Never`](/documentation/appintents/appintent/perform\(\)-pjg0)

[`func perform() async throws -> some IntentResult`](/documentation/appintents/appintent/perform\(\)-rp9j)

[`func perform() async throws -> Self.NeverResult`](/documentation/appintents/appintent/perform\(\)-39thw)

A default implementation that throws an error rather than perform an intent.

### [UISceneAppIntent Implementations](/documentation/appintents/appintent/perform\(\)#UISceneAppIntent-Implementations)

[`func perform() async throws -> some IntentResult`](/documentation/appintents/uisceneappintent/perform\(\)-s290)

[`func perform() async throws -> some IntentResult`](/documentation/appintents/uisceneappintent/perform\(\)-9wmom)

# systemContext | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   systemContext

Instance Property

# systemContext

Context information that’s available while the system performs the app intent’s action.

```
var systemContext: IntentSystemContext { get }
```

## [Discussion](/documentation/appintents/appintent/systemcontext#discussion)

Access information the system provides to your app intent while it performs its action in its [`perform()`](/documentation/appintents/appintent/perform\(\)) implementation. The provided information can vary and include information for each platform. For example, in watchOS, the intent system context includes a precise timestamp when a person started the app intent’s action using the Action button on Apple Watch Ultra.

# PerformResult | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   PerformResult

Associated Type

# PerformResult

```
associatedtype PerformResult : IntentResult
```

**Required**

#   Requesting confirmation

# requestConfirmation() | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   requestConfirmation()

Instance Method

# requestConfirmation()

Requests user confirmation before performing the app intent.

```
func requestConfirmation() async throws
```

## [See Also](/documentation/appintents/appintent/requestconfirmation\(\)#see-also)

### [Requesting confirmation](/documentation/appintents/appintent/requestconfirmation\(\)#Requesting-confirmation)

[`func requestConfirmation(conditions: ConfirmationConditions, actionName: ConfirmationActionName, dialog: IntentDialog) async throws`](/documentation/appintents/appintent/requestconfirmation\(conditions:actionname:dialog:\))

Requests user confirmation before performing the app intent.

[`func requestConfirmation<Content>(conditions: ConfirmationConditions, actionName: ConfirmationActionName, dialog: IntentDialog?, showDialogAsPrompt: Bool, content: () -> Content) async throws`](/documentation/appintents/appintent/requestconfirmation\(conditions:actionname:dialog:showdialogasprompt:content:\))

Request user confirmation before performing the app intent.

[`func requestConfirmation<Result>(result: Result, confirmationActionName: ConfirmationActionName, showPrompt: Bool) async throws`](/documentation/appintents/appintent/requestconfirmation\(result:confirmationactionname:showprompt:\))

Requests user confirmation before performing the app intent.

Deprecated

[`func requestConfirmation<Result>(output: Result, confirmationActionName: ConfirmationActionName, showPrompt: Bool) async throws`](/documentation/appintents/appintent/requestconfirmation\(output:confirmationactionname:showprompt:\))Deprecated

Current page is requestConfirmation()

# requestConfirmation(conditions:actionName:dialog:showDialogAsPrompt:content:) | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   requestConfirmation(conditions:actionName:dialog:showDialogAsPrompt:content:)

Instance Method

# requestConfirmation(conditions:actionName:dialog:showDialogAsPrompt:content:)

Request user confirmation before performing the app intent.

```
func requestConfirmation<Content>(
    conditions: ConfirmationConditions = [],
    actionName: ConfirmationActionName = .`continue`,
    dialog: IntentDialog? = nil,
    showDialogAsPrompt: Bool = true,
    @ViewBuilder content: () -> Content
) async throws where Content : View
```

## [Parameters](/documentation/appintents/appintent/requestconfirmation\(conditions:actionname:dialog:showdialogasprompt:content:\)#parameters)

`conditions`

The preconditions for requesting confirmation.

`actionName`

The name for the confirmation action.

`dialog`

The confirmation dialog.

`showDialogAsPrompt`

Flag indicating whether the confirmation dialog should be displayed as prompt text with the confirmation.

`content`

The `View` to display for confirmation.

## [See Also](/documentation/appintents/appintent/requestconfirmation\(conditions:actionname:dialog:showdialogasprompt:content:\)#see-also)

### [Requesting confirmation](/documentation/appintents/appintent/requestconfirmation\(conditions:actionname:dialog:showdialogasprompt:content:\)#Requesting-confirmation)

[`func requestConfirmation() async throws`](/documentation/appintents/appintent/requestconfirmation\(\))

Requests user confirmation before performing the app intent.

[`func requestConfirmation(conditions: ConfirmationConditions, actionName: ConfirmationActionName, dialog: IntentDialog) async throws`](/documentation/appintents/appintent/requestconfirmation\(conditions:actionname:dialog:\))

Requests user confirmation before performing the app intent.

[`func requestConfirmation<Result>(result: Result, confirmationActionName: ConfirmationActionName, showPrompt: Bool) async throws`](/documentation/appintents/appintent/requestconfirmation\(result:confirmationactionname:showprompt:\))

Requests user confirmation before performing the app intent.

Deprecated

[`func requestConfirmation<Result>(output: Result, confirmationActionName: ConfirmationActionName, showPrompt: Bool) async throws`](/documentation/appintents/appintent/requestconfirmation\(output:confirmationactionname:showprompt:\))Deprecated

Current page is requestConfirmation(conditions:actionName:dialog:showDialogAsPrompt:content:)

# Donating the intent to the system

# donate() | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   donate()

Instance Method

# donate()

Donates the intent to the transcript.

```
@discardableResult
func donate() async throws -> IntentDonationIdentifier
```

## [See Also](/documentation/appintents/appintent/donate\(\)-1e60c#see-also)

### [Donating the intent to the system](/documentation/appintents/appintent/donate\(\)-1e60c#Donating-the-intent-to-the-system)

[`func donate() -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(\)-jp6k)

Donates the intent to the transcript.

[`func donate(result: some IntentResult) async throws -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(result:\)-36cia)

Donates the intent and optional result to the transcript.

[`func donate(result: some IntentResult) -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(result:\)-9b25i)

Donates the intent and optional result to the transcript.

[`func callAsFunction(donate: Bool) async throws -> Self.PerformResult.Value`](/documentation/appintents/appintent/callasfunction\(donate:\)-3qvbt)

[`func callAsFunction(donate: Bool) async throws`](/documentation/appintents/appintent/callasfunction\(donate:\)-7v1om)

Current page is donate()

# donate() | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   donate()

Instance Method

# donate()

Donates the intent to the transcript.

```
@discardableResult
func donate() -> IntentDonationIdentifier
```

## [Discussion](/documentation/appintents/appintent/donate\(\)-jp6k#discussion)

This synchronous method is available to applications that haven’t adopted Swift async concurrency. The system ignores any exceptions encountered when donating this intent.

## [See Also](/documentation/appintents/appintent/donate\(\)-jp6k#see-also)

### [Donating the intent to the system](/documentation/appintents/appintent/donate\(\)-jp6k#Donating-the-intent-to-the-system)

[`func donate() async throws -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(\)-1e60c)

Donates the intent to the transcript.

[`func donate(result: some IntentResult) async throws -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(result:\)-36cia)

Donates the intent and optional result to the transcript.

[`func donate(result: some IntentResult) -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(result:\)-9b25i)

Donates the intent and optional result to the transcript.

[`func callAsFunction(donate: Bool) async throws -> Self.PerformResult.Value`](/documentation/appintents/appintent/callasfunction\(donate:\)-3qvbt)

[`func callAsFunction(donate: Bool) async throws`](/documentation/appintents/appintent/callasfunction\(donate:\)-7v1om)

Current page is donate()

# donate(result:) | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   donate(result:)

Instance Method

# donate(result:)

Donates the intent and optional result to the transcript.

```
@discardableResult
func donate(result: some IntentResult) async throws -> IntentDonationIdentifier
```

## [See Also](/documentation/appintents/appintent/donate\(result:\)-36cia#see-also)

### [Donating the intent to the system](/documentation/appintents/appintent/donate\(result:\)-36cia#Donating-the-intent-to-the-system)

[`func donate() async throws -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(\)-1e60c)

Donates the intent to the transcript.

[`func donate() -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(\)-jp6k)

Donates the intent to the transcript.

[`func donate(result: some IntentResult) -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(result:\)-9b25i)

Donates the intent and optional result to the transcript.

[`func callAsFunction(donate: Bool) async throws -> Self.PerformResult.Value`](/documentation/appintents/appintent/callasfunction\(donate:\)-3qvbt)

[`func callAsFunction(donate: Bool) async throws`](/documentation/appintents/appintent/callasfunction\(donate:\)-7v1om)

Current page is donate(result:)

# donate(result:) | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   donate(result:)

Instance Method

# donate(result:)

Donates the intent and optional result to the transcript.

```
@discardableResult
func donate(result: some IntentResult) -> IntentDonationIdentifier
```

## [Discussion](/documentation/appintents/appintent/donate\(result:\)-9b25i#discussion)

This synchronous method is available to applications that haven’t adopted Swift async concurrency. The system ignores any exceptions encountered when donating this intent.

## [See Also](/documentation/appintents/appintent/donate\(result:\)-9b25i#see-also)

### [Donating the intent to the system](/documentation/appintents/appintent/donate\(result:\)-9b25i#Donating-the-intent-to-the-system)

[`func donate() async throws -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(\)-1e60c)

Donates the intent to the transcript.

[`func donate() -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(\)-jp6k)

Donates the intent to the transcript.

[`func donate(result: some IntentResult) async throws -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(result:\)-36cia)

Donates the intent and optional result to the transcript.

[`func callAsFunction(donate: Bool) async throws -> Self.PerformResult.Value`](/documentation/appintents/appintent/callasfunction\(donate:\)-3qvbt)

[`func callAsFunction(donate: Bool) async throws`](/documentation/appintents/appintent/callasfunction\(donate:\)-7v1om)

Current page is donate(result:)

# callAsFunction(donate:) | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   callAsFunction(donate:)

Instance Method

# callAsFunction(donate:)

```
func callAsFunction(donate donateOnCompletion: Bool = true) async throws -> Self.PerformResult.Value where Self.PerformResult : ReturnsValue
```

## [See Also](/documentation/appintents/appintent/callasfunction\(donate:\)-3qvbt#see-also)

### [Donating the intent to the system](/documentation/appintents/appintent/callasfunction\(donate:\)-3qvbt#Donating-the-intent-to-the-system)

[`func donate() async throws -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(\)-1e60c)

Donates the intent to the transcript.

[`func donate() -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(\)-jp6k)

Donates the intent to the transcript.

[`func donate(result: some IntentResult) async throws -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(result:\)-36cia)

Donates the intent and optional result to the transcript.

[`func donate(result: some IntentResult) -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(result:\)-9b25i)

Donates the intent and optional result to the transcript.

[`func callAsFunction(donate: Bool) async throws`](/documentation/appintents/appintent/callasfunction\(donate:\)-7v1om)

Current page is callAsFunction(donate:)

# callAsFunction(donate:) | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   callAsFunction(donate:)

Instance Method

# callAsFunction(donate:)

```
func callAsFunction(donate donateOnCompletion: Bool = true) async throws where Self.PerformResult.Value == Never
```

## [See Also](/documentation/appintents/appintent/callasfunction\(donate:\)-7v1om#see-also)

### [Donating the intent to the system](/documentation/appintents/appintent/callasfunction\(donate:\)-7v1om#Donating-the-intent-to-the-system)

[`func donate() async throws -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(\)-1e60c)

Donates the intent to the transcript.

[`func donate() -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(\)-jp6k)

Donates the intent to the transcript.

[`func donate(result: some IntentResult) async throws -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(result:\)-36cia)

Donates the intent and optional result to the transcript.

[`func donate(result: some IntentResult) -> IntentDonationIdentifier`](/documentation/appintents/appintent/donate\(result:\)-9b25i)

Donates the intent and optional result to the transcript.

[`func callAsFunction(donate: Bool) async throws -> Self.PerformResult.Value`](/documentation/appintents/appintent/callasfunction\(donate:\)-3qvbt)

Current page is callAsFunction(donate:)

#   Summarizing the parameters
# SummaryContent | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   SummaryContent

Associated Type

# SummaryContent

The type of parameter summary representing this intent.

```
associatedtype SummaryContent : ParameterSummary
```

**Required**

## [Discussion](/documentation/appintents/appintent/summarycontent#discussion)

When you create an intent, Swift infers this type from your implementation of the [`parameterSummary`](/documentation/appintents/appintent/parametersummary) property.

## [See Also](/documentation/appintents/appintent/summarycontent#see-also)

### [Summarizing the parameters](/documentation/appintents/appintent/summarycontent#Summarizing-the-parameters)

[`static var parameterSummary: Self.SummaryContent`](/documentation/appintents/appintent/parametersummary)

Defines the summary of this intent in relation to how its parameters are populated.

**Required** Default implementation provided.

[`static var parameterSummary: some ParameterSummary`](/documentation/appintents/appintent/parametersummary-4vgic)

[`enum ParameterSummaryBuilder`](/documentation/appintents/parametersummarybuilder)

A result builder that allows you to declaratively describe a parameter summary.

[`typealias Parameter`](/documentation/appintents/appintent/parameter)

[`typealias Case`](/documentation/appintents/appintent/case)

[`typealias DefaultCase`](/documentation/appintents/appintent/defaultcase)

[`typealias Summary`](/documentation/appintents/appintent/summary)

[`typealias Switch`](/documentation/appintents/appintent/switch)

[`typealias When`](/documentation/appintents/appintent/when)

Current page is SummaryContent

# parameterSummary | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   parameterSummary

Type Property

# parameterSummary

Defines the summary of this intent in relation to how its parameters are populated.

```
static var parameterSummary: Self.SummaryContent { get }
```

**Required** Default implementation provided.

## [Mentioned in](/documentation/appintents/appintent/parametersummary#mentions)

[

Adding parameters to an app intent](/documentation/appintents/adding-parameters-to-an-app-intent)

## [Discussion](/documentation/appintents/appintent/parametersummary#discussion)

For more information about using parameters and providing a parameter summary, see [Provide an interactive parameter summary for your intent](/documentation/appintents/adding-parameters-to-an-app-intent#Provide-an-interactive-parameter-summary-for-your-intent).

## [Default Implementations](/documentation/appintents/appintent/parametersummary#default-implementations)

### [AppIntent Implementations](/documentation/appintents/appintent/parametersummary#AppIntent-Implementations)

[`static var parameterSummary: some ParameterSummary`](/documentation/appintents/appintent/parametersummary-4vgic)

# parameterSummary | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   parameterSummary

Type Property

# parameterSummary

```
static var parameterSummary: some ParameterSummary { get }
```

## [See Also](/documentation/appintents/appintent/parametersummary-4vgic#see-also)

### [Summarizing the parameters](/documentation/appintents/appintent/parametersummary-4vgic#Summarizing-the-parameters)

[`associatedtype SummaryContent : ParameterSummary`](/documentation/appintents/appintent/summarycontent)

The type of parameter summary representing this intent.

**Required**

[`static var parameterSummary: Self.SummaryContent`](/documentation/appintents/appintent/parametersummary)

Defines the summary of this intent in relation to how its parameters are populated.

**Required** Default implementation provided.

[`enum ParameterSummaryBuilder`](/documentation/appintents/parametersummarybuilder)

A result builder that allows you to declaratively describe a parameter summary.

[`typealias Parameter`](/documentation/appintents/appintent/parameter)

[`typealias Case`](/documentation/appintents/appintent/case)

[`typealias DefaultCase`](/documentation/appintents/appintent/defaultcase)

[`typealias Summary`](/documentation/appintents/appintent/summary)

[`typealias Switch`](/documentation/appintents/appintent/switch)

[`typealias When`](/documentation/appintents/appintent/when)

Current page is parameterSummary

# ParameterSummaryBuilder | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   ParameterSummaryBuilder

Enumeration

# ParameterSummaryBuilder

A result builder that allows you to declaratively describe a parameter summary.

```
@resultBuilder
enum ParameterSummaryBuilder<Intent> where Intent : AppIntent
```

## [Mentioned in](/documentation/appintents/parametersummarybuilder#mentions)

[

Adding parameters to an app intent](/documentation/appintents/adding-parameters-to-an-app-intent)

## [Overview](/documentation/appintents/parametersummarybuilder#overview)

For more information about using parameters and providing a parameter summary, see [Provide an interactive parameter summary for your intent](/documentation/appintents/adding-parameters-to-an-app-intent#Provide-an-interactive-parameter-summary-for-your-intent).

## [Topics](/documentation/appintents/parametersummarybuilder#topics)

### [Type Methods](/documentation/appintents/parametersummarybuilder#Type-Methods)

[`static func buildBlock<Summary>(Summary) -> Summary`](/documentation/appintents/parametersummarybuilder/buildblock\(_:\))

[`static func buildExpression<Summary>(Summary) -> Summary`](/documentation/appintents/parametersummarybuilder/buildexpression\(_:\))

## [See Also](/documentation/appintents/parametersummarybuilder#see-also)

### [Summarizing the parameters](/documentation/appintents/parametersummarybuilder#Summarizing-the-parameters)

[`associatedtype SummaryContent : ParameterSummary`](/documentation/appintents/appintent/summarycontent)

The type of parameter summary representing this intent.

**Required**

[`static var parameterSummary: Self.SummaryContent`](/documentation/appintents/appintent/parametersummary)

Defines the summary of this intent in relation to how its parameters are populated.

**Required** Default implementation provided.

[`static var parameterSummary: some ParameterSummary`](/documentation/appintents/appintent/parametersummary-4vgic)

[`typealias Parameter`](/documentation/appintents/appintent/parameter)

[`typealias Case`](/documentation/appintents/appintent/case)

[`typealias DefaultCase`](/documentation/appintents/appintent/defaultcase)

[`typealias Summary`](/documentation/appintents/appintent/summary)

[`typealias Switch`](/documentation/appintents/appintent/switch)

[`typealias When`](/documentation/appintents/appintent/when)

Current page is ParameterSummaryBuilder

# buildBlock(\_:) | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [ParameterSummaryBuilder](/documentation/appintents/parametersummarybuilder)
-   buildBlock(\_:)

Type Method

# buildBlock(\_:)

```
static func buildBlock<Summary>(_ block: Summary) -> Summary where Intent == Summary.Intent, Summary : ParameterSummary
```

Current page is buildBlock(\_:)

# buildExpression(\_:) | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [ParameterSummaryBuilder](/documentation/appintents/parametersummarybuilder)
-   buildExpression(\_:)

Type Method

# buildExpression(\_:)

```
static func buildExpression<Summary>(_ expression: Summary) -> Summary where Intent == Summary.Intent, Summary : ParameterSummary
```

Current page is buildExpression(\_:)

# AppIntent.Parameter | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   AppIntent.Parameter

Type Alias

# AppIntent.Parameter

```
typealias Parameter = IntentParameter
```

## [Mentioned in](/documentation/appintents/appintent/parameter#mentions)

[

Responding to the Action button on Apple Watch Ultra](/documentation/appintents/actionbuttonarticle)

## [See Also](/documentation/appintents/appintent/parameter#see-also)

### [Summarizing the parameters](/documentation/appintents/appintent/parameter#Summarizing-the-parameters)

[`associatedtype SummaryContent : ParameterSummary`](/documentation/appintents/appintent/summarycontent)

The type of parameter summary representing this intent.

**Required**

[`static var parameterSummary: Self.SummaryContent`](/documentation/appintents/appintent/parametersummary)

Defines the summary of this intent in relation to how its parameters are populated.

**Required** Default implementation provided.

[`static var parameterSummary: some ParameterSummary`](/documentation/appintents/appintent/parametersummary-4vgic)

[`enum ParameterSummaryBuilder`](/documentation/appintents/parametersummarybuilder)

A result builder that allows you to declaratively describe a parameter summary.

[`typealias Case`](/documentation/appintents/appintent/case)

[`typealias DefaultCase`](/documentation/appintents/appintent/defaultcase)

[`typealias Summary`](/documentation/appintents/appintent/summary)

[`typealias Switch`](/documentation/appintents/appintent/switch)

[`typealias When`](/documentation/appintents/appintent/when)

Current page is AppIntent.Parameter

# AppIntent.Case | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   AppIntent.Case

Type Alias

# AppIntent.Case

```
typealias Case = ParameterSummaryCaseCondition
```

## [See Also](/documentation/appintents/appintent/case#see-also)

### [Summarizing the parameters](/documentation/appintents/appintent/case#Summarizing-the-parameters)

[`associatedtype SummaryContent : ParameterSummary`](/documentation/appintents/appintent/summarycontent)

The type of parameter summary representing this intent.

**Required**

[`static var parameterSummary: Self.SummaryContent`](/documentation/appintents/appintent/parametersummary)

Defines the summary of this intent in relation to how its parameters are populated.

**Required** Default implementation provided.

[`static var parameterSummary: some ParameterSummary`](/documentation/appintents/appintent/parametersummary-4vgic)

[`enum ParameterSummaryBuilder`](/documentation/appintents/parametersummarybuilder)

A result builder that allows you to declaratively describe a parameter summary.

[`typealias Parameter`](/documentation/appintents/appintent/parameter)

[`typealias DefaultCase`](/documentation/appintents/appintent/defaultcase)

[`typealias Summary`](/documentation/appintents/appintent/summary)

[`typealias Switch`](/documentation/appintents/appintent/switch)

[`typealias When`](/documentation/appintents/appintent/when)

Current page is AppIntent.Case

# AppIntent.DefaultCase | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   AppIntent.DefaultCase

Type Alias

# AppIntent.DefaultCase

```
typealias DefaultCase = ParameterSummaryDefaultCaseCondition
```

# AppIntent.Summary | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   AppIntent.Summary

Type Alias

# AppIntent.Summary

```
typealias Summary = IntentParameterSummary<Self>
```

## [See Also](/documentation/appintents/appintent/summary#see-also)

### [Summarizing the parameters](/documentation/appintents/appintent/summary#Summarizing-the-parameters)

[`associatedtype SummaryContent : ParameterSummary`](/documentation/appintents/appintent/summarycontent)

The type of parameter summary representing this intent.

**Required**

[`static var parameterSummary: Self.SummaryContent`](/documentation/appintents/appintent/parametersummary)

Defines the summary of this intent in relation to how its parameters are populated.

**Required** Default implementation provided.

[`static var parameterSummary: some ParameterSummary`](/documentation/appintents/appintent/parametersummary-4vgic)

[`enum ParameterSummaryBuilder`](/documentation/appintents/parametersummarybuilder)

A result builder that allows you to declaratively describe a parameter summary.

[`typealias Parameter`](/documentation/appintents/appintent/parameter)

[`typealias Case`](/documentation/appintents/appintent/case)

[`typealias DefaultCase`](/documentation/appintents/appintent/defaultcase)

[`typealias Switch`](/documentation/appintents/appintent/switch)

[`typealias When`](/documentation/appintents/appintent/when)

Current page is AppIntent.Summary

# AppIntent.Switch | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   AppIntent.Switch

Type Alias

# AppIntent.Switch

```
typealias Switch<Value, CaseCondition> = ParameterSummarySwitchCondition<Self, Value, CaseCondition> where Value : _IntentValue, CaseCondition : _ParameterSummarySwitchCase
```

## [Mentioned in](/documentation/appintents/appintent/switch#mentions)

[

Adding parameters to an app intent](/documentation/appintents/adding-parameters-to-an-app-intent)

## [See Also](/documentation/appintents/appintent/switch#see-also)

### [Summarizing the parameters](/documentation/appintents/appintent/switch#Summarizing-the-parameters)

[`associatedtype SummaryContent : ParameterSummary`](/documentation/appintents/appintent/summarycontent)

The type of parameter summary representing this intent.

**Required**

[`static var parameterSummary: Self.SummaryContent`](/documentation/appintents/appintent/parametersummary)

Defines the summary of this intent in relation to how its parameters are populated.

**Required** Default implementation provided.

[`static var parameterSummary: some ParameterSummary`](/documentation/appintents/appintent/parametersummary-4vgic)

[`enum ParameterSummaryBuilder`](/documentation/appintents/parametersummarybuilder)

A result builder that allows you to declaratively describe a parameter summary.

[`typealias Parameter`](/documentation/appintents/appintent/parameter)

[`typealias Case`](/documentation/appintents/appintent/case)

[`typealias DefaultCase`](/documentation/appintents/appintent/defaultcase)

[`typealias Summary`](/documentation/appintents/appintent/summary)

[`typealias When`](/documentation/appintents/appintent/when)

Current page is AppIntent.Switch

# AppIntent.When | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   AppIntent.When

Type Alias

# AppIntent.When

```
typealias When = ParameterSummaryWhenCondition
```

## [Mentioned in](/documentation/appintents/appintent/when#mentions)

[

Adding parameters to an app intent](/documentation/appintents/adding-parameters-to-an-app-intent)

## [See Also](/documentation/appintents/appintent/when#see-also)

### [Summarizing the parameters](/documentation/appintents/appintent/when#Summarizing-the-parameters)

[`associatedtype SummaryContent : ParameterSummary`](/documentation/appintents/appintent/summarycontent)

The type of parameter summary representing this intent.

**Required**

[`static var parameterSummary: Self.SummaryContent`](/documentation/appintents/appintent/parametersummary)

Defines the summary of this intent in relation to how its parameters are populated.

**Required** Default implementation provided.

[`static var parameterSummary: some ParameterSummary`](/documentation/appintents/appintent/parametersummary-4vgic)

[`enum ParameterSummaryBuilder`](/documentation/appintents/parametersummarybuilder)

A result builder that allows you to declaratively describe a parameter summary.

[`typealias Parameter`](/documentation/appintents/appintent/parameter)

[`typealias Case`](/documentation/appintents/appintent/case)

[`typealias DefaultCase`](/documentation/appintents/appintent/defaultcase)

[`typealias Summary`](/documentation/appintents/appintent/summary)

[`typealias Switch`](/documentation/appintents/appintent/switch)

Current page is AppIntent.When

#   Instance Methods
# continueInForeground(\_:alwaysConfirm:) | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   continueInForeground(\_:alwaysConfirm:)

Instance Method

# continueInForeground(\_:alwaysConfirm:)

A method you call to ask a person to continue an action in the foreground.

```
func continueInForeground(
    _ dialog: IntentDialog? = nil,
    alwaysConfirm: Bool = true
) async throws
```

## [Discussion](/documentation/appintents/appintent/continueinforeground\(_:alwaysconfirm:\)#discussion)

To update your app’s state after a person permits the action to continue execution in the foreground, provide an optional continuation closure that the system executes on the main thread. The system passes the result of the closure back to the function’s caller.

Current page is continueInForeground(\_:alwaysConfirm:)

# needsToContinueInForegroundError(\_:alwaysConfirm:) | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   needsToContinueInForegroundError(\_:alwaysConfirm:)

Instance Method

# needsToContinueInForegroundError(\_:alwaysConfirm:)

A method you call to ask a person to continue an intent’s action in the foreground after it encounters an error.

```
func needsToContinueInForegroundError(
    _ dialog: IntentDialog? = nil,
    alwaysConfirm: Bool = true
) -> AppIntentError
```

## [Discussion](/documentation/appintents/appintent/needstocontinueinforegrounderror\(_:alwaysconfirm:\)#discussion)

Call this method when you need to stop performing the app intent and ask a person to continue execution in the foreground. Provide an optional continuation closure that runs on the main thread to update your app’s state after the person permits the action to continue in the foreground.

Current page is needsToContinueInForegroundError(\_:alwaysConfirm:)

# requestChoice(between:dialog:) | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   requestChoice(between:dialog:)

Instance Method

# requestChoice(between:dialog:)

Pauses the app intent to request a person to choose from several options.

```
func requestChoice(
    between options: [IntentChoiceOption],
    dialog: IntentDialog? = nil
) async throws -> IntentChoiceOption
```

## [Parameters](/documentation/appintents/appintent/requestchoice\(between:dialog:\)#parameters)

`options`

An array of options to choose from. The order of options in this array determines the order of options in the UI, excluding the option to cancel the choice. The system automatically handles the placement of the cancel option according to platform conventions.

`dialog`

Instructional text or a question to help the person to choose an option.

## [Return Value](/documentation/appintents/appintent/requestchoice\(between:dialog:\)#return-value)

The value that represents the person’s selection.

## [Discussion](/documentation/appintents/appintent/requestchoice\(between:dialog:\)#discussion)

Call this method in your app intent’s [`perform()`](/documentation/appintents/appintent/perform\(\)) method when you need a person to confirm an action, disambiguate between possibilities, or select a specific variation of the intent’s behavior before proceeding.

The system presents a standard interface — for example, a modal sheet or alert — using the provided `dialog`, the `content` view for context, and buttons for each [`IntentChoiceOption`](/documentation/appintents/intentchoiceoption). The app intent’s execution resumes only after the person selects an option.

Current page is requestChoice(between:dialog:)

# requestChoice(between:dialog:content:) | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   requestChoice(between:dialog:content:)

Instance Method

# requestChoice(between:dialog:content:)

Pauses the app intent to request a person to choose from several options.

```
func requestChoice<Content>(
    between options: [IntentChoiceOption],
    dialog: IntentDialog? = nil,
    @ViewBuilder content: () -> Content
) async throws -> IntentChoiceOption where Content : View
```

## [Parameters](/documentation/appintents/appintent/requestchoice\(between:dialog:content:\)#parameters)

`options`

An array of options to choose from. The order of options in this array determines the order of options in the UI, excluding the option to cancel the choice. The system automatically handles the placement of the cancel option according to platform conventions.

`dialog`

Instructional text or a question to help the person to choose an option.

`content`

A closure that provides a view that appears next to options, offering visual context that’s relevant to the decision; for example, the closure could show a preview of data.

## [Return Value](/documentation/appintents/appintent/requestchoice\(between:dialog:content:\)#return-value)

The value that represents the person’s selection.

## [Discussion](/documentation/appintents/appintent/requestchoice\(between:dialog:content:\)#discussion)

Call this method in your app intent’s [`perform()`](/documentation/appintents/appintent/perform\(\)) method when you need a person to confirm an action, disambiguate between possibilities, or select a specific variation of the intent’s behavior before proceeding.

The system presents a standard interface — for example, a modal sheet or alert — using the provided `dialog`, the `content` view for context, and buttons for each [`IntentChoiceOption`](/documentation/appintents/intentchoiceoption). The app intent’s execution resumes only after the person selects an option.

Current page is requestChoice(between:dialog:content:)

# requestChoice(between:dialog:view:) | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   requestChoice(between:dialog:view:)

Instance Method

# requestChoice(between:dialog:view:)

Pauses the app intent to request a person to choose from several options.

```
func requestChoice<Content>(
    between options: [IntentChoiceOption],
    dialog: IntentDialog? = nil,
    view: Content
) async throws -> IntentChoiceOption where Content : View
```

## [Parameters](/documentation/appintents/appintent/requestchoice\(between:dialog:view:\)#parameters)

`options`

An array of options to choose from. The order of options in this array determines the order of options in the UI, excluding the option to cancel the choice. The system automatically handles the placement of the cancel option according to platform conventions.

`dialog`

Instructional text or a question to help the person to choose an option.

`view`

A SwiftUI view that appears next to options, offering visual context that’s relevant to the decision; for example, the closure could show a preview of data.

## [Return Value](/documentation/appintents/appintent/requestchoice\(between:dialog:view:\)#return-value)

The value that represents the person’s selection.

## [Discussion](/documentation/appintents/appintent/requestchoice\(between:dialog:view:\)#discussion)

Call this method in your app intent’s [`perform()`](/documentation/appintents/appintent/perform\(\)) method when you need a person to confirm an action, disambiguate between possibilities, or select a specific variation of the intent’s behavior before proceeding.

The system presents a standard interface — for example, a modal sheet or alert — using the provided `dialog`, the `content` view for context, and buttons for each [`IntentChoiceOption`](/documentation/appintents/intentchoiceoption). The app intent’s execution resumes only after the person selects an option.

Current page is requestChoice(between:dialog:view:)

# requestConfirmation(conditions:actionName:dialog:showDialogAsPrompt:snippetIntent:) | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   requestConfirmation(conditions:actionName:dialog:showDialogAsPrompt:snippetIntent:)

Instance Method

# requestConfirmation(conditions:actionName:dialog:showDialogAsPrompt:snippetIntent:)

```
func requestConfirmation<Snippet>(
    conditions: ConfirmationConditions = [],
    actionName: ConfirmationActionName = .`continue`,
    dialog: IntentDialog? = nil,
    showDialogAsPrompt: Bool = true,
    snippetIntent: Snippet
) async throws where Snippet : SnippetIntent
```

## [Mentioned in](/documentation/appintents/appintent/requestconfirmation\(conditions:actionname:dialog:showdialogasprompt:snippetintent:\)-3vewj#mentions)

[

Displaying static and interactive snippets](/documentation/appintents/displaying-static-and-interactive-snippets)

Current page is requestConfirmation(conditions:actionName:dialog:showDialogAsPrompt:snippetIntent:)

# requestConfirmation(conditions:actionName:dialog:showDialogAsPrompt:snippetIntent:) | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   requestConfirmation(conditions:actionName:dialog:showDialogAsPrompt:snippetIntent:)

Instance Method

# requestConfirmation(conditions:actionName:dialog:showDialogAsPrompt:snippetIntent:)

Requests user confirmation before performing the app intent.

```
@discardableResult
func requestConfirmation<Snippet>(
    conditions: ConfirmationConditions = [],
    actionName: ConfirmationActionName = .`continue`,
    dialog: IntentDialog? = nil,
    showDialogAsPrompt: Bool = true,
    snippetIntent: Snippet
) async throws -> Snippet.PerformResult.Value where Snippet : SnippetIntent, Snippet.PerformResult : ReturnsValue
```

## [Parameters](/documentation/appintents/appintent/requestconfirmation\(conditions:actionname:dialog:showdialogasprompt:snippetintent:\)-jxb8#parameters)

`conditions`

The preconditions for requesting user confirmation.

`actionName`

The name for the confirmation action.

`dialog`

The confirmation dialog.

`showDialogAsPrompt`

A flag that indicates whether the confirmation dialog should appear as prompt text with the confirmation.

`snippetIntent`

The intent responsible for presenting a snippet for this confirmation.

## [Return Value](/documentation/appintents/appintent/requestconfirmation\(conditions:actionname:dialog:showdialogasprompt:snippetintent:\)-jxb8#return-value)

The returned value of the Snippet Intent Perform Result

## [Discussion](/documentation/appintents/appintent/requestconfirmation\(conditions:actionname:dialog:showdialogasprompt:snippetintent:\)-jxb8#discussion)

The function displays an interactive snippet as a result of the app intent if a person confirms the action and otherwise throws an error.

Current page is requestConfirmation(conditions:actionName:dialog:showDialogAsPrompt:snippetIntent:)

# AppIntent.Option | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   AppIntent.Option

Type Alias

# AppIntent.Option

A convenience type alias that represents a choice option within the scope of an app intent.

```
typealias Option = IntentChoiceOption
```

Current page is AppIntent.Option

# supportedModes | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   [AppIntent](/documentation/appintents/appintent)
-   supportedModes

Type Property

# supportedModes

Defines the supported modes that describe the behavior of your app intent.

```
static var supportedModes: IntentModes { get }
```

**Required** Default implementation provided.

## [Discussion](/documentation/appintents/appintent/supportedmodes#discussion)

The suppprted modes determine whether the intent performs its action in the background, the foreground, or a combination of both. Available modes include:

`.background`

The intent performs entirely in the background without foregrounding the app. This mode results in the same behavior as setting [`openAppWhenRun`](/documentation/appintents/appintent/openappwhenrun) to `false`.

`.foreground(.immediate)`

The intent foregrounds the app immediately after parameter resolution and before [`perform()`](/documentation/appintents/appintent/perform\(\)) runs. This mode results in the same behavior as setting [`openAppWhenRun`](/documentation/appintents/appintent/openappwhenrun) to `true`.

`.foreground(.dynamic)`

The app intent to can foreground the app during execution based on runtime conditions. This mode is results in the same behavior as creating an app intent that conforms to [`ForegroundContinuableIntent`](/documentation/appintents/foregroundcontinuableintent).

`.foreground(.deferred)`

Enables the app to perform in the background initially, with the guarantee that the app will be foregrounded either by you within [`perform()`](/documentation/appintents/appintent/perform\(\)) or by the system before returning from [`perform()`](/documentation/appintents/appintent/perform\(\)).

Additionally, you can set `supportedModes` to a combination of background and foreground modes:

`[.background, .foreground]` or `[.background, .foreground(.immediate)]`

Allows the app intent to perform its action in the foreground per default and to perform it in the background as a fallback when it can’t run in the foreground.

`[.background, .foreground(.dynamic)]`

The app intent can perform its action with the app in the foreground or in the background, defaulting to running it with the app in the background. In your [`perform()`](/documentation/appintents/appintent/perform\(\)) implementation, you can request to open your app in the foreground with the system determining whether or not it can grant the request.

`[.background, .foreground(.deferred)]`

The app intent can perform its action with the app in the background and the foreground execution. Per default, the system runs the action in the background and the system guarantees to bring your app to the foreground when you request it in your [`perform()`](/documentation/appintents/appintent/perform\(\)) implementation or automatically before it returns the result of the `perform()` method.

When handling both background and foreground modes you can consult `SystemContext`’s `SystemContext/currentMode` property within your [`perform()`](/documentation/appintents/appintent/perform\(\)) implementation to determine the active mode and decide whether foregrounding your app is appropriate.

## [Default Implementations](/documentation/appintents/appintent/supportedmodes#default-implementations)

### [AppIntent Implementations](/documentation/appintents/appintent/supportedmodes#AppIntent-Implementations)

[`static var supportedModes: IntentModes`](/documentation/appintents/appintent/supportedmodes-5zhmb)

Defines the supported modes that describe the behavior of your app intent.

Current page is supportedModes

# IntentChoiceOption | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   IntentChoiceOption

Structure

# IntentChoiceOption

A structure representing an entry in a list of options for a person to choose from before an app intent resumes its action.

```
struct IntentChoiceOption
```

## [Overview](/documentation/appintents/intentchoiceoption#overview)

Each option includes display text and an optional style that influences its visual presentation. For example, an option for deleting a file might use a text and style to communicate its destructive action.

## [Topics](/documentation/appintents/intentchoiceoption#topics)

### [Structures](/documentation/appintents/intentchoiceoption#Structures)

[`struct Style`](/documentation/appintents/intentchoiceoption/style-swift.struct)

Defines the visual style and semantic meaning of an [`IntentChoiceOption`](/documentation/appintents/intentchoiceoption).

### [Initializers](/documentation/appintents/intentchoiceoption#Initializers)

[`init(title: LocalizedStringResource, style: IntentChoiceOption.Style)`](/documentation/appintents/intentchoiceoption/init\(title:style:\))

Creates a new option for a person to choose to continue an app intent.

### [Instance Properties](/documentation/appintents/intentchoiceoption#Instance-Properties)

[`let style: IntentChoiceOption.Style`](/documentation/appintents/intentchoiceoption/style-swift.property)

The style applied to the option, affecting its visual appearance in the system UI.

[`let title: LocalizedStringResource`](/documentation/appintents/intentchoiceoption/title)

The localized text displayed for this option.

### [Type Properties](/documentation/appintents/intentchoiceoption#Type-Properties)

[`static var cancel: IntentChoiceOption`](/documentation/appintents/intentchoiceoption/cancel)

A system-provided option that cancel the app intent.

## [Relationships](/documentation/appintents/intentchoiceoption#relationships)

### [Conforms To](/documentation/appintents/intentchoiceoption#conforms-to)

-   [`Equatable`](/documentation/Swift/Equatable)

Current page is IntentChoiceOption