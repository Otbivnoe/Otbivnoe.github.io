---
layout: post
title: "Automating RawRepresentable Conformance with Swift Macros"
subtitle: "When creating a travel-planner app, it's essential for users to provide information about their trips. One of the required fields is the visa type. To handle various visa types, we can utilize an enumeration called Visa"
date: 2023-06-13 22:00:00 +0800
image: /assets/img/articles/swift-macros/logo.png
tags: Swift Macros
---

When creating a travel-planner app, it's essential for users to provide information about their trips. One of the required fields is the visa type. To handle various visa types, we can utilize an enumeration called Visa:

```swift
enum Visa {
    case tourist
    case business
    case student
}
```

However, considering the possibility of new visa types being introduced in the future, we need a fallback a case that can accommodate any unrecognized visa types. This is where the `other` case comes into play:

```swift
enum Visa {
    // ...
    case other(String)
}
```

Now, here comes the problem: how do we store this enum in a persistence storage solution like Core Data? Unfortunately, Core Data doesn't provide built-in support for enums with associated values. One approach that comes to mind is making the Visa enum conform to `RawRepresentable` and save an underlying value. However, since enums with associated values don't offer a default implementation for `RawRepresentable`, we need to manually implement it ourselves:

```swift
enum Visa: RawRepresentable {
    var rawValue: String {
        switch self {
        case .tourist:
            return "tourist"
        case .business:
            return "business"
        case .student:
            return "student"
        case .other(let value):
            return value
        }
    }

    init?(rawValue: String) {
        switch rawValue {
        case "tourist":
            self = .tourist
        case "business":
            self = .business
        case "student":
            self = .student
        default:
            self = .other(rawValue)
        }
    }
}
```

With this implementation, we can now store and retrieve visa types seamlessly using Core Data or any other persistence storage mechanism. 

<hr>

We all know developers are clever at finding shortcuts. But what happens when we need to duplicate the same logic for different types? That's why Swift has introduced a feature called Macros. It automates the whole process, saving us lazy developers tons of time and effort. If you're not familiar with Macros, I suggest checking out this [WWDC session](https://developer.apple.com/wwdc23/10166) or this [introduction article](https://www.avanderlee.com/swift/macros/) by Antoine before diving into the rest of this article.

Today we'll discuss the attached macros: 

_Attached macros provide a way to extend Swift by creating and extending declarations based on arbitrary syntactic transformations on their arguments. They make it possible to extend Swift in ways that were only previously possible by introducing new language features, helping developers build more expressive libraries and eliminate extraneous boilerplate._

Before we dive into the implementation details, let's start by adding the Macro itself:

```swift
public enum StringRepresentationMacro {}
```

With that in place, we can move on to the first step, which is adding protocol conformance to our type. To accomplish this, we'll utilize the `ConformanceMacro` protocol. This protocol describes a macro that can add conformance to the declaration it's attached to.

In the following code snippet, we'll use the `expansion` function to add the `RawRepresentable` conformance to our type. We keep it simple and only specify a single conformance to `RawRepresentable`, omitting any additional constraints (where clause) by returning `nil`.

```swift
extension StringRepresentationMacro: ConformanceMacro {
    static func expansion<Declaration, Context>(...) throws -> [(TypeSyntax, GenericWhereClauseSyntax?)] where (...) {
        return [
            (TypeSyntax("RawRepresentable"), nil)
        ]
    }
}
```

To make our macro accessible, we need to add the `@attached(conformance)` attribute to the public declaration:

```swift
@attached(conformance)
public macro StringRawRepresentation() = #externalMacro(module: "MyMacros", type: "StringRepresentationMacro")
```

If we try to expand our Macro in Xcode using `Expand Macro` option we'll see the following generated code:

```swift
extension Visa : RawRepresentable  {}
```

Now that we've added the conformance, the next step is to implement the `RawRepresentable` protocol itself. To achieve this, we'll utilize the `MemberMacro` protocol, which defines new members for the declaration it's attached to. Similar to the `ConformanceMacro`, there's a single function for implementing:

```swift
extension StringRepresentationMacro: MemberMacro {
    public static func expansion<Declaration, Context>(of node: AttributeSyntax,
                                                       providingMembersOf declaration: Declaration,
                                                       in context: Context) throws -> [DeclSyntax]
    where Declaration : DeclGroupSyntax, Context : MacroExpansionContext {
        ...
    }
}
```

First, we must ensure that the type we're adding the macro to is an enum. If it's not, we need to throw an error about the unsupported declaration:

```swift
enum MacroError: Error, CustomStringConvertible {
    case message(String)

    var description: String {
        switch self {
        case .message(let text):
            return text
        }
    }
}

guard let enumDeclaration = declaration.as(EnumDeclSyntax.self) else {    
    throw MacroError.message("@StringRawRepresentation only works with Enums")
}
```

Next, we obtain all the enum cases to iterate through:

```swift
let cases = enumDeclaration.memberBlock.members
    .compactMap {
        $0.decl.as(EnumCaseDeclSyntax.self)
    }
    .flatMap {
        $0.elements
    }
```

To generate an initializer conformance, we need to iterate through each case, and add a mapping to a string representation. To keep things simple for this article, we'll assume that there's only a single case with a String associated type. However, it's important to note that you can customize and handle different edge cases and present a validation error for a developer as we did before:

```swift
let initializer = try InitializerDeclSyntax("init?(rawValue: String)") {
    try SwitchExprSyntax("switch rawValue") {
        for element in cases {
            if element.associatedValue == nil {
                SwitchCaseSyntax(
                    """
                    case "\(element.identifier)":
                        self = .\(element.identifier)
                    """
                )
            } else {
                SwitchCaseSyntax(
                    """
                    default:
                        self = .\(element.identifier)(rawValue)
                    """
                )
            }
        }

    }
}
```

Using the similar techniques we'll generate a variable conformance: 

```swift
let variable = try VariableDeclSyntax("var rawValue: String") {
    try SwitchExprSyntax("switch self") {
        for element in cases {
            if element.associatedValue == nil {
                SwitchCaseSyntax(
                """
                case .\(element.identifier):
                    return "\(element.identifier)"
                """
                )
            } else {
                SwitchCaseSyntax(
                    """
                    case .\(element.identifier)(let value):
                        return value
                    """
                )
            }
        }
    }
}
```

Lastly, we can simply return these two declarations:

```swift
return [
    DeclSyntax(variable),
    DeclSyntax(initializer)
]
```

and add a second `attached` attribute:

```swift
@attached(member, names: named(rawValue), named(init))
@attached(conformance)
public macro StringRawRepresentation() = #externalMacro(module: "MyMacros", type: "StringRepresentationMacro")
```

That's it! Take a moment to appreciate the beauty of this code:

```swift
@StringRawRepresentation
enum Visa {
    case tourist
    case business
    case student
    case other(String)
}
```

 With the implementation of attached macros, all the tedious work is now handled seamlessly under the hood. As developers, we can truly appreciate the time and effort saved by leveraging these powerful features of Swift. Our code is now more elegant, concise, and maintainable, allowing us to focus on the core logic of our applications.

<hr>

### References

- [Write Swift Macros, WWDC23](https://developer.apple.com/wwdc23/10166)
- [Swift Macros: Extend Swift with New Kinds of Expressions](https://www.avanderlee.com/swift/macros/)
- [Proposal: Attached Macros](https://github.com/apple/swift-evolution/blob/main/proposals/0389-attached-macros.md)
- [Awesome Swift Macros](https://github.com/artemnovichkov/awesome-swift-macros)
