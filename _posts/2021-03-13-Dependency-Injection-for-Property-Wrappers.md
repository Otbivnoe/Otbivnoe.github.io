---
layout: post
title: "Dependency Injection for Property Wrappers"
subtitle: "This article doesn't look like a long step-by-step tutorial, it's just a quick tip for a problem I faced recently. So I decided to share a non-obvious solution for me, maybe it will save some time for someone.  Email me if the article was useful to you :)"
date: 2021-03-13 22:50:00 +0300
badge: Quick tip
image: /assets/img/articles/property-wrapper-di/logo.png
image-dark: /assets/img/articles/property-wrapper-di/logo-dark.png
---

My past sprint was nothing special. I had to implement an onboarding flow with saving a completion flag so that a user can see this for the first time only. Sounds like a typical work for iOS developers for saving something in UserDefaults, doesn't it? So, I prepared a special class with the most popular (IMHO) property wrapper inside:

_If you haven't heard about Property Wrappers so far, I recommend reading [this article](https://nshipster.com/propertywrapper/) first._

```swift
@propertyWrapper
struct UserDefault<Value> {

    private let key: String
    private let defaultValue: Value
    private let storage: UserDefaults

    var wrappedValue: Value {
        get {
            return storage.object(forKey: key) as? Value ?? defaultValue
        }
        set {
            storage.set(newValue, forKey: key)
        }
    }

    init(defaultValue: Value, key: String, storage: UserDefaults = .standard) {
        self.defaultValue = defaultValue
        self.key = key
        self.storage = storage
    }
}

class Onboarding {

    @UserDefault(defaultValue: false, key: "isOnboardingCompleted")
    var isOnboardingCompleted: Bool
}
```

Done, looks as simple as possible. The second step is to cover this code by tests. In order to run tests safely - to have stable tests, I decided to pass different domains of User Defaults. So the first code I wrote was the following one:

```swift
class Onboarding {

    @UserDefault(defaultValue: false, key: "isOnboardingCompleted", storage: storage)
    var isOnboardingCompleted: Bool

    private let storage: UserDefaults

    init(storage: UserDefaults) {
        self.storage = storage
    }
}
```

Obviously it didn't work. The red error was flashing to my eyes:

```swift
Cannot use instance member 'storage' within property initializer; property initializers run before 'self' is available
```

After that I used to try a few approaches to solve this issue without luck until I remembered one important thing: using an underscore within a class allowing to access a property wrapper class itself rather than a general property.

The final solution:

```swift
class Onboarding {

    @UserDefault
    var isOnboardingCompleted: Bool

    init(storage: UserDefaults) {
        _isOnboardingCompleted = UserDefault(defaultValue: false, key: "isOnboardingCompleted", storage: storage)
    }
}
```

<hr>

This article doesn't look like a long step-by-step tutorial, it's just a quick tip for a problem I faced recently. So I decided to share a non-obvious solution for me, maybe it will save some time for someone. [Let me know](https://twitter.com/iOtbivnoe) if the article was useful to you :)
 
