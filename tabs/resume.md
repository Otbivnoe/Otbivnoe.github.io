---
title: Resume
permalink: /resume
layout: resume
---

<h1 class="resume-name">Nikita Ermolenko</h1>

As an accomplished iOS Developer since 2014, I am highly skilled in crafting top-notch native applications. Application development is not merely my profession, but also my personal hobby and lifelong passion. With a strong commitment to delivering excellence, I consistently execute tasks efficiently and punctually. I thrive on embracing new challenges and exceeding clients' expectations.


### Education 

2011 → 2015 Bachelor's degree (Faculty of Mathematics and Information Technology) in [Omsk State University](https://omsu.ru/about/structure/general/omp/eng/)


### Professional hobbies

- Led a programming [Telegram channel](https://t.me/readaggregator), curating valuable content for a diverse audience.
- Maintained own open-source projects on [Github](https://github.com/otbivnoe), showcasing continuous learning and contribution to the developer community.
- Authored [articles](/) on programming in Apple ecosystems, providing practical insights for fellow developers.
- Actively assisted the community by answering programming questions on [Stack Overflow](https://stackoverflow.com/users/3733734/nikita-ermolenko?tab=profile).


### Experience

<ul>
{% 
include experience.html 
company = "June" 
link = "https://junehomes.com/about-us"
position = "Senior iOS Developer"
time = "Aug 2022 → Present"
city = "Full Remote"
description = "
Led an app refactoring project from UIKit to SwiftUI as the main developer in a real-estate startup. This initiative resulted in improved user experience and increased development efficiency.
<br>
Contributed as a member of the design team, collaborating closely with colleagues to build components in Figma. Played a key role in shaping the user experience by adjusting the user flow.
"
stack = "Swift, Realm, SwiftUI, Swift Concurrency"
%}

{% 
include experience.html 
company = "UpTeam" 
link = "https://upteam.com"
position = "Senior iOS SDK Developer"
time = "May 2022 → Jul 2022 (startup faced the financial problems)"
city = "Full Remote"
description = "
As a full-time contractor, I played an integral role in the development of the <a href='https://www.getnexar.com'>Nexar Smart Dash Cams</a> within the SDK team. A key accomplishment was integrating a neural model into the device, enabling on-device processing and analysis of traffic images captured by the DVR.
"
stack = "Swift, CoreML, Object Detection, Vision, YOLO"
%}

{% 
include experience.html 
company = "Igloo" 
link = "https://www.linkedin.com/company/iglootest/about/"
position = "Senior iOS Developer"
time = "Jul 2020 → Apr 2022"
city = "Saint-Petersburg, Russia"
description = "
Joined the biotech startup as main senior developer. I played a pivotal role in developing two applications from the ground up, catering to medical and veterinary applications. I was responsible for implementing the entire functionality of these apps, ensuring their seamless operation and user-friendly experience.
<br> 
In addition to my development responsibilities, I actively engaged in cross-functional collaboration with hardware developers. This open communication approach allowed for effective coordination and alignment across different teams, contributing to the overall success of the projects.
"
stack = "Swift, RxSwift, Realm, Core Bluetooth, Protobuf"
%}

{% 
include experience.html 
company = "Yandex" 
link = "https://yandex.com/company/"
position = "iOS Developer"
time = "Apr 2018 → Jul 2020"
city = "Saint-Petersburg, Russia"
description = "
I made significant contributions to the development of a client for Yandex Mail. One of my key achievements was enhancing the app's accessibility by successfully implementing dynamic type and a dark theme across its interface. Serving as the head of these features, I collaborated extensively with managers, developers, and designers to ensure their successful execution.
"
stack = "Objective-C, Swift, Core Data"
%}

{% 
include experience.html 
company = "Rosberry" 
link = "https://rosberry.com/"
position = "iOS Developer"
time = "Dec 2014 → Apr 2018"
city = "Omsk, Russia"
description = "
Been involved in many different projects at the outsource company as junior developer: music apps, workout app, booking-analolgue and so on.
"
stack = "Swift, RxSwift, ReactiveCocoa, Viper, CoreData, Objective-C"
%}
</ul>


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


### Contacts 

- <a href="mailto:{{ site.email }}">Email me</a>
- [Twitter](https://twitter.com/{{ site.twitter_username }})