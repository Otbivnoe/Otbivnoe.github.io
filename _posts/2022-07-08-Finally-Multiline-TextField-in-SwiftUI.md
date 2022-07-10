---
layout: post
title: "Multiline TextField in SwiftUI"
subtitle: "Here it is! How many times did I try to find an answer on Stackoverflow, uhm? No more custom multiline text fields using TextEditor. Starting with iOS 16 we can achieve it using TextField just by adding a new parameter axis."
date: 2022-07-10 10:00:00 +0300
tags: iOS16+, SwiftUI
badge: Quick tip
---

Here it is! How many times did I try to find an [answer](https://stackoverflow.com/search?q=multiline+TextField+SwiftUI) on Stackoverflow, uhm? No more custom multiline text fields using `TextEditor`. Starting with **iOS 16** we can achieve it using [TextField](https://developer.apple.com/documentation/swiftui/textfield) just by adding a new parameter `axis`:

```swift
TextField("Multiline textfield", text: $text, axis: .vertical)
```

This argument controls the scroll direction when the text is large than the text field bounds. Using the `vertical` value we get a new-modern multiline text field, but by specifying the `horizontal` one we return to the behaviour we are all already get used to.

<img class="centered post-img" srcset="/assets/img/articles/multiline-textfield/multiline-textfield-1.gif" alt="">

See how smooth it works. This text field is not only multiline now, but it automatically expands and shrinks itself on content changes. If endlessly expanded content is not what you expect to see, old `lineLimit` with new handy friends are for rescue!
 
Using this modifier a text field grows until it exceeds the limited number of lines. There're a few ways to specify how many **visible** text lines you need. Using the [old one](https://developer.apple.com/documentation/swiftui/defaultdateprogresslabel/linelimit(_:)-3c4c9) you only configure the maximum number of lines that text can occupy in this view.

```swift
// iOS 13+

TextField("Multiline textfield", text: $text, axis: .vertical)
    .lineLimit(2)
```

Starting with **iOS 16** there're a few more with the configuration of a minimum and maximum number of lines as well.

```swift
// iOS 16+

TextField("Multiline textfield", text: $text, axis: .vertical)
    .lineLimit(...2) // It's identical to usual `lineLimit(2)`
    .lineLimit(2...) // min=2, max=Int.max
 
TextField("Multiline textfield", text: $text, axis: .vertical)
    .lineLimit(2...3) // min=2, max=3
 
TextField("Multiline textfield", text: $text, axis: .vertical)
    .lineLimit(2, reservesSpace: true)  // = `lineLimit(2...2)`
    .lineLimit(2, reservesSpace: false) // = `lineLimit(2)`
 ```
 
 Although all these features sound cool, need to remember about iOS 16. So create yourself a reminder about this feature in a few years :)