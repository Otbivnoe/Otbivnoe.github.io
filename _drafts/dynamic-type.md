---
layout: post
title: "On a way to great accessibility with Dynamic Type"
subtitle: "Sir, put off your monocle, today we will talk about Dynamic Type! Accessibility plays a crucial role in all apps by far, so designing an app keep text size and layout in mind for clarity and readability."
date: 2021-01-21 15:47:12 +0300
image: /assets/img/articles/dynamic-type/logo.png
---

<head>
<style>
    @import "colors";

    .dynamic-type-chart {
        margin-top: 40px;
        margin-bottom: 50px;
        width: 100%;
        height: auto;
    }
</style>
</head>

*Sir*, put off your monocle, today we will talk about Dynamic Type!

Accessibility plays a crucial role in all apps by far, so designing an app keep text size and layout in mind for clarity and readability.

In a nutshell, Dynamic Type lets you define a desired text size in iOS user settings and use it across apps that support it. Supporting this feature not only improve the user's experience but also train developers to write code for more dynamic interfaces - for instance, plenty of localization options.

Today I will try to cover the following topics:

- Why it's so important to pay attention to it
- Find out the best accessibility practices
- UI adaptation for different text sizes
- Check the available API for developers.

Without further ado, let's dive into the article!

<hr>

