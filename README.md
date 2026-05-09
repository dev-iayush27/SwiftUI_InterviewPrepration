# SwiftUI InterviewPrepration

## 1. SwiftUI vs UIKit (Major Differences)

| Point                         | SwiftUI                                                                      | UIKit                                                                   |
| ----------------------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **1. Programming Style**      | Declarative framework — describes *what* UI should look like                 | Imperative framework — describes *how* UI should be created and updated |
| **2. Introduced**             | Introduced in 2019 (iOS 13)                                                  | Introduced in 2008                                                      |
| **3. Code Structure**         | Requires less boilerplate code and is more concise                           | Requires more code for UI setup and management                          |
| **4. UI Updates**             | Automatically updates UI when state changes                                  | Developers manually update UI components                                |
| **5. State Management**       | Built-in state management using `@State`, `@Binding`, `@ObservedObject` etc. | No built-in reactive state management                                   |
| **6. Layout System**          | Uses `VStack`, `HStack`, `ZStack` for layouts                                | Uses AutoLayout and constraints                                         |
| **7. Preview Support**        | Supports live preview in Xcode Canvas                                        | No live preview support                                                 |
| **8. Navigation**             | Uses `NavigationStack` and `NavigationLink`                                  | Uses `UINavigationController` and `pushViewController`                  |
| **9. Backward Compatibility** | Supports iOS 13 and above                                                    | Supports much older iOS versions                                        |
| **10. Industry Usage**        | Preferred for modern apps and new features                                   | Widely used in legacy and enterprise applications                       |

## 2. SwiftUI App Life Cycle

SwiftUI lifecycle defines:

* How the app starts
* How views are created
* How app state changes are managed
* How app responds to foreground/background events

Before SwiftUI, UIKit apps used:

* AppDelegate
* SceneDelegate

SwiftUI simplified this using:

* `App` protocol
* `Scene`
* State-driven lifecycle

---

### What is SwiftUI App Lifecycle?

SwiftUI app lifecycle is the modern application entry and management system based on the `App` protocol.

Instead of manually configuring:

* Window
* Root ViewController
* Scene management

SwiftUI automatically handles them.

---

### Entry Point of SwiftUI App

The app starts from a structure conforming to `App`.

## Example

```ruby
import SwiftUI

@main
struct MyApp: App {

    var body: some Scene {

        WindowGroup {
            ContentView()
        }
    }
}
```

---

### Understanding Each Part

---

#### @main

```ruby
@main
```

Marks the starting point of the application.

Equivalent to:

* `main.swift`
* App launch entry point

---

#### App Protocol

```ruby
struct MyApp: App
```

The `App` protocol manages:

* App initialization
* Scene creation
* App lifecycle events

Equivalent to:

* AppDelegate responsibilities

---

#### Scene

```ruby
var body: some Scene
```

A Scene represents:

* One instance of UI
* App window/interface

---

#### WindowGroup

```ruby
WindowGroup {
    ContentView()
}
```

Creates a window containing the root view.

Equivalent to:

* UIWindow
* RootViewController setup

---

### UIKit Lifecycle vs SwiftUI Lifecycle

| UIKit              | SwiftUI      |
| ------------------ | ------------ |
| AppDelegate        | App protocol |
| SceneDelegate      | Scene        |
| UIWindow           | WindowGroup  |
| RootViewController | Root View    |
| viewDidLoad        | onAppear     |
| viewWillDisappear  | onDisappear  |

---

### SwiftUI View Lifecycle

Each SwiftUI View has its own lifecycle.

Main lifecycle methods:

* `init()`
* `body`
* `onAppear`
* `onDisappear`

---

### init() in SwiftUI

Called when view is initialized.

#### Example

```ruby
struct ContentView: View {

    init() {
        print("View Initialized")
    }

    var body: some View {
        Text("Hello")
    }
}
```

---

### body Property

```ruby
var body: some View
```

Defines UI layout.

Important:

