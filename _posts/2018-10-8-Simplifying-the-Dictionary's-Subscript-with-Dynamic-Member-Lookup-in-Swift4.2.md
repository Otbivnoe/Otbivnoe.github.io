---
layout: post
title: "Simplifying the Dictionary’s Subscript with Dynamic Member Lookup in Swift 4.2"
subtitle: "Apple’s added some great features in Swift 4.2 release, so in this post, we’re going to use some of them to achieve a neat key-syntax feature for the Dictionary, but before reading further you by far have to know about the Dynamic Member Lookup."
date: 2018-10-08 10:47:12 +0300
image: /assets/img/articles/dynamic-lookup.png
---

Apple’s added some [great features](https://www.hackingwithswift.com/articles/77/whats-new-in-swift-4-2) in Swift 4.2 release, so in this post, we’re going to use some of them to achieve a neat key-syntax feature for the Dictionary, but before reading further you by far have to know about the [Dynamic Member Lookup](https://github.com/apple/swift-evolution/blob/master/proposals/0195-dynamic-member-lookup.md). I recommend that you to get acquainted with this feature by the first link — there’re great examples in a simple form.

**TL;DR** “At the core of this feature is a new attribute called `@dynamicMemberLookup`, which instructs Swift to call a subscript method when accessing properties. This subscript method, `subscript(dynamicMember:)`, is required: you’ll get passed the string name of the property that was requested, and can return any value you like.”

```swift
@dynamicMemberLookup
struct Person {
    subscript(dynamicMember member: String) -> String {
        let properties = ["name": "Nikita", "age": "24"]
        return properties[member, default: ""]
    }
}

print(Person().name) // Nikita
```

<hr>

> *This article doesn’t imply some good practices, and a final solution you’ll see later isn’t supposed to be used in production code — no doubt it can confuse other developers from your team and can lead to error-prone code, so it’s better to use the standard approach with constant keys.*
<br><br>
> *But, it’s definitely worth to get acquainted with this feature this way — trying to come up with something new, as in this post! So, Swift language is becoming more powerful day by day!*
<br><br>
> *Even so, I like this feature and I’m going to use it in my pet projects or during fast-prototyping demos :)*

<hr>

Before diving into our implementation, it’s important to know about this Swift’s feature as well:

```swift
protocol ProtocolWithAssociatedType {
    associatedtype T
}

struct Test<T>: ProtocolWithAssociatedType {} // no compile error
struct Test: ProtocolWithAssociatedType {}    // error: Type 'Test' does not conform to protocol 'ProtocolWithAssociatedType'
```

You can see, that the struct with a generic type auto-conforms to the protocol due to the same `T` name — it’s not necessary writing `typealias T = ...` by yourself.

Now you should not be confused with this code:

```swift
@dynamicMemberLookup
protocol DictionaryDynamicLookup {
    associatedtype Key
    associatedtype Value
    subscript(key: Key) -> Value? { get }
}

extension DictionaryDynamicLookup where Key == String {
    subscript(dynamicMember member: String) -> Value? {
        return self[member]
    }
}

extension Dictionary: DictionaryDynamicLookup {}

let dict: [String : Any] = [
    "name": "Nikita",
    "age": 24
]

print(dict.name)
print(dict.age)
```

The following snippet allows you writing `dict.name` instead of `dict["name"]`, that’s it.

**p.s. You may be surprised, but there’s no autocompletion for the name, of course.**

Now we have the opportunity to take advantage of another recently released feature — [KeyPath](https://www.klundberg.com/blog/swift-4-keypaths-and-you/) (Swift 4.0) and use the same keypath if the `name`, for instance, **has been referred several times to avoid an error in duplication.**

```swift
func printData(from keyPaths: KeyPath<[String: Any], Any?>...) {
    keyPaths.forEach { keyPath in
        print(dict[keyPath: keyPath])
    }
}

printData(from: \.name, \.age)

// Optional("Nikita")
// Optional(24)
```

But of course, this all is only for fun!

Related to good examples — check out the [JSON example](https://github.com/apple/swift-evolution/blob/master/proposals/0195-dynamic-member-lookup.md#example-usage).

<hr>

In conclusion, I want to say that playing with new features of the language is amazing, you can come up with so pretty *syntax-sugar*, but you need to be careful with that, not every sugar is needed. 😉