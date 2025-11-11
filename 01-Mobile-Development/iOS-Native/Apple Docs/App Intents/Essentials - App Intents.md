# Making actions and content discoverable and widely available | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   Making actions and content discoverable and widely available

Article

# Making actions and content discoverable and widely available

Adopt App Intents to make your app discoverable with Spotlight, controls, widgets, and the Action button.

## [Overview](/documentation/appintents/making-actions-and-content-discoverable-and-widely-available#Overview)

The App Intents framework offers functionality to express your app’s actions and data in a way that enables deep integration with system capabilities Apple Intelligence provides and system experiences like Spotlight. Use App Intents to enable people to view your app’s content and to use its actions when and where they need them — whether they’re using your app or are elsewhere in the system.

The App Intents API is a fundamental framework that facilitates deep integration with system experiences across platforms and devices. You use the framework to express data and actions once to build a reusable foundation for many experiences and features. For example, use App Intents to integrate your app with Siri and Apple Intelligence, then reuse the code to create controls and interactive widgets in combination with [WidgetKit](/documentation/WidgetKit).

### [Review experiences that App Intents enables directly](/documentation/appintents/making-actions-and-content-discoverable-and-widely-available#Review-experiences-that-App-Intents-enables-directly)

When you use the App Intents framework to express your app’s actions and data, you integrate your app with system experiences that offer broad visibility for your app and content and make its functionality available outside of the app itself; for example:

-   People will use Siri to perform app actions.
    
-   People find App Shortcuts you create in the Shortcuts app and initiate them throughout the system, across platforms and devices with Siri, Spotlight, the Action button, Apple Pencil Pro, and more.
    
-   Using the Shortcuts app, people create custom shortcuts with your app’s functionality and entirely new workflows across apps.
    
-   People reduce distractions with Focus, and you use the App Intents framework to respond to Focus changes.
    

On supported devices, the App Intents framework will provide integration with Apple Intelligence, a personal intelligence system that deeply integrates powerful generative models into the core of iPhone, iPad, and Mac. Siri will draw on the capabilities of Apple Intelligence to deliver assistance that’s natural, contextually relevant, and personal for everyone, including in the apps they use every day. The App Intents framework will enable you to express your app’s capabilities and content, giving the system access to this context and integrating your app with Siri and Apple Intelligence, and unlocking new ways for people to interact with it from anywhere on their device. For more information, refer to [Integrating actions with Siri and Apple Intelligence](/documentation/appintents/integrating-actions-with-siri-and-apple-intelligence) and [Making onscreen content available to Siri and Apple Intelligence](/documentation/appintents/making-onscreen-content-available-to-siri-and-apple-intelligence).

### [Understand experiences that use App Intents API](/documentation/appintents/making-actions-and-content-discoverable-and-widely-available#Understand-experiences-that-use-App-Intents-API)

Across devices, your app’s content and actions appear in additional system experiences you create with a combination of the App Intents framework and other frameworks. As a result, adopting App Intents not only helps you adopt features the framework enables directly, it allows you to easily support additional system experiences that increase your app’s reach and allow people to personalize how they use your app. Use the App Intents framework to describe actions and content together with:

-   [WidgetKit](/documentation/WidgetKit) to offer interactive and configurable widgets, watch complications, and controls
    
-   [ActivityKit](/documentation/ActivityKit) to offer interactive Live Activities
    
-   [Core Spotlight](/documentation/CoreSpotlight) to enable people to find your content using semantic search in Spotlight
    

### [Plan App Intents framework adoption](/documentation/appintents/making-actions-and-content-discoverable-and-widely-available#Plan-App-Intents-framework-adoption)

If you’re new to the App Intents framework, first evaluate your app’s functionality and content. The framework is a fundamental building block for apps, and enables a broad range of user experiences, so it’s important to design a new app with App Intents functionality in mind. Similarly, consider a measured, thoughtful approach when adopting App Intents in your existing app.

To get started:

1.  Understand the role of the App Intents framework and the experiences it enables.
    
2.  Review key framework concepts and create a first implementation that launches your app with an [`AppIntent`](/documentation/appintents/appintent) and add an App Shortcut. For more information, see [Creating your first app intent](/documentation/appintents/creating-your-first-app-intent) and [App Shortcuts](/documentation/appintents/app-shortcuts).
    
3.  Express additional actions and content using the App Intents framework.
    
4.  Integrate actions and content with Siri and Apple Intelligence. For more information, see [App intent domains](/documentation/appintents/app-intent-domains) and [Integrating actions with Siri and Apple Intelligence](/documentation/appintents/integrating-actions-with-siri-and-apple-intelligence).
    
5.  Depending on your app’s functionality, add support for additional system experiences and interactions that fit your app’s functionality. For example, respond to Focus changes as described in [Focus](/documentation/appintents/focus) or add support for the Action button and squeeze gestures on Apple Pencil Pro, as described in [Responding to the Action button on Apple Watch Ultra](/documentation/appintents/actionbuttonarticle).
    

### [Expand existing App Intents usage](/documentation/appintents/making-actions-and-content-discoverable-and-widely-available#Expand-existing-App-Intents-usage)

If you’re currently using the App Intents framework in your app, you might limit app intents to selected actions and content. The App Intents framework will provide integration with Siri and Apple Intelligence for every action of your app and its content. Review your app’s actions and content, and consider expressing actions and content with App Intents.

### [Understand the impact of removing app intents and shortcuts](/documentation/appintents/making-actions-and-content-discoverable-and-widely-available#Understand-the-impact-of-removing-app-intents-and-shortcuts)

