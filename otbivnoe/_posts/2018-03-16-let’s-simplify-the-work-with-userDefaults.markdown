---
layout: post
title:    "Let’s Simplify the Work with UserDefaults"
subtitle: "Everyone has worked with UserDefaults in order to store some simple data and knows that working with that storage is easy as it can be. But today I’m going to improve the interaction with it a bit though!"
date:     "2018-03-16 15:47:12 +0300"
---

![userdefaults](/assets/images/userdefaults.png)

Everyone has worked with *UserDefaults* in order to store some simple data and knows that working with that storage is easy as it can be. But today I’m going to improve the interaction with it a bit though! Let’s start with the most obvious solution and implement something new and elegant. 😌

Imagine we have some service — *SettingsService*. This service knows about the app’s settings — which theme is used (dark, light), whether notifications are enabled and so on. To implement it the majority of developers will think about the *UserDefaults* at first. Of course, it depends on the case, but let’s simplify it.
Our first simplest solution:

<span>1.</span> Our first simplest solution:

```swift
class SettingsService {

    private enum Keys {
        static let isNotificationsEnabled = "isNotificationsEnabled"
    }

    var isNotificationsEnabled: Bool {
        get {
            let isEnabled = UserDefaults.standard.value(forKey: Keys.isNotificationsEnabled) as? Bool
            return isEnabled ?? true
        }
        set {
            UserDefaults.standard.setValue(newValue, forKey: Keys.isNotificationsEnabled)
        }
    }
}
```

For simplifying, I explicitly use UserDefaults.standard but in a real project you’d better to store it on a property and use DI of course.

<span>2.</span> The next step what I want to take is to get rid of *Keys* enum — use the `#function` instead:


```swift
class SettingsService {

    var isNotificationsEnabled: Bool {
        get {
            let isEnabled = UserDefaults.standard.value(forKey: #function) as? Bool
            return isEnabled ?? true
        }
        set {
            UserDefaults.standard.setValue(newValue, forKey: #function)
        }
    }
}
```

Looks better! Let’s go further :)

<span>3.</span> Subscript time! We’ve just wrapped the `value(forKey:)` function into a subscript with a generic:

```swift
extension UserDefaults {

    subscript<T>(key: String) -> T? {
        get {
            return value(forKey: key) as? T
        }
        set {
            set(newValue, forKey: key)
        }
    }
}

class SettingsService {

    var isNotificationsEnabled: Bool {
        get {
            return UserDefaults.standard[#function] ?? true
        }
        set {
            UserDefaults.standard[#function] = newValue
        }
    }
}
```

It already looks pretty neat! But what about *Enums*? 🤔

```swift
enum AppTheme: Int {
    case light
    case dark
}

class SettingsService {

    var appTheme: AppTheme {
        get {
            if let rawValue: AppTheme.RawValue = UserDefaults.standard[#function], let theme = AppTheme(rawValue: rawValue) {
                return theme
            }
            return .light
        }
        set {
            UserDefaults.standard[#function] = newValue.rawValue
        }
    }
}
```

There’s a place for refactoring!

<span>4.</span> Let’s write a similar subscript only for RawRepresentable values:

```swift
extension UserDefaults {
    
    subscript<T: RawRepresentable>(key: String) -> T? {
        get {
            if let rawValue = value(forKey: key) as? T.RawValue {
                return T(rawValue: rawValue)
            }
            return nil
        }
        set {
            set(newValue?.rawValue, forKey: key)
        }
    }
}

class SettingsService {
    
    var appTheme: AppTheme {
        get {
            return UserDefaults.standard[#function] ?? .light
        }
        set {
            UserDefaults.standard[#function] = newValue
        }
    }
}
```

Ready for production! Please, be aware that this extension is only applied for enums with *RawRepresentable* presentation.

I hope you enjoyed my extensions! If you know any ways of improving it — let me know! There’s a [final extension](https://gist.github.com/Otbivnoe/04b8bd7984fba0cb58ca7f136fd95582) on UserDefaults. Feel free to test it out! :)

{% include readaggregator_footer.html %}