Source:
```
https://medium.com/@chaemin2001/sharing-core-data-with-cloudkit-in-swiftui-94536ad61133
```

For the past 6 weeks, our team worked on an app named ‘Cleanny’, which is now also available on App Store.

[https://apps.apple.com/kr/app/cleanny/id1630640491](https://apps.apple.com/kr/app/cleanny/id1630640491)

‘Cleanny’ is for people who are not motivated to clean up their house and do the daily household routines due to lack of energy. Our app provides four types of character, each representing the state of chores done.

We manage the user’s data with CoreData, and used Cloudkit to implement one of the core features of the app, ‘sharing your character with friends’.

Since it was my first time dealing with Cloudkit, I refered to sample codes provided in the link below.

Since it was my first time dealing with Cloudkit, I refered to sample codes provided in the link below.

```
https://www.kodeco.com/29934862-sharing-core-data-with-cloudkit-in-swiftui#toc-anchor-001
```

**simple warning:** The code in the link works well until before the sharing stage. If you need any tip to make the sharing happen, take a look at the instructions below.

The original plan was to connect the ‘User’ Model of core data directly to cloudkit so that the data on the cloud would update automatically each time the core data is changed. In order to do this, you have to set the configuration of the core data as below.

![[Screenshot 2025-09-03 at 18.52.26.png]]

You will find further details ⬇️

```
https://developer.apple.com/documentation/coredata/setting-up-core-data-with-cloudkit
```

## CloudkitUser.swift

However, due to several problems regarding fetching CKRecord and CKContainer without an attribute whose type is ‘CKRecord’, I decided to make another model named ‘CloudkitUser’, and fetch and save data through the model.

```
import Foundation
import CloudKit

struct CloudkitUser: Identifiable {
    let id: String
    var name: String
    var totalPercentage: Double
    let associatedRecord: CKRecord
    
    mutating func setName(name : String){
        self.name = name
    }
}

extension CloudkitUser {
    init?(record: CKRecord) {
        guard let name = record["name"] as? String,
              let totalPercentage = record["totalPercentage"] as? Double else { return nil }
        self.id = record.recordID.recordName
        self.name = name
        self.totalPercentage = totalPercentage
        self.associatedRecord = record
    }
}
```

## CloudkitUserViewModel.swift

Then, a View Model with all the functions and enums to communicate with your cloudkit database will be needed. This view model should

- **check the state of the fetching process: whether it is loading, done with loading, or returned with an error**

```
enum State {
  case loading
  case loaded(me: [CloudkitUser], friends: [CloudkitUser])
  case error(Error)
}
    
@Published private(set) var state: State = .loading
lazy var container = CKContainer(identifier: Config.containerIdentifier)
private lazy var database = container.privateCloudDatabase
let sharingZone = CKRecordZone(zoneName: "SharingZone")

nonisolated init() {}

init(state: State) {
  self.state = state
}
```

- **initialize every time the View is refreshed, and create a cloudkit zone if needed**

```
func initialize() async throws {
  do {
    try await createZoneIfNeeded()
  } catch {
    state = .error(error)
  }
}

private func createZoneIfNeeded() async throws {
  guard !UserDefaults.standard.bool(forKey: "isZoneCreated") else { return }
  
  do {
    _ = try await database.modifyRecordZones(saving: [sharingZone], deleting: [])
  } catch {
    print("ERROR: Failed to create custom zone: \(error.localizedDescription)")
    throw error
  }
  
  UserDefaults.standard.setValue(true, forKey: "isZoneCreated")
}
```

- **fetch data from both ‘Private Database’ and ‘Shared Database’ and return with type [CloudkitUser]**

```
private func fetchUsers(scope: CKDatabase.Scope, in zones: [CKRecordZone]) async throws -> [CloudkitUser] {
  
  let database = container.database(with: scope)
  var allCloudkitUsers: [CloudkitUser] = []
  
  @Sendable func usersInZone(_ zone: CKRecordZone) async throws -> [CloudkitUser] {
    var allUsers: [CloudkitUser] = []
    var awaitingChanges = true
    var nextChangeToken: CKServerChangeToken? = nil

    while awaitingChanges {
      let zoneChanges = try await database.recordZoneChanges(inZoneWith: zone.zoneID, since: nextChangeToken)
      let users = zoneChanges.modificationResultsByID.values
            .compactMap { try? $0.get().record }
            .compactMap { CloudkitUser(record: $0)}
      allUsers.append(contentsOf: users)
      awaitingChanges = zoneChanges.moreComing
      nextChangeToken = zoneChanges.changeToken
    }
    
    return allUsers
  }
  
  try await withThrowingTaskGroup(of: [CloudkitUser].self) { group in
    for zone in zones {
      group.addTask {
        try await usersInZone(zone)
      }
    }
    for try await usersResult in group {
      allCloudkitUsers.append(contentsOf: usersResult)
    }
  }
    
  return allCloudkitUsers
}
```

- **update the data to the cloud when needed**

```
func addUser(name: String, totalPercentage: Double) async throws {
  let id = CKRecord.ID(zoneID: sharingZone.zoneID)
  let userRecord = CKRecord(recordType: "CloudkitUser", recordID: id)
  userRecord["name"] = name
  userRecord["totalPercentage"] = totalPercentage
  try await database.save(userRecord)
}

func updateUser(user: CloudkitUser, name: String, totalPercentage: Double) {
  let userRecord = user.associatedRecord
  userRecord["name"] = name
  userRecord["totalPercentage"] = totalPercentage
  
  database.save(userRecord) { [weak self] returnedRecord, returnedError in
    print("Record: \(String(describing: returnedRecord))")
    print("Error: \(String(describing: returnedError))")
  }
}
```

## CloudkitShareView.swift

The next thing needed is the actual sharing view, where users can check who they are sharing with, and send links via iMessage.

Apple provides UICloudSharingControllerDelegate, so that it is easy to create such linking view. Here is the code for the view.

```
import Foundation
import SwiftUI
import UIKit
import CloudKit

struct CloudkitShareView: UIViewControllerRepresentable {

  @Environment(\.presentationMode) var presentationMode
  let container: CKContainer
  let share: CKShare

  func updateUIViewController(_ uiViewController: UIViewControllerType, context: Context) {}
  
  func makeUIViewController(context: Context) -> some UIViewController {
    let sharingController = UICloudSharingController(share: share, container: container)
    sharingController.availablePermissions = [.allowReadWrite, .allowPrivate]
    sharingController.delegate = context.coordinator
    sharingController.modalPresentationStyle = .none
    return sharingController
  }

  func makeCoordinator() -> CloudkitShareView.Coordinator {
    Coordinator()
  }
  
  final class Coordinator: NSObject, UICloudSharingControllerDelegate {
    func cloudSharingController(_ csc: UICloudSharingController, failedToSaveShareWithError error: Error) {
      debugPrint("Error saving share: \(error)")
    }
    func itemTitle(for csc: UICloudSharingController) -> String? {
      "Sharing Example"
    }
  }
}
```

## ShareView.swift (or any View that you wish)

The last but not least, is to load the data onto your view and link the cloudkit sharing view with some button or function.

First, get the ViewModel as an EnvironmentObject and name it as viewModel.

```
@EnvironmentObject private var viewModel: CloudkitUserViewModel
@State private var isSharing = false
@State private var isProcessingShare = false
@State private var activeShare: CKShare?
@State private var activeContainer: CKContainer?
```

Next, make an asynchronous function to load data from cloudkit.

```
private func loadFriends() async throws {

switch viewModel.state {
  case let .loaded(me: me, friends: friends):
    // you can use the loaded data as you wish
            
  case .error(_):
    return
            
  case .loading:
    return
            
  }
}
```

Lastly, make a function to connect your view to the cloudkit sharing view.

```
private func shareUser(user: CloudkitUser, shareTouched: Int) async throws {
  isProcessingShare = true
  try await viewModel.refresh()
        
  do {
    let (share, container) = try await viewModel.fetchOrCreateShare(user: user)
    isProcessingShare = false
    activeShare = share
    activeContainer = container
    isSharing = true
    
    if (shareTouched == 1) {
      isSharing = false
      showError = true
    }
  } catch {
    debugPrint("Error sharing contact record: \(error)")
  }
}
    
private func shareView() -> CloudkitShareView? {
  guard let share = activeShare, let container = activeContainer else { return nil }
  return CloudkitShareView(container: container, share: share)
}
```

**Here is the full code for our app, and you will be able to find the files you were looking for.**

```
https://github.com/shinehardd/Cleanny-iOS?source=post_page-----94536ad61133
```

### 🚨**Final: If your Cloudkit does not work after App Store distribution**

Check if your Cloudkit Database is deployed for production. In many cases, it may be available only for development.

![[Screenshot 2025-09-03 at 18.59.18.png]]

If the message just below your cloudkit name informs that it has not been deployed for production, click the squared button on the left bottom and make changes.

Still, changes might not be made due to schema problems, then add the following to your AppDelegate.swift and try again.

```
let options = NSPersistentCloudKitContainerSchemaInitializationOptions()
try? container.initializeCloudKitSchema(options: options)
```