* Recomputed whenever state changes
* Not called only once

SwiftUI rebuilds UI frequently.

---

### onAppear()

Called when view appears on screen.

Equivalent to:

* `viewDidAppear`
* `viewWillAppear`

#### Example

```ruby
Text("Welcome")
    .onAppear {
        print("View Appeared")
    }
```

---

### Common Uses

* API calls
* Analytics
* Start animations
* Load data

---

### onDisappear()

Called when view disappears.

Equivalent to:

* `viewDidDisappear`

#### Example

```ruby
Text("Details")
    .onDisappear {
        print("View Disappeared")
    }
```

---

### Common Uses

* Stop timers
* Cancel API requests
* Cleanup resources

---

### Complete Lifecycle Example

```ruby
struct ContentView: View {

    init() {
        print("Init Called")
    }

    var body: some View {

        Text("SwiftUI Lifecycle")
            .onAppear {
                print("onAppear Called")
            }
            .onDisappear {
                print("onDisappear Called")
            }
    }
}
```

---

### App States in SwiftUI

Apps can move through states:

| State      | Description                  |
| ---------- | ---------------------------- |
| Active     | App is running in foreground |
| Inactive   | Temporary interruption       |
| Background | App running in background    |
| Suspended  | App frozen by system         |

---

### ScenePhase in SwiftUI

SwiftUI provides `scenePhase` to observe app state changes.

#### Example

```ruby
@Environment(\.scenePhase)
private var scenePhase
```

---

### Detect App State Changes

#### Example

```ruby
import SwiftUI

@main
struct DemoApp: App {

    @Environment(\.scenePhase)
    private var scenePhase

    var body: some Scene {

        WindowGroup {
            ContentView()
        }
        .onChange(of: scenePhase) { newPhase in

            switch newPhase {

            case .active:
                print("App Active")

            case .inactive:
                print("App Inactive")

            case .background:
                print("App Background")

            @unknown default:
                break
            }
        }
    }
}
```

---

### ScenePhase States

---

#### .active

App is:

* Open
* Interactive
* Foreground

Example:

* User actively using app

---

#### .inactive

Temporary interruption.

Example:

* Phone call
* Notification popup

---

#### .background

App moved to background.

Example:

* User pressed Home button

---

### Multiple Scenes Support

SwiftUI supports multiple windows/scenes.

Especially useful in:

* iPadOS
* macOS

Each scene manages independent UI state.

---

### AppDelegate in SwiftUI

SwiftUI can still use AppDelegate if needed.

#### Example

```ruby
class AppDelegate: NSObject, UIApplicationDelegate {
    func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions:
        [UIApplication.LaunchOptionsKey : Any]? = nil
    ) -> Bool {
        print("App Launched")
        return true
    }
}
```

Use in SwiftUI:

```ruby
@UIApplicationDelegateAdaptor(AppDelegate.self)
var appDelegate
```

---

### When AppDelegate is Still Needed

Sometimes needed for:

* Push Notifications
* Firebase setup
* Deep Linking
* Third-party SDKs
* App launch handling

---

### View Recreation in SwiftUI

Important interview concept.

SwiftUI Views are:

* Structs
* Lightweight
* Frequently recreated

Therefore:

* Never rely heavily on init()
* Use state properly

---

### SwiftUI Lifecycle Flow

#### App Launch Flow

```text
@main
   ↓
App Protocol
   ↓
Scene Creation
   ↓
WindowGroup
   ↓
Root View
   ↓
body Rendered
   ↓
onAppear()
```

---

### UIKit vs SwiftUI Lifecycle Comparison

| UIKit Lifecycle     | SwiftUI Lifecycle |
| ------------------- | ----------------- |
| AppDelegate         | App               |
| SceneDelegate       | Scene             |
| viewDidLoad         | init              |
| viewWillAppear      | onAppear          |
| viewDidDisappear    | onDisappear       |
| UIApplication State | ScenePhase        |

---
