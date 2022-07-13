---
layout: post
title: "Enumerated ForEach in SwiftUI"
subtitle: "Today I'm faced with a fairly common task - implementing an enumerated ForEach with indexes in SwiftUI. At first it seemed straightforward, and it was, but with some small pitfalls that I want to highlight in this post. So, without further ado, let's dive into."
date: 2022-07-13 10:00:00 +0300
tags: SwiftUI
image: /assets/img/articles/foreach-enumerated/logo.png
image-dark: /assets/img/articles/foreach-enumerated/logo-dark.png
---

Today I'm faced with a fairly common task - implementing an enumerated ForEach with indexes in SwiftUI. At first it seemed straightforward, and it was, but with some small pitfalls that I want to highlight in this post. So, without further ado, let's dive into.

As an idol we'll use the [ForEach](https://developer.apple.com/documentation/swiftui/foreach) from the SwiftUI library with the same protocol conformance for our own version.

```swift
public struct ForEach<Data, ID, Content> where Data : RandomAccessCollection, ID : Hashable {

    /// The collection of underlying identified data that SwiftUI uses to create
    /// views dynamically.
    public var data: Data

    /// A function to create content on demand using the underlying data.
    public var content: (Data.Element) -> Content
}
```

The first implementation is the following:

```swift
struct ForEachEnumerated<Data, Content>: View where Data: RandomAccessCollection, Data.Element: Hashable, Content: View {

    var data: Data
    var content: (Int, Data.Element) -> Content

    var body: some View {
        ForEach(Array(data.enumerated()), id: \.element) { index, element in
            content(index, element)
        }
    }
}
```

It does the desired behavior, but there's something we need to be aware of.

_When you enumerate a collection, the integer part of each pair is a counter for the enumeration, but is not necessarily the index of the paired value. These counters can be used as indices only in instances of zero-based, integer-indexed collections, such as Array and ContiguousArray. For other collections the counters may be out of range or of the wrong type to use as an index. To iterate over the elements of a collection with its indices, use the zip(_:_:) function._ [(documentation)](https://developer.apple.com/documentation/swift/array/enumerated())

- This version is only suitable for integer-indexed collections
- Furthermore, it can lead us to an out of range crash. Consider this example, it leads to crashing an app, since the index is not zero-based:

```swift
let array = [0, 1, 2, 3, 4]
let newArray = array[2...3]

// newArray[0] // -> CRASH
// newArray.indices // -> [2..<4]
```

With the above in mind, we should modify our version. Now the index type is the same as Data's one and indices are not zero-based anymore.

```swift
struct ForEachEnumerated<Data, Content>: View where Data: RandomAccessCollection, Data.Element: Hashable, Content: View {

    var data: Data
    var content: (Data.Index, Data.Element) -> Content

    var body: some View {
        ForEach(Array(zip(data.indices, data)), id: \.1) { index, element in
            content(index, element)
        }
    }
}
```

It already looks as a production-ready, however there's something that would be great to improve. What about this code `id: \.1`? This restricts `Data.Element` to be `Hashable` and what if I don't want to use the entire object as a identifier? 

To achieve this goal, we'll add another property for storing a keypath and a small structure (`Info`) for enumerating using id.

```swift
struct ForEachEnumerated<Data, ID, Content>: View where Data: RandomAccessCollection, ID: Hashable, Content: View {

    // For the simplicity of type reading
    typealias Index = Data.Index
    typealias Element = Data.Element

    private struct Info {
        let index: Index
        let id: KeyPath<Element, ID>
        let element: Element

        var elementID: ID {
            element[keyPath: id]
        }
    }

    var data: Data
    var content: (Index, Element) -> Content
    let id: KeyPath<Element, ID>

    var body: some View {
        let enumeratedData = zip(data.indices, data).map { index, element in
            Info(index: index, id: id, element: element)
        }

        ForEach(enumeratedData, id: \.elementID) { indexInfo in
            content(indexInfo.index, indexInfo.element)
        }
    }
}
```

Following the rules of our idol, let's extend our type with a handy initializer that's really useful for `Identifiable` data objects: 

```swift
extension ForEachEnumerated where Element: Identifiable, ID == Element.ID {
    init(_ data: Data, @ViewBuilder content: @escaping (Index, Element) -> Content) {
        self.init(data, content: content, id: \.id)
    }
}
```

In conclusion, a small example that demonstrates how it looks:

```swift
struct Article {
    let title: String
    let author: String
}

struct ArticlesView: View {
    let articles: [Article]

    var body: some View {
        List {
            ForEachEnumerated(articles, id: \.title) { index, article in
                HStack {
                    Text("\(index)" + article.title)
                    Text(article.author)
                }
            }
        }
    }
}
```