Developers write apps for users obviously and about **25 percent** of them use non-default text size. Having discovered these statistics reading the [PSPDFKit blog](https://pspdfkit.com/blog/2018/improving-dynamic-type-support/#user-adoption) and consider the fact Apple has moved the "Accessibility" settings one level up from the "General" tab in iOS 14 I was really surprised. So decided to add the metrics in the currently developing app and got the similiar results:

<picture>
    <source media="(prefers-color-scheme: dark)" srcset="/assets/img/articles/dynamic-type/statistics-dark.png">
    <img class="dynamic-type-chart centered" srcset="/assets/img/articles/dynamic-type/statistics-light.png" alt="Dynamic Type statistics (chart)">
</picture>

Just for clarification, the **Accessibility fonts** is an additional group with even larger text sizes for users with accessibility needs. Usually, looking at the Apple apps for these fonts not only changing the text size but apply some layout changes - you will see it later.

<hr>

## Best practices

Now knowing why it's important to pay attention to it, let's summarize the most important practices in my opinion:

- **Apple in its HIG recommends setting the minimum size for Body text to be 17pt.**

- **As font size increases, avoid truncating text.** It’s best when people can simply scroll to see the same amount of text that’s visible at the default font size. Don’t truncate text unless people can open a separate view to read the rest of the content.

- **Keep in mind the Accessibility fonts.** Group with even larger text sizes usually requires layout changes.

- **Increase the size of meaningful glyphs as font size increases.** If you use glyphs to communicate important information, make sure the glyphs are easy to view at larger font sizes, too.

- **Maintain a consistent information hierarchy regardless of the user’s font size choice.** For example, keep primary elements towards the top of the screen even when the font size is very large, so that people don’t lose track of these elements.

- **Make sure custom fonts are legible.** Custom typefaces can sometimes be difficult to read. Unless your app has a compelling need for a custom font, such as for branding purposes or to create an immersive gaming experience, it’s usually best to use the system fonts. If you do use a custom font, make sure it’s easy to read, even at small sizes.

More is available on [uxdesigh](https://uxdesign.cc/guide-for-designing-better-mobile-apps-typography-5796495ef86f) and [HIG](https://developer.apple.com/design/human-interface-guidelines/accessibility/overview/text-size-and-weight/).

<hr>

## Example of Accessible UI 

At first, it seems effortless adapting Dynamic Type. If you follow the auto-layout guidelines properly, avoid using constant heights for UI elements, it should work as expected without any additional coding. But there's a little number of users with accessibility fonts and you have to bear in mind them. 

**Important to note** that at the time of writing this article iOS 14.3 is the most actual one and all further examples will be related to this version.

So let's discover how to adapt the Accessibility UI in the right way!

<a id="phone-favorites-tab"></a>

#### 1. Phone app - Favorites tab 

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/contacts-favourites.png" alt="Dynamic Type UI example">

You can observe the following changes:
- Increased size of glyphs in conjunction with the text.
- Removed avatars for getting extra space for large text size (**accessibility**).
- Thick separators (**accessibility**).

It is worth mentioning, you should not get rid of images all-time in similar UI. In some cases, a user does pay more attention to an image for finding needed info rather than reading text. As I said, it's not as easy as it might seem :)

Points with the **accessibility** label related only to extra large fonts.

#### 2. Phone app - Recents tab 

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/contacts-recents.png" alt="Dynamic Type UI example">

For simplicity, I will omit the general UI changes and describe the new ones:

- The time position is changed, again - for extra space. 
- And the same with the phone icon.

Further example with the Mail app will demonstrate not only adjusting the position of a label but changing a color too.

#### 3. Mail app

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/mail.png" alt="Dynamic Type UI example">

With the above in place, this time label (**"Yesterday"**) has an adjusted color. It looks a lot better even with such small changes. 

**Keep in my the contrast of elements!**

#### 4. Phone app - Contact tab 

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/tim.png" alt="Dynamic Type UI example">

One more example of adapting to the vertical-style UI.

Generally, I really appreciate that Apple adapts all their apps to Dynamic Type carefully. It's a great place to be inspired and learn ways of adaptive layout.

#### 5. Placeholdes and contextual menus

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/placeholders.png" alt="Dynamic Type UI example">

You may have noticed some non-changing elements, namely the tab bar, and the navigation bar. Since generally, they are static-text elements and not represent the main content, they stay with the same size or just with minor changes. For native elements, Apple prepared placeholders. When a user holds a finger over an element, this placeholder appears. It's a win-win for developers and users, so keep in mind when planning to implement a custom tab bar or navigation controllers. 

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/photos.png" alt="Dynamic Type UI example">

Actually, it's ok to apply contextual menus for custom elements as well, so the photos app does.

p.s. it looks buggy, but my point is clear :)

#### 6. Magic switcher

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/switcher.png" alt="Dynamic Type UI example">

The last one - the most confusing. Recently discovered the differences in different iOS versions and no ideas either it's a bug in iOS 12 or just a regression in iOS 13. Or maybe just the awareness of better readability, who knows. I've tried to find an explanation for this behavior with no luck. So if you have information about this - please, [tell me](/contact).

<hr>

## It's time to dive into the practice! 

I won't show step by step tutorial of implementing Dynamic Type in your app, but I'm going to cover some hacks and tips for simplifying this process. If it's really your first meeting with this topic I highly recommend to get familiar with [this small article](https://developer.apple.com/documentation/uikit/uifont/scaling_fonts_automatically) at first.


-  As I said before, primarily is a collecting statistics - you need the [preferredContentSizeCategory](https://developer.apple.com/documentation/uikit/uiapplication/1623048-preferredcontentsizecategory):

> ~~~swift
UIApplication.shared.preferredContentSizeCategory
~~~

- [SF Symbols](https://developer.apple.com/design/human-interface-guidelines/sf-symbols/overview/) were introduced during WWDC 2019 and it's a huge gift for us! Over 2,400 consistent, highly configurable symbols. Apple designed SF Symbols to integrate seamlessly with the San Francisco system font, so the symbols automatically ensure optical vertical alignment with text in all weights and sizes. 

> As you can see the image has the same style using a symbol configuration. So the image is growing alongside the label automatically.

> ~~~swift
let configuration = UIImage.SymbolConfiguration(textStyle: .body)
let image = UIImage(systemName: "bookmark", withConfiguration: configuration)
imageView.image = image
> 
label.font = UIFont.preferredFont(forTextStyle: .body)
label.adjustsFontForContentSizeCategory = true
label.numberOfLines = 0
~~~

> SwiftUI is more friendly - provides Dynamic Type out of the box:

> ~~~swift
// .body style - the default
Image(systemName: "bookmark")
Text("Interesting article")
>
Image(systemName: "bookmark").font(.largeTitle)
Text("Interesting article").font(.largeTitle)
~~~

- This is more useful for iPads, and since they are gaining popularity these days by far, so more apps will adapt UI for new realities, we need to talk about it a bit.

> [Readable Content Guide](https://developer.apple.com/documentation/uikit/uiview/1622644-readablecontentguide) - our next fellow, a layout guide defines an area that can easily be read without forcing users to move their head to track the lines.

> <img class="centered post-img" srcset="/assets/img/articles/dynamic-type/readable-guides.png" alt="Dynamic Type UI example">

> The width of readable area changes according user's preferred content size.

> ~~~swift
let guide = view.readableContentGuide
label.topAnchor.constraint(equalTo: guide.topAnchor).isActive = true
label.leftAnchor.constraint(equalTo: guide.leftAnchor).isActive = true
label.rightAnchor.constraint(equalTo: guide.rightAnchor).isActive = true
~~~

> p.s. I haven't found a similar in SwiftUI, except padding with different values. So would appreciate if you tell me how to configure it right.

- In iOS 13 UIKit kicks off a prediction for initial traits of a view. UIKit guesses the likely traits for the view based on the context. So if you relied that `traitCollectionDidChange` will be called when a view is first added to the view hierarchy, it's not always true anymore. You can read the detailed article [here](https://useyourloaf.com/blog/predicting-size-classes-in-ios-13/).


> ~~~swift
let vew = UIView()
print(customVew.traitCollection.preferredContentSizeCategory) // iOS 12 - Unspecified
print(customVew.traitCollection.preferredContentSizeCategory) // iOS 13+ - AccessibilityXXL
~~~

> My point is to have a properly configured view in both places. Depends on iOS version it'll work different. So be aware of it!

- Using `automaticDimension` without any doubt is a good friend for Dynamic Type. It's required setting `estimatedRowHeight` when using `automaticDimension` as well and we can improve performance by passing a dynamic value instead of a static one.

> ~~~swift
tableView.rowHeight = UITableView.automaticDimension
tableView.estimatedRowHeight = UIFontMetrics.default.scaledValue(for: 50)
~~~

- It would seem obvious, but worth mention that using Stack View simplifies a lot in some cases. If you're building a simple setting menu, stack view will help out you by far.

> <img class="centered post-img" srcset="/assets/img/articles/dynamic-type/stackview.png" alt="Dynamic Type UI example">

>~~~swift
let stackView = UIStackView()
stackView.axis = .horizontal
~~~

>~~~swift
struct ContentView: View {
    var body: some View {
        Group {
            if sizeCategory.isAccessibilityCategory {
                VStack { ... }
            } else {
                HStack { ... }
            }
        }
    }
}
~~~

- Also worth mentioning a property wrapper that was released during WWDC20 - `ScaledMetric`. It allows you to scale a number according to the size category chosen by a user.

>~~~swift
struct ContentView: View {
    @ScaledMetric(relativeTo: .body) var spacing: CGFloat = 10
~~~
>~~~swift
    var body: some View {
        VStack(spacing: spacing) {
            (elements)
        }
    }
}
~~~

- At the end I will ask you to pay attention to the <a href="#phone-favorites-tab">first example</a> one more time. We can distinctly see the thick separators on the second picture. And because it's enough a common practice implementing custom separators on our side, a developer needs to keep in mind the native behaviour.

<hr>

## Debugging or how to make your life easier

Testing Dynamic Type can be a time-consuming process for sure - switching between Settings app for changing the font size and your app... As a result spending a lot of time to this is not what you're planning to achieve. So let's cover a few much simple ways of changing the font size on the fly.

#### Control Centre

This is a way you need when developing on a real device. Just add "Text Size" option in Control Centre. Make sure the Larger Accessibility Sizes switcher is enabled: **Setting > Accessibility > Display & Text Size > Larger Text**.

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/control-centre.png" alt="Dynamic Type UI example">

For super-duper optimization fellows, try creating a shortcut for setting a needed text size and invoke it by triple or double tap action on a back of your phone.

1. Create shortcut in Shortcut app
2. Use it in accessibility menu of Back Tap: **Settings > Accessibility > Touch > Back Tap (at the bottom) > Double / Triple Tap > (Created Shortut)**

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/tripple-tap.png" alt="Adjusting accessibility via Control Centre">

#### Accessibility Inspector

The old best friend located at **Xcode > Open Developer Tool**. Great to know that you can't only change settings for the Simulator, but also for any connected iOS device.

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/accessibility_inspector.png" alt="Screenshot of Xcode Accessibility Inspector">

#### Xcode

Xcode makes it easier overriding these settings directly from debugger and I hope everyone has already had time to get to know this.

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/xcode-dynamic-type.png" alt="Xcode Dynamic Type Setting">

#### SwiftUI

At the end it's quite simple to set environment for different previews and see multiple results in a row. But this approach will suit for real lucky mans, who already use SwiftUI in production. 😉 

~~~swift
struct ContentView_Previews: PreviewProvider {
   static var previews: some View {
      Group {          
         ContentView().environment(\.sizeCategory, .extraSmall)
         ContentView().environment(\.sizeCategory, .accessibilityLarge)
      }
   }
}
~~~

<hr>

I hope this was worth reading article for you! Dynamic Type is our good friend and we shouldn't forget it.