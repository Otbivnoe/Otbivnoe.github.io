---
title: Resume
permalink: /Resume
layout: default
---

I am a professional iOS Developer since 2014, who specializes in building high-quality native applications. Application development is not just my job, it's also my hobby and passion. I carry out all work efficiently and on time. Always glad to find new challenges and ways to surpass clients' expectations.

### Education 

2011 → 2015 Bachelor's degree (Faculty of Mathematics and Information Technology) in [Dostoevsky Omsk State University](https://omsu.ru/about/structure/general/omp/eng/)

### Professional hobbies

- Lead the [telegram channel](https://t.me/readaggregator) about programming
- I maintain own open-source projects on [Github](https://github.com/otbivnoe)
- Write [articles](/) about programming in Apple ecosystems
- Help the community answering questions at [StackOverflow](https://stackoverflow.com/users/3733734/nikita-ermolenko?tab=profile)

### Experience

- [Igloo](https://iglootest.ru): Jul 2020 → Apr 2022 (1 year, 10 months) 
<br> 
Joined a biotech-startup as a main senior developer. Implemented the whole functionality of 2 apps from scratch for med and vet applications. Actively communicated with designers, managers and hardware developers.
<br>
*Swift, RxSwift, Realm, Core Bluetooth, Protobuf*

- [Yandex](https://apps.apple.com/us/app/yandex-mail-email-app/id441785419): Apr 2018 → Jul 2020 (2 years, 4 months) 
<br> 
Used to develop a client for Yandex mail as a middle developer. In particular improved the accessibility of the app by implementing the dynamic type and dark theme across the app. Worked out on features from scrath as a head of these features: communicated with managers, developers and designers a lot.
<br>
*Objective-C, Swift, Core Data*

- [Rosberry](https://rosberry.com/): Dec 2014 → Apr 2018 (3 years, 5 months) 
<br>
Been involved in many different projects at the outsource company as junior developer: music apps, workout app, booking-analolgue and so on.
    - [Pixelcraft](https://itunes.apple.com/us/app/pixelcraft-color-by-number-art/id1303437993?mt=8): Swift, MVVM+Routing, Firebase, AVFoundation
    - [BEATMix](https://itunes.apple.com/us/app/beatmix/id1227356223): Swift, MVVM, RxSwift, Realm, OpenGL, AVFoundation
    - [Phyzseek](https://itunes.apple.com/us/app/id1076780161?mt=8): Objective-C, MVC, ReactiveCocoa, CoreData
    - [KostHERO](https://itunes.apple.com/ru/app/kosthero/id1140399629?l=en&mt=8): Objective-C, VIPER, ReactiveCocoa

### Open Source Projects

 - [CodableAlamofire](https://github.com/Otbivnoe/CodableAlamofire): An extension for Alamofire that converts JSON data into Decodable objects.
 - [NxEnabled](https://github.com/Otbivnoe/NxEnabled): Library that allows you binding enabled property of button with textable elements ﴾TextView, TextField﴿
 - [XShared](https://github.com/Otbivnoe/XShared): Xcode extension that allows you copying the code with special formatting quotes for social ﴾Slack, Telegram﴿
 - [Framezilla](https://github.com/Otbivnoe/Framezilla): Elegant library that wraps working with frames with a nice chaining syntax.
 - [Framer](https://github.com/Otbivnoe/Framer): Framer is a good framework that wraps working with frames with a nice chaining syntax.

### Articles

{% for post in site.posts | sort: "date" %}
- [{{ post.title }}]({{ post.url }}). {{ post.subtitle }}
{% endfor %}