People use app intents to automate workflows with custom shortcuts and [App Shortcuts](/documentation/appintents/app-shortcuts). As a result, removing [`AppIntent`](/documentation/appintents/appintent) code or an App Shortcut from your app can break people’s workflows and confuse or frustrate them because previously available functionality might stop working. Keep this in mind when you adopt the App Intents framework and consider a deprecation strategy for your [`AppIntent`](/documentation/appintents/appintent) code. When you plan to remove an [`AppIntent`](/documentation/appintents/appintent), give people notice about your intention to remove the app intent. Publish a release where you change an existing [`AppIntent`](/documentation/appintents/appintent) to [`DeprecatedAppIntent`](/documentation/appintents/deprecatedappintent) and offer people a suggested replacement. After giving people enough time to update their custom shortcuts and move to new App Shortcuts, remove the [`DeprecatedAppIntent`](/documentation/appintents/deprecatedappintent) from your app.

### [Know when to migrate to the App Intents framework](/documentation/appintents/making-actions-and-content-discoverable-and-widely-available#Know-when-to-migrate-to-the-App-Intents-framework)

For new functionality, use the App Intents framework to integrate your app with system experiences like widgets, controls, and Live Activities. Siri and Apple Intelligence automatically will leverage SiriKit intents. For existing functionality keep existing SiriKit implementations and take a measured approach to replacing SiriKit code with App Intents. If you remove code that uses SiriKit, give people advance notice about changes to avoid breaking their existing custom shortcuts and make sure to provide the same or comparable functionality that uses App Intents.

For more information about migrating your SiriKit code to App Intents, see - [Soup Chef with App Intents: Migrating custom intents](/documentation/SiriKit/soup-chef-with-app-intents-migrating-custom-intents).

---
# Creating your first app intent | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   Creating your first app intent

Article

# Creating your first app intent

Create your first app intent that makes your app available in system experiences like Spotlight or the Shortcuts app.

## [Overview](/documentation/appintents/creating-your-first-app-intent#Overview)

To let people leverage your app’s features outside of the app itself, system experiences like Spotlight and the Shortcuts app require your help to understand your app’s actions and content so the system can expose that functionality. Use [App Intents](/documentation/appintents) to express your app’s capabilities and make your app’s actions available to the system. App intents are self-contained types that act as a bridge between your code and system experiences and services. Each app intent encapsulates a single action that’s specific to your app. It provides the system with any action that makes sense for your app’s audience, such as showing information about a hiking trail from a hiking app, exporting a person’s transaction history from a budgeting app, or converting between two specific units of measurement with a converter app.

Every app intent provides descriptive information about itself that experiences and services like Siri can display or announce. When you build an app that contains app intents, the compiler examines your source and generates data about those intents that Xcode stores in the app bundle. After someone installs your app, the system uses that data to discover the intents and makes them available to the system.

Before you get started creating your first app intent, read [Making actions and content discoverable and widely available](/documentation/appintents/making-actions-and-content-discoverable-and-widely-available) to review App Intents framework features and functionality. Then, identify an action and create your first app intent, and offer an App Shortcut as described below. App Shortcuts make your app intent even more useful. For example, App Shortcuts don’t require configuration, and people can place them on the Action button. Additionally, App Shortcuts appear in Spotlight even when a person hasn’t launched your app.

### [Identify an action](/documentation/appintents/creating-your-first-app-intent#Identify-an-action)

Think about actions and tasks people perform in your app, an action’s input and output data, and how the system could surface actions in its services and experiences. In general, implement your [`AppIntent`](/documentation/appintents/appintent) to have a narrow focus and do one thing well. People can invoke it individually, or create custom shortcuts by combining it with app intents from other apps in the Shortcuts app.

For your first app intent, choose an action that people are likely to use frequently. Then, add an App Shortcut that includes the app intent.

### [Review when to adopt specialized app intent protocols](/documentation/appintents/creating-your-first-app-intent#Review-when-to-adopt-specialized-app-intent-protocols)

For many app intents, the [`AppIntent`](/documentation/appintents/appintent) protocol is the preferred protocol to adopt. However, depending on your app’s specific behaviors, you might prefer your code to conform to one of the other intent protocols; for example:

-   Create app intents that conform to assistant schemas that make sure your actions and content work well with the enhanced action capabilities of Siri that Apple Intelligence provides.
    
-   If your app plays or records audio and you want to offer that same functionality in an app intent, adopt [`AudioPlaybackIntent`](/documentation/appintents/audioplaybackintent) instead. This protocol inherits from `AppIntent` and indicates the audio-related behavior to the system so that, where possible, it avoids audio interactions and other potential interruptions.
    

The App Intents framework provides a number of other specialized app intent protocols. For more information about integrating your app intents with Siri and Apple Intelligence, see [Integrating actions with Siri and Apple Intelligence](/documentation/appintents/integrating-actions-with-siri-and-apple-intelligence) and [App intent domains](/documentation/appintents/app-intent-domains). For more information about other specialized protocols, see [App intents](/documentation/appintents/app-intents).

### [Create an app intent that opens your app](/documentation/appintents/creating-your-first-app-intent#Create-an-app-intent-that-opens-your-app)

To define an action, create a type that adopts the [`AppIntent`](/documentation/appintents/appintent) protocol, or a related protocol that provides the specific behavior you need. If possible, start with a simple action that doesn’t require parameters. Alternatively, if your action requires a parameter, consider initially hard-coding the parameter to get your first app intent implementation to work. Then make changes to add parameters to your first app intent as described in [Adding parameters to an app intent](/documentation/appintents/adding-parameters-to-an-app-intent).

For example, the [Accelerating app interactions with App Intents](/documentation/appintents/acceleratingappinteractionswithappintents) sample code project provides an app intent that opens the app and displays a person’s favorite hiking trails:

```
struct OpenFavorites: AppIntent {
    
    static var title: LocalizedStringResource = "Open Favorite Trails"


    static var description = IntentDescription("Opens the app and goes to your favorite trails.")
    
    static var openAppWhenRun: Bool = true
    
    @MainActor
    func perform() async throws -> some IntentResult {
        navigationModel.selectedCollection = trailManager.favoritesCollection
        
        return .result()
    }
    
    @Dependency
    private var navigationModel: NavigationModel
    
    @Dependency
    private var trailManager: TrailDataManager
}
```

