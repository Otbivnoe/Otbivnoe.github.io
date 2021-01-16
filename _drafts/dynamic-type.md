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

(подумать еще)

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

### 5. Navigation bar and Tab bar 

<img class="centered post-img" srcset="/assets/img/articles/dynamic-type/examples/placeholders.png" alt="Dynamic Type UI example">