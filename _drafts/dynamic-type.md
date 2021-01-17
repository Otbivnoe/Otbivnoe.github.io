---
layout: post
title: "On a way to great accessibility with Dynamic Type"
subtitle: "Sir, put off your monocle, today we will talk about Dynamic Type! Accessibility plays a crucial role in all apps by far, so designing an app keep text size and layout in mind for clarity and readability."
date: 2021-01-10 15:47:12 +0300
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

Developers write apps for users obviously and about **25 percent** of them use non-default text size. Having discovered these statistics reading the [PSPDFKit blog](https://pspdfkit.com/blog/2018/improving-dynamic-type-support/#user-adoption) and consider the fact Apple has moved the "Accessibility" settings one level up from the "General" tab in iOS 14 I was really surprised. So decided to add the metrics in the currently developing app. With the received similar results I went to a manager so that a ticket for implementing this was created.

<picture>
    <source media="(prefers-color-scheme: dark)" srcset="/assets/img/articles/dynamic-type/statistics-dark.png">
    <img class="dynamic-type-chart centered" srcset="/assets/img/articles/dynamic-type/statistics-light.png" alt="Dynamic Type statistics (chart)">
</picture>

Just for clarification, the **Accessibility fonts** are an additional group with even larger text sizes for users with accessibility needs. Usually, looking at the Apple apps for these fonts not only changing the text size but apply some layout changes - you will see it later.

<hr>

Now knowing why it's important to pay attention to it, let's summarize the most important practices in my opinion:

- **Apple in its HIG recommends setting the minimum size for Body text to be 17pt.**

- **As font size increases, avoid truncating text.** It’s best when people can simply scroll to see the same amount of text that’s visible at the default font size. Don’t truncate text unless people can open a separate view to read the rest of the content.

- **Keep in mind the Accessibility fonts.** Group with even larger text sizes usually requires layout changes.

- **Increase the size of meaningful glyphs as font size increases.** If you use glyphs to communicate important information, make sure the glyphs are easy to view at larger font sizes, too.

- **Maintain a consistent information hierarchy regardless of the user’s font size choice.** For example, keep primary elements towards the top of the screen even when the font size is very large, so that people don’t lose track of these elements.

- **Make sure custom fonts are legible.** Custom typefaces can sometimes be difficult to read. Unless your app has a compelling need for a custom font, such as for branding purposes or to create an immersive gaming experience, it’s usually best to use the system fonts. If you do use a custom font, make sure it’s easy to read, even at small sizes.

More is available on [uxdesigh](https://uxdesign.cc/guide-for-designing-better-mobile-apps-typography-5796495ef86f) and [HIG](https://developer.apple.com/design/human-interface-guidelines/accessibility/overview/text-size-and-weight/).

<hr>

At first, it seems effortless adapting Dynamic Type. If you follow the auto-layout guidelines properly, avoid using constant heights for UI elements, it should work as expected without any additional coding. But there's a little number of users with accessibility fonts and you have to bear in mind them. 

**Important to note** that at the time of writing this article iOS 14.3 is the most actual one and all further examples will be related to this version.

So let's discover how to adapt the Accessibility UI in the right way!

### 1. Phone app - Favorites tab 

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/contacts-favourites.png" alt="Dynamic Type UI example">

You can observe the following changes:
- Increased size of glyphs in conjunction with the text.
- Removed avatars for getting extra space for large text size (**accessibility**).
- Thick separators (**accessibility**).

It is worth mentioning, you should not get rid of images all-time in similar UI. In some cases, a user does pay more attention to an image for finding needed info rather than reading text. As I said, it's not as easy as it might seem :)

Points with the **accessibility** label related only to extra large fonts.

### 2. Phone app - Recents tab 

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/contacts-recents.png" alt="Dynamic Type UI example">

For simplicity, I will omit the general UI changes and describe the new ones:

- The time position is changed, again - for extra space. 
- And the same with the phone icon.

Further example with the Mail app will demonstrate not only adjusting the position of a label but changing a color too.

### 3. Mail app

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/mail.png" alt="Dynamic Type UI example">

With the above in place, this time label ("Yesterday") has an adjusted color. It looks a lot better even with such small changes. 

**Keep in my the contrast of elements!**

### 4. Phone app - Contact tab 

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/tim.png" alt="Dynamic Type UI example">

One more example of adapting to the vertical-style UI.

### 5. Placeholdes and contextual menus

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/placeholders.png" alt="Dynamic Type UI example">

You may have noticed some non-changing elements, namely the tab bar, and the navigation bar. Since generally, they are static-text elements and not representing the main content, they stay with the same size or just with minor changes. For native elements, Apple prepared placeholders. When a user holds a finger over an element, this placeholder appears. It's a win-win for developers and users, so keep in mind when planning to implement a custom tab bar or navigation controllers. 

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/photos.png" alt="Dynamic Type UI example">

Actually, it's ok to apply contextual menus for custom elements as well, so the photos app does.

p.s. it looks buggy, but my point is clear :)

### 6. Magic switcher

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/switcher.png" alt="Dynamic Type UI example">

The last one - the most confusing. Recently discovered the differences in different iOS versions and no ideas either it's a bug in iOS 12 or just a regression in iOS 13. Or maybe just the awareness of better readability, who knows. I've tried to find an explanation for this behavior with no luck. So if you have information about this - please, [tell me](/contact).

<hr>

It's time to dive into the practice! 

I won't show step by step tutorial of implementing Dynamic Type in your app, but I'm going to cover some hacks and tips for simplify this process. If it's really your first meeting with this topic I highly recommend to get familiar with [this small article](https://developer.apple.com/documentation/uikit/uifont/scaling_fonts_automatically) at first.


-  As I said before, primarily is a collecting statistics - you need the [preferredContentSizeCategory](https://developer.apple.com/documentation/uikit/uiapplication/1623048-preferredcontentsizecategory):

```swift
UIApplication.shared.preferredContentSizeCategory
```

- [SF Symbols](https://developer.apple.com/design/human-interface-guidelines/sf-symbols/overview/) were introduced during WWDC 2019 and it's a huge gift for us! Over 2,400 consistent, highly configurable symbols. Apple designed SF Symbols to integrate seamlessly with the San Francisco system font, so the symbols automatically ensure optical vertical alignment with text in all weights and sizes. 

As you can see the image has the same style using a symbol configuration. So the image is growing alongside the label automatically.

```swift
let configuration = UIImage.SymbolConfiguration(textStyle: .body)
let image = UIImage(systemName: "bookmark", withConfiguration: configuration)
label.font = UIFont.preferredFont(forTextStyle: .body)
imageView.image = image
```

SwiftUI is more friendly - provides Dynamic Type out of the box:

```swift
// .body style - the default
Image(systemName: "bookmark")
Text("Interesting article")

Image(systemName: "bookmark").font(.largeTitle)
Text("Interesting article").font(.largeTitle)
```

- This is more useful for iPads, and since they are gaining popularity these days by far, so more apps will adapt UI for new realities, we need to talk about it a bit.

[Readable Content Guide](https://developer.apple.com/documentation/uikit/uiview/1622644-readablecontentguide) - our next fellow, a layout guide defines an area that can easily be read without forcing users to move their head to track the lines.

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/readable-guides.png" alt="Dynamic Type UI example">

```swift
let guide = view.readableContentGuide
label.topAnchor.constraint(equalTo: guide.topAnchor).isActive = true
label.leftAnchor.constraint(equalTo: guide.leftAnchor).isActive = true
label.rightAnchor.constraint(equalTo: guide.rightAnchor).isActive = true
```

p.s. I haven't found a similar in SwiftUI, except padding with different values. So would appreciate if you tell me how to configure it right.