In the structure, implement the protocol’s [`title`](/documentation/appintents/appintent/title) requirement to provide the localized text that the Shortcuts app displays in its Action Library and shortcut editor. To include additional context for the intent, implement the optional [`description`](/documentation/appintents/appintent/description) requirement to provide localized text that describes the app intent’s behavior. The Shortcuts app shows the description in its Action Library.

### [Perform the app intent’s action](/documentation/appintents/creating-your-first-app-intent#Perform-the-app-intents-action)

To provide your intent’s functionality, implement the [`perform()`](/documentation/appintents/appintent/perform\(\)) protocol requirement. The system invokes this method after it resolves any required parameters, meaning those parameters are safe for your code to access from the function’s body.

Your implementation must complete the necessary work and return a result to the system. A result may include, among other things, a value that a shortcut can use in subsequent connected actions, dialogue to display or announce, and a [SwiftUI](/documentation/SwiftUI) snippet view.

For example, the [Accelerating app interactions with App Intents](/documentation/appintents/acceleratingappinteractionswithappintents) sample code project returns a dialog for the `GetTrailInfo` app intent:

```
func perform() async throws -> some IntentResult & ReturnsValue<TrailEntity> & ProvidesDialog & ShowsSnippetView {
    guard let trailData = trailManager.trail(with: trail.id) else {
        throw TrailIntentError.trailNotFound
    }
            
    /**
     You provide a custom view by conforming the return type of the `perform()` function to the `ShowsSnippetView` protocol.
     */
    let snippet = TrailInfoView(trail: trailData, includeConditions: true)
    
    /**
     This intent displays a custom view that includes the trail conditions as part of the view. The dialog includes the trail conditions when
     the system can only read the response, but not display it. When the system can display the response, the dialog omits the trail
     conditions.
     */
    let dialog = IntentDialog(full: "The latest conditions reported for \(trail.name) indicate: \(trail.currentConditions).",
                              supporting: "Here's the latest information on trail conditions.")
    
    return .result(value: trail, dialog: dialog, view: snippet)
}
```

If it doesn’t make sense for your intent to return a concrete result, return `.result()` to tell the system the intent is complete.

### [Verify the behavior of your intent in Simulator or on-device](/documentation/appintents/creating-your-first-app-intent#Verify-the-behavior-of-your-intent-in-Simulator-or-on-device)

During development, validate that your intents behave as you expect by testing them in Simulator or on-device. If you’re adding intents to a macOS app, build and run the app. For other platforms, select the relevant simulator or connected device and then build and run. After your app launches, follow these steps:

1.  Launch the Shortcuts app.
    
2.  Tap or click the New Shortcut (+) button to create a shortcut.
    
3.  Choose Apps in the Action Library’s segmented control.
    
4.  Tap or click your app’s icon.
    
5.  Select the action to test.
    
6.  For app intents with parameters, use the summary to set the parameter values.
    
7.  Tap or click the Run button.
    

Set a breakpoint at the top of your `perform()` method to confirm your implementation is working. The debugger pauses execution immediately after you run the shortcut, enabling you to step through the code and inspect the intent’s parameters to verify they have the values they require.

### [Design custom responses](/documentation/appintents/creating-your-first-app-intent#Design-custom-responses)

People may interact with your app intent through Siri. For a good user experience, consider communicating the intent’s result with a visual response using a custom UI snippet, and as a dialog for Siri to communicate the same information. For more information, see [Design custom responses](/documentation/appintents/acceleratingappinteractionswithappintents#Design-custom-responses).

### [Receive input with parameters and return results](/documentation/appintents/creating-your-first-app-intent#Receive-input-with-parameters-and-return-results)

Creating an app intent that opens a screen in your app is the first step to becoming familiar with the App Intents framework and to making your app and its content discoverable. Many actions in your app receive input and return data. To describe actions that receive and return data, add parameters to the app intent to tell the system about that data and whether it’s required or optional. By exposing parameters, you enable people to configure your intents with values unique to their requirements and enable the App Intents framework to communicate with system experiences to write those values at runtime. For example, the [Accelerating app interactions with App Intents](/documentation/appintents/acceleratingappinteractionswithappintents) sample code project enables people to choose which hiking trail information to view when they invoke an app intent. For more information about using parameters in an app intent, see [Adding parameters to an app intent](/documentation/appintents/adding-parameters-to-an-app-intent).

### [Create an App Shortcut](/documentation/appintents/creating-your-first-app-intent#Create-an-App-Shortcut)

App intents you create appear in the Shortcuts app. People can create custom shortcuts that initiate your app intent and combine app intents to perform custom workflows. To enable people to discover and run your app intents without any configuration, bundle your app’s app intents into App Shortcuts to provide workflows of your app’s actions.

By offering App Shortcuts, you make your app’s functionality instantly available for use in Shortcuts, Spotlight, and Siri from the moment a person installs your app — without any setup in the Shortcuts app or an Add to Siri button. On devices that support the Action button, people can invoke your App Shortcut with the Action button for quick access to your app’s functionality.

To offer an App Shortcut for your first app intent:

1.  Create the [`AppShortcut`](/documentation/appintents/appshortcut) object for your app intent using the [`init(intent:phrases:shortTitle:systemImageName:)`](/documentation/appintents/appshortcut/init\(intent:phrases:shorttitle:systemimagename:\)-8yntq) initializer and provide phrases that people can use to run the app intent and the metadata that appears in the Shortcuts app.
    
2.  Implement the [`AppShortcutsProvider`](/documentation/appintents/appshortcutsprovider) protocol that provides the App Shortcuts you offer to the Shortcuts app.
    

For more information about creating App Shortcuts, see [App Shortcuts](/documentation/appintents/app-shortcuts) and [Create App Shortcuts](/documentation/appintents/acceleratingappinteractionswithappintents#Create-App-Shortcuts).

To learn more about supporting the Action button on iPhone and Apple Watch, see [Responding to the Action button on Apple Watch Ultra](/documentation/appintents/actionbuttonarticle).

### [Donate app intents to the system](/documentation/appintents/creating-your-first-app-intent#Donate-app-intents-to-the-system)

Make your app intents discoverable by explicitly donating them to the system. When someone performs an action in your app, donate an intent that corresponds to that action. The system uses the information you provide to predict actions someone might take in the future. For example, if someone requests the weather from your app each morning, the system might proactively offer the corresponding app intent at the same time each day.

For more information, see [Intent discovery](/documentation/appintents/intent-discovery).

---
# Adopting App Intents to support system experiences | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   Adopting App Intents to support system experiences

Sample Code

# Adopting App Intents to support system experiences

Create app intents and entities to incorporate system experiences such as Spotlight, visual intelligence, and Shortcuts.

[Download](https://docs-assets.developer.apple.com/published/ea2226a454ce/AdoptingAppIntentsToSupportSystemExperiences.zip)

## [Overview](/documentation/appintents/adopting-app-intents-to-support-system-experiences#Overview)

The app in this sample offers actions in the Shortcuts app that people can use to create custom shortcuts. It includes an App Shortcut to find the closest landmark and find tickets to visit the landmark, all without opening the app. Additionally, the app makes its data available to system experiences like Spotlight, Siri and Apple Intelligence, and visual intelligence.

By adopting the App Intents framework, the app provides functionality across the system, enabling people to:

-   In Shortcuts, find and run the app’s app intents.
    
-   In Shortcuts, create custom shortcuts or view the provided “Find Closest” App Shortcut.
    
-   In Shortcuts, place custom shortcuts or the App Shortcut on the Home Screen as a bookmark.
    
-   In Spotlight, search for a landmark or the “Find Closest” App Shortcut.
    
-   With visual intelligence, circle an object in the visual intelligence camera or onscreen and view matching results from the app.
    
-   With the Action button, trigger a custom shortcut or the App Shortcut.
    
-   From Siri suggestions, use custom shortcuts or the App Shortcut.
    
-   In the app, view information about a landmark, then ask Siri something like “What’s a summary of the history of this place?” or similar to receive a content summary, and more.
    

### [Describe actions as app intents and entities](/documentation/appintents/adopting-app-intents-to-support-system-experiences#Describe-actions-as-app-intents-and-entities)

The app contains many actions and makes them available to the system as app intents, so people can use them to create custom shortcuts and invoke across system experiences. For example, the app offers key actions like finding the closest landmark or opening a landmark in the app. This app intent opens a landmark in the app:

```
import AppIntents


struct OpenLandmarkIntent: OpenIntent {
    static let title: LocalizedStringResource = "Open Landmark"


    @Parameter(title: "Landmark", requestValueDialog: "Which landmark?")
    var target: LandmarkEntity


    func perform() async throws -> some IntentResult {
        return .result()
    }
}
```

To use your data as input and output of app intents and make the data available to the system, you use app entities. App entities often limit the information a model object you persist to storage to what the system needs. They also add required information to understand the data or to use it in system experiences. For example, the `LandmarkEntity` of the sample app provides required `typeDisplayRepresentation` and `displayRepresentation` properties but doesn’t include every property of the `Landmark` model object:

```
struct LandmarkEntity: IndexedEntity {
    static var typeDisplayRepresentation: TypeDisplayRepresentation {
        return TypeDisplayRepresentation(
            name: LocalizedStringResource("Landmark", table: "AppIntents", comment: "The type name for the landmark entity"),
            numericFormat: "\(placeholder: .int) landmarks"
        )
    }


    var displayRepresentation: DisplayRepresentation {
        DisplayRepresentation(
            title: "\(name)",
            subtitle: "\(continent)",
            image: .init(data: try! self.thumbnailRepresentationData)
        )
    }


    static let defaultQuery = LandmarkEntityQuery()


    var id: Int { landmark.id }


    @ComputedProperty(indexingKey: \.displayName)
    var name: String { landmark.name }


    // Maps the description variable to the Spotlight indexing key `contentDescription`.
    @ComputedProperty(indexingKey: \.contentDescription)
    var description: String { landmark.description }


    @ComputedProperty
    var continent: String { landmark.continent }


    @DeferredProperty
    var crowdStatus: Int {
        get async throws { // swiftlint:disable:this implicit_getter
            await modelData.getCrowdStatus(self)
        }
    }


    var landmark: Landmark
    var modelData: ModelData


    init(landmark: Landmark, modelData: ModelData) {
        self.modelData = modelData
        self.landmark = landmark
    }
}
```

For more information about describing actions as app intents and app entities, refer to [Making actions and content discoverable and widely available](/documentation/AppIntents/Making-actions-and-content-discoverable-and-widely-available) and [Creating your first app intent](/documentation/AppIntents/Creating-your-first-app-intent).

### [Offer interactive snippets](/documentation/appintents/adopting-app-intents-to-support-system-experiences#Offer-interactive-snippets)

The app’s “Find Closest” App Shortcut performs an app intent that finds the closest nearby landmark without opening the app, and allows people to find tickets to visit it. Instead of taking them to the app, the app intent displays interactive snippets that appear as overlays at the top of the screen. To display the interactive snippet, the app’s `ClosestLandmarkIntent` returns a [`SnippetIntent`](/documentation/AppIntents/SnippetIntent) that presents the interactive snippet in its `perform()` method:

```
import AppIntents
import SwiftUI


struct ClosestLandmarkIntent: AppIntent {
    static let title: LocalizedStringResource = "Find Closest Landmark"


    @Dependency var modelData: ModelData


    func perform() async throws -> some ReturnsValue<LandmarkEntity> & ShowsSnippetIntent & ProvidesDialog {
        let landmark = await self.findClosestLandmark()


        return .result(
            value: landmark,
            dialog: IntentDialog(
                full: "The closest landmark is \(landmark.name).",
                supporting: "\(landmark.name) is located in \(landmark.continent)."
            ),
            snippetIntent: LandmarkSnippetIntent(landmark: landmark)
        )
    }
}
```

For more information about displaying interactive snippets, refer to [Displaying static and interactive snippets](/documentation/AppIntents/displaying-static-and-interactive-snippets).

### [Make your entity available to Siri and Apple Intelligence](/documentation/appintents/adopting-app-intents-to-support-system-experiences#Make-your-entity-available-to-Siri-and-Apple-Intelligence)

To allow Siri to access the landmark information that’s visible onscreen in the app, its `LandmarkEntity` implements the [`Transferable`](/documentation/CoreTransferable/Transferable) protocol and provides plain-text, image, and PDF representations that Siri can understand and forward to other services, including third-party services:

```
extension LandmarkEntity: Transferable {
    static var transferRepresentation: some TransferRepresentation {
        FileRepresentation(exportedContentType: .pdf) { @MainActor landmark in
            let url = URL.documentsDirectory.appending(path: "\(landmark.name).pdf")


            let renderer = ImageRenderer(content: VStack {
                Image(landmark.landmark.backgroundImageName)
                    .resizable()
                    .aspectRatio(contentMode: .fit)
                Text(landmark.name)
                Text("Continent: \(landmark.continent)")
                Text(landmark.description)
            }.frame(width: 600))


            renderer.render { size, renderer in
                var box = CGRect(x: 0, y: 0, width: size.width, height: size.height)


                guard let pdf = CGContext(url as CFURL, mediaBox: &box, nil) else {
                    return
                }
                pdf.beginPDFPage(nil)
                renderer(pdf)
                pdf.endPDFPage()
                pdf.closePDF()
            }


            return .init(url)
        }


        DataRepresentation(exportedContentType: .image) {
            try $0.imageRepresentationData
        }


        DataRepresentation(exportedContentType: .plainText) {
            """
            Landmark: \($0.name)
            Description: \($0.description)
            """.data(using: .utf8)!
        }
    }
}
```

When the landmark becomes visible onscreen, the app uses the user activity annotation API to give the system access to the data:

```
HStack(alignment: .bottom) {
    Text(landmark.name)
        .font(.title)
        .fontWeight(.bold)
        .userActivity(
            "com.landmarks.ViewingLandmark"
        ) {
            $0.title = "Viewing \(landmark.name)"
            $0.appEntityIdentifier = EntityIdentifier(for: try! modelData.landmarkEntity(id: landmark.id))
        }
}
```

For more information about making onscreen content available to Siri and Apple Intelligence, refer to [Making onscreen content available to Siri and Apple Intelligence](/documentation/AppIntents/Making-onscreen-content-available-to-siri-and-apple-intelligence).

### [Add entities to the Spotlight index](/documentation/appintents/adopting-app-intents-to-support-system-experiences#Add-entities-to-the-Spotlight-index)

The app describes its data as app entities, so the system can use it when it performs app intents. Additionally, the app donates the entities into the semantic search index, making it possible to find the app entities in Spotlight. The following example shows how the app’s `LandmarkEntity` conforms to [`IndexedEntity`](/documentation/AppIntents/IndexedEntity) and uses Swift macros to add the indexing keys that Spotlight needs.

```
struct LandmarkEntity: IndexedEntity {
    // ...


    // Maps the description to the Spotlight indexing key `contentDescription`.
    @ComputedProperty(indexingKey: \.contentDescription)
    var description: String { landmark.description }


    @ComputedProperty
    var continent: String { landmark.continent }


    // ...
}
```

In a utility function, the app donates the landmark entities to the Spotlight index:

```
static func donateLandmarks(modelData: ModelData) async throws {
    let landmarkEntities = await modelData.landmarkEntities
    try await CSSearchableIndex.default().indexAppEntities(landmarkEntities)
}
```

For more information, refer to [Making app entities available in Spotlight](/documentation/AppIntents/making-app-entities-available-in-spotlight).

### [Integrate search results with visual intelligence](/documentation/appintents/adopting-app-intents-to-support-system-experiences#Integrate-search-results-with-visual-intelligence)

With visual intelligence, people circle items onscreen or in visual intelligence camera to search for matching results across apps that support visual intelligence. To support visual intelligence search, the sample app implements an [`IntentValueQuery`](/documentation/AppIntents/IntentValueQuery) to find matching landmarks:

```
@UnionValue
enum VisualSearchResult {
    case landmark(LandmarkEntity)
    case collection(CollectionEntity)
}


struct LandmarkIntentValueQuery: IntentValueQuery {


    @Dependency var modelData: ModelData


    func values(for input: SemanticContentDescriptor) async throws -> [VisualSearchResult] {


        guard let pixelBuffer: CVReadOnlyPixelBuffer = input.pixelBuffer else {
            return []
        }


        let landmarks = try await modelData.search(matching: pixelBuffer)


        return landmarks
    }
}


extension ModelData {
    /**
     This method contains the search functionality that takes the pixel buffer that visual intelligence provides
     and uses it to find matching app entities. To keep this example app easy to understand, this function always
     returns the same landmark entity.
    */
    func search(matching pixels: CVReadOnlyPixelBuffer) throws -> [VisualSearchResult] {
        let landmarks = landmarkEntities.filter {
            $0.id != 1005
        }.map {
            VisualSearchResult.landmark($0)
        }.shuffled()


        let collections = userCollections
            .filter {
                $0.landmarks.contains(where: { $0.id == 1005 })
            }
            .map {
                CollectionEntity(collection: $0, modelData: self)
            }
            .map {
                VisualSearchResult.collection($0)
            }


        return [try! .landmark(landmarkEntity(id: 1005))]
            + collections
            + landmarks
    }
}
```

For more information about integrating your app with visual intelligence, refer to [Visual Intelligence](/documentation/VisualIntelligence).

---
# Accelerating app interactions with App Intents | Apple Developer Documentation

-   [App Intents](/documentation/appintents)
-   Accelerating app interactions with App Intents

Sample Code

# Accelerating app interactions with App Intents

Enable people to use your app’s features quickly through Siri, Spotlight, and Shortcuts.

[Download](https://docs-assets.developer.apple.com/published/069aab226592/AcceleratingAppInteractionsWithAppIntents.zip)

## [Overview](/documentation/appintents/acceleratingappinteractionswithappintents#Overview)

The app in this sample code project provides information on trails, allowing people to check on conditions, search for trails that allow activities like skiing, and record which trails they visit. Expressing these features as intents allows people to use them through Siri, Spotlight search, and Shortcuts. Additionally, the project integrates workout tracking on Apple Watch, and shows how to implement Action button support on Apple Watch Ultra. The intents also appear as actions in the Shortcuts app. People can combine these actions to build entirely new features in Shortcuts because the intents provide custom data types that match each other’s inputs.

### [Identify common actions](/documentation/appintents/acceleratingappinteractionswithappintents#Identify-common-actions)

The sample app includes two key features that people are likely to use frequently: looking up information on a trail, and recording activity on a trail. To make it easy for people to use these features without even opening the app, the sample code creates intents for them to use with Siri, Spotlight search, and Shortcuts. For example, if someone saves their favorite trails in the app and wants to get the current conditions for those trails, the app implements the `OpenFavorites` structure, which conforms to [`AppIntent`](/documentation/AppIntents/AppIntent). When someone runs this intent, the app opens and navigates to the Favorites view.

```
/// Each intent needs to include metadata, such as a localized title. The title of the intent displays throughout the system.
static let title: LocalizedStringResource = "Open Favorite Trails"


/// An intent can optionally provide a localized description that the Shortcuts app displays.
static let description = IntentDescription("Opens the app and goes to your favorite trails.")


/// Tell the system to bring the app to the foreground when the intent runs.
static let openAppWhenRun: Bool = true


/**
 When the system runs the intent, it calls `perform()`.
 
 Intents run on an arbitrary queue. Intents that manipulate UI need to annotate `perform()` with `@MainActor`
 so that the UI operations run on the main actor.
 */
@MainActor
func perform() async throws -> some IntentResult {
    navigationModel.selectedCollection = trailManager.favoritesCollection
    
    /// Return an empty result, indicating that the intent is complete.
    return .result()
}
```

### [Create App Shortcuts](/documentation/appintents/acceleratingappinteractionswithappintents#Create-App-Shortcuts)

People may ask Siri to show their favorite trails, or they may find this suggested action through a Spotlight search. To support both of these options, the app implements an [`AppShortcut`](/documentation/AppIntents/AppShortcut) using `OpenFavorites`. An App Shortcut combines an intent with phrases people may use with Siri to perform the action, and additional metadata, such as an icon, and then uses this information in a Spotlight search. People can invoke the App Shortcut with a suggested phrase, or other similiar words, because the system uses a semantic similarity index to help identify people’s requests — automatically matching phrases that are similar, but not identical.

```
AppShortcut(intent: OpenFavorites(), phrases: [
    "Open Favorites in \(.applicationName)",
    "Show my favorite \(.applicationName)"
],
shortTitle: "Open Favorites",
systemImageName: "star.circle")
```

To register the App Shortcut with the system, the app calls [`updateAppShortcutParameters`](/documentation/AppIntents/AppShortcutsProvider/updateAppShortcutParameters\(\)) on its [`AppShortcutsProvider`](/documentation/AppIntents/AppShortcutsProvider) during the [`init`](/documentation/SwiftUI/App/init\(\)) of the [`App`](/documentation/SwiftUI/App) structure.

To aid the system’s presentation of the App Shortcut, the sample app includes a short title and an SF Symbols name that represent the App Shortcut. Further, the sample app’s `Info.plist` file declares `NSAppIconActionTintColorName` with the app’s primary color and two contrasting colors in an array for the `NSAppIconComplementingColorNames` key. The system uses these colors when displaying the App Shortcuts, such as in Spotlight or the Shortcuts app. The specified values of the color names for these keys come from the app’s asset catalog.

After registering an App Shortcut with the system, people can begin using the intent through Siri without any further configuration. To teach people a phrase to use the intent, the app provides a [`SiriTipView`](/documentation/AppIntents/SiriTipView) in the associated view.

```
SiriTipView(intent: OpenFavorites(), isVisible: $displaySiriTip)
```

The `SiriTipView` takes a binding to a visibility Boolean so that the app hides the view if an individual chooses to dismiss it.

Aside from intents for people to quickly view their favorite trails and track their workouts, the sample app provides extensive search capabilities through intents. The app doesn’t provide App Shortcuts for intents that people use less commonly. Best practice is to provide App Shortcuts for only the most common actions in an app — usually between two and five intents, and not more than ten.

### [Design custom responses](/documentation/appintents/acceleratingappinteractionswithappintents#Design-custom-responses)

Even though the app doesn’t provide `GetTrailInfo` as an App Shortcut, people may still interact with it through Siri, such as including the intent in a shortcut they create in the Shortcuts app. For a good user experience, this intent provides its result with a visual response using a custom UI snippet, and as a dialog for Siri to communicate the same information. It does so by conforming the return type of the intent’s [`perform`](/documentation/AppIntents/AppIntent/perform\(\)) function to both [`ProvidesDialog`](/documentation/AppIntents/ProvidesDialog) and [`ShowsSnippetView`](/documentation/AppIntents/ShowsSnippetView).

```
func perform() async throws -> some IntentResult & ReturnsValue<TrailEntity> & ProvidesDialog & ShowsSnippetView {
```

The app provides both visual experiences and voice-only experiences because people may be in a context where they can’t see information in a custom UI (such as when the intent runs on HomePod), or when displaying the custom UI may be inappropriate (such as when the intent runs through CarPlay). This implementation provides a custom UI with a shorter supporting dialog to use when the custom UI is visible, and a different dialog containing additional information if the system can’t show the snippet. The sample uses a transparent background for the custom UI because the system displays it over a translucent background material. Avoiding opaque backgrounds provides the best results.

```
let snippet = TrailInfoView(trail: trailData, includeConditions: true)


/**
 This intent displays a custom view that includes the trail conditions as part of the view. The dialog includes the trail conditions when
 the system can only read the response, but not display it. When the system can display the response, the dialog omits the trail
 conditions.
 */
let dialog = IntentDialog(full: "The latest reported conditions for \(trail.name) indicate: \(trail.currentConditions).",
                          supporting: "Here's the latest information on trail conditions.")


return .result(value: trail, dialog: dialog, view: snippet)
```

This sample app provides custom dialog throughout its intents. `SuggestTrails` validates the parameters that people provide and uses the custom dialog to prompt them for additional information. For example, if the provided location parameter isn’t specific enough, the intent prompts the individual to choose from a list of locations related to their input. The app does this by throwing [`needsDisambiguationError`](/documentation/AppIntents/IntentParameter/needsDisambiguationError\(among:dialog:\)) with a value for the dialog parameter.

```
let dialog = IntentDialog("Multiple locations match \(location). Did you mean one of these locations?")
let disambiguationList = suggestedMatches.sorted(using: KeyPathComparator(\.self, comparator: .localizedStandard))
throw $location.needsDisambiguationError(among: disambiguationList, dialog: dialog)
```

### [Add parameters to an intent](/documentation/appintents/acceleratingappinteractionswithappintents#Add-parameters-to-an-intent)

An app intent can optionally require certain parameters to complete its action. For example, the `GetTrailInfo` intent declares a `trail` parameter by decorating the property with the [`IntentParameter`](/documentation/AppIntents/IntentParameter) property wrapper.

```
@Parameter(title: "Trail", description: "The trail to get information for.")
var trail: TrailEntity
```

The system supports parameters using common Foundation types, such as [`String`](/documentation/Swift/String), and those for custom data types in an app. The app makes its trail data available in an app intent through the `TrailEntity` type, which is a structure conforming to the [`AppEntity`](/documentation/AppIntents/AppEntity) protocol.

To allow the system to query the app for `TrailEntity` data, the entity implements the [`Identifiable`](/documentation/Swift/Identifiable) protocol with values that are stable and persistent. `TrailEntity` declares [`defaultQuery`](/documentation/AppIntents/AppEntity/defaultQuery-4khg7), which the system uses to perform queries to receive `TrailEntity` structures.

```
static let defaultQuery = TrailEntityQuery()
```

An `AppEntity` makes its properties available to the system by decorating it with the [`EntityProperty`](/documentation/AppIntents/EntityProperty) property wrapper.

```
/**
 The trail's name. The `EntityProperty` property wrapper makes this property's data available to the system as part of the intent,
 such as when an intent returns a trail in a shortcut.
 
 The system automatically generates the title for this property from the variable name when it displays it in a system UI, like Shortcuts.
 Generated titles are available for both `EntityProperty` and `IntentIntentParameter` property wrappers.
 */
@Property var name: String


/**
 A description of the trail's location, such as a nearby city name, or the national park encompassing it.
 
 If you want the displayed title for the property to be different from the variable name, use a `title` parameter with the
 `EntityProperty` property wrapper.
 */
@Property(title: "Region")
var regionDescription: String


```

### [Provide the app’s data through queries](/documentation/appintents/acceleratingappinteractionswithappintents#Provide-the-apps-data-through-queries)

The system queries the app for its trail data through `TrailEntityQuery`, a type conforming to [`EntityQuery`](/documentation/AppIntents/EntityQuery). For example, if someone saves a specific value as the `trail` parameter for `GetTrailInfo`, the system locates the `TrailEntity` by using the `defaultQuery` and requesting the entity by its ID from the `Identifable` protocol. All types conforming to `EntityQuery` need to implement this method.

```
func entities(for identifiers: [TrailEntity.ID]) async throws -> [TrailEntity] {
    Logger.entityQueryLogging.debug("[TrailEntityQuery] Query for IDs \(identifiers)")
    
    return trailManager.trails(with: identifiers)
            .map { TrailEntity(trail: $0) }
}
```

The app also provides a list of common trail suggestions by implementing the optional [`suggestedEntities`](/documentation/AppIntents/EntityQuery/suggestedEntities\(\)) function.

```
func suggestedEntities() async throws -> [TrailEntity] {
    Logger.entityQueryLogging.debug("[TrailEntityQuery] Request for suggested entities")
    
    return trailManager.trails(with: trailManager.favoritesCollection.members)
            .map { TrailEntity(trail: $0) }
}
```

There are several subprotocols to `EntityQuery`, each of which enables different types of functionality. The sample app implements all of them for demonstration purposes, but a real app can use only the ones that meet its needs.

The app implements [`EntityStringQuery`](/documentation/AppIntents/EntityStringQuery) to help people configure `GetTrailInfo`. When people configure this intent in the Shortcuts app, they first see the list of trails from `suggestedEntities`. The Shortcuts app provides a search field, enabling people to search for results that appear in the list of suggested trails. The app provides results for the search term by implementing [`entities(matching:)`](/documentation/AppIntents/EntityStringQuery/entities\(matching:\)).

```
func entities(matching string: String) async throws -> [TrailEntity] {
    Logger.entityQueryLogging.debug("[TrailEntityQuery] String query for term \(string)")
    
    return trailManager.trails { trail in
        trail.name.localizedCaseInsensitiveContains(string)
    }.map { TrailEntity(trail: $0) }
}
```

### [Enable Find intents](/documentation/appintents/acceleratingappinteractionswithappintents#Enable-Find-intents)

Apps implementing either the [`EnumerableEntityQuery`](/documentation/AppIntents/EnumerableEntityQuery) or the [`EntityPropertyQuery`](/documentation/AppIntents/EntityPropertyQuery) protocol automatically add a Find intent in the Shortcuts app. These intents enable people to build powerful new features for themselves in Shortcuts, powered by the app’s data — without requiring the app to implement that feature itself. For example, the sample app focuses its UI on providing trail information, but people can also use its data to plan activities for a vacation. The app doesn’t need to build vacation-planning features because it implements these entity query protocols to provide an interface to the data through a Shortcut.

The sample app groups trails into collections based on geographic region, and implements the collections as a type called `TrailCollection` that conforms to `AppEntity`. The list of geographic regions is small, and a `TrailCollection` is a simple structure with the collection name and a list of trail IDs that require little memory. To make this information available through a Find intent, the app implements `FeaturedCollectionEntityQuery` with conformance to `EnumerableEntityQuery`. The app uses `EnumerableEntityQuery` here because the data for the featured trail collections is a small and fixed set of values, and doesn’t require a large amount of memory. The app implements [`allEntities`](/documentation/AppIntents/EnumerableEntityQuery/allEntities\(\)) to return all of the values, which people can filter by name in the Shortcuts app.

```
func allEntities() async throws -> [TrailCollection] {
    Logger.entityQueryLogging.debug("[FeaturedCollectionEntityQuery] Request for all entities")
    return trailManager.featuredTrailCollections
}
```

The app also implements `EntityPropertyQuery` for `TrailEntity`. This query type is ideal for large data sets that may have large numbers of entities, or entities that have higher memory consumption. Implementing this query adds a Find intent to the Shortcuts app, enabling people to run predicate searches on entity properties. For example, someone planning a vacation around seeing waterfalls that are easily accessible can configure the Find intent with criteria for trails containing _fall_ in the trail name, and a trail distance of less than 1 kilometer. An implementation of `EntityPropertyQuery` includes several required functions and properties. `TrailEntityQuery+PropertyQuery.swift` contains the complete implementation.

Designing great intents for integration with the system means that the intents work as standalone intents with their parameters, and also work with other intents the app provides, or with other apps that may be installed. People can create shortcuts that use the output of one intent the app provides and use it as input to another intent the app provides, like the following examples:

-   `SuggestTrails` can use the output of the Find intent for trail collections as input.
    
-   The Find intent for trails can use the output of `SuggestTrails` to further refine the results.
    
-   The Find intent for trails can also work alone, searching for matching trail properties from all of the trail data the app provides.
    

### [Contribute entities to Spotlight](/documentation/appintents/acceleratingappinteractionswithappintents#Contribute-entities-to-Spotlight)

The sample app provides its trail data to Spotlight when the app first runs. The app declares a `Trail` structure for this data, containing the app’s internal representation of that data. The app maps its data from the structure to searchable attributes in a [`CSSearchableItemAttributeSet`](/documentation/CoreSpotlight/CSSearchableItemAttributeSet).

```
var searchableAttributes: CSSearchableItemAttributeSet {
    let attributes = CSSearchableItemAttributeSet()
    
    attributes.title = name
    attributes.namedLocation = regionDescription
    attributes.keywords = activities.localizedElements
    
    attributes.latitude = NSNumber(value: coordinate.latitude)
    attributes.longitude = NSNumber(value: coordinate.longitude)
    attributes.supportsNavigation = true
    
    return attributes
}
```

The app also declares a `TrailEntity` structure to make the trail data available to the rest of the system as part of its App Intents integration. To integrate `TrailEntity` with Spotlight, `TrailEntity` conforms to [`IndexedEntity`](/documentation/AppIntents/IndexedEntity). The app associates the searchable attributes from the `Trail` structure with the `TrailEntity` by calling [`associateAppEntity(_:priority:)`](/documentation/CoreSpotlight/CSSearchableItem/associateAppEntity\(_:priority:\)) before contributing the data to the Spotlight index.

```
// Create an array of the searchable information for each `Trail`.
let searchableItems = trails.map { trail in
    let item = CSSearchableItem(uniqueIdentifier: String(trail.id),
                                domainIdentifier: nil,
                                attributeSet: trail.searchableAttributes)
    
    let isFavorite = favoritesCollection.members.contains(trail.id)
    let weight = isFavorite ? 10 : 1
    let intent = TrailEntity(trail: trail)
    
    /**
     Associate `TrailEntity` with the data that the `Trail` structure provides so the system recognizes that
     both types represent the same data. You need to create this association before adding the `CSSearchableItem`
     to a `CSSearchableIndex`.
     */
    item.associateAppEntity(intent, priority: weight)
    return item
}


do {
    // Add the trails to the search index so people can find them through Spotlight.
    // You need to do this as part of the app's initial setup on launch.
    let index = CSSearchableIndex.default()
    try await index.indexSearchableItems(searchableItems)
    Logger.spotlightLogging.info("[Spotlight] Trails indexed by Spotlight")
} catch let error {
    Logger.spotlightLogging.error("[Spotlight] Trails were not indexed by Spotlight. Reason: \(error.localizedDescription)")
}
```

### [Integrate universal links](/documentation/appintents/acceleratingappinteractionswithappintents#Integrate-universal-links)

The sample app offers an `OpenTrail` intent so that people can open the app to a specific trail’s information from a shortcut. Rather than adding code to configure the app’s UI for displaying a trail’s information just for this intent, the app uses the same URL scheme it uses to implement universal links. The app declares the URL for a trail’s details through conformance to [`URLRepresentableEntity`](/documentation/AppIntents/URLRepresentableEntity).

```
extension TrailEntity: URLRepresentableEntity {
    static var urlRepresentation: URLRepresentation {
        // Use string interpolation to fill values from your entity necessary for constructing the universal link URL.
        // This example URL uses the unique and persistant identifier for the `TrailEntity` in the URL.
        "https://example.com/trail/\(.id)/details"
    }
}
```

To leverage the app’s existing code for handling a universal link, the app conforms the `OpenTrail` intent to both [`OpenIntent`](/documentation/AppIntents/OpenIntent) and [`URLRepresentableIntent`](/documentation/AppIntents/URLRepresentableIntent). These conformances allow the app to skip implementing a `perform()` method on `OpenTrail`. When the intent runs, the system automatically passes the URL to the app using the standard mechanisms required for handling universal links.
