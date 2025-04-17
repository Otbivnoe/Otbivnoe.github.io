---
title: Resume
permalink: /resume
layout: resume
---

<h1 class="resume-name">Nikita Ermolenko</h1>

<br>

### Summary

<ul>
    <li>
        Highly experienced Senior iOS Developer with 9 years in the field, adept at crafting top-notch native applications.
    </li>
    <li>
        Meticulous attention to detail in creating beautiful and intuitive UI designs. I firmly believe that a well-crafted user interface can greatly enhance the overall user experience.
    </li>
    <li>
        My passion for iOS development extends beyond coding, as I actively contribute to the developer community through my writing in a dedicated Telegram channel. I share practical insights and valuable content about the latest technologies. 
    </li>
    <li>
        Passionate mentor for junior developers, dedicated to fostering their growth and success. I find immense fulfillment in sharing my knowledge and experience with aspiring developers.
    </li>
    <li>
        I have a curiosity and a passion for staying at the forefront of the ever-evolving iOS development landscape. Actively researching and experimenting with emerging trends and best practices.
    </li>
</ul>

<hr>

### Experience

{% 
include experience.html 
company = "June" 
link = "https://junehomes.com/about-us"
position = "Senior iOS Developer"
time = "Aug 2022 → Present"
city = "Full Remote"
description = "
<ul>
    <li>
        Led an app refactoring project from UIKit to SwiftUI as the main developer in a proptech startup, resulting in improved user experience and increased development efficiency.
    </li>
    <li>
        Collaborated closely with the design team to build components in Figma, shaping the user experience by adjusting the user flow and ensuring seamless integration of the new UI framework.
    </li>
</ul>
<p>Funding Information:</p>
<ul>
    <li>
        Series B-funded proptech startup, securing $50 million in investments.
    </li>
    <li>
        Read more about the funding on <a href='https://techcrunch.com/2021/09/22/softbank-and-demi-lovato-back-june-homes-a-proptech-startup-emerging-from-stealth-with-50m-in-funding/'>TechCrunch</a>.
    </li>
</ul>
"
stack = "Swift, Realm, SwiftUI, Combine, Async / Await, Fastlane"
%}

<br>

{% 
include experience.html 
company = "UpTeam" 
link = "https://upteam.com"
position = "Senior iOS SDK Developer"
time = "May 2022 → Jul 2022 (startup faced the financial problems)"
city = "Full Remote"
description = "
<ul>
    <li>
        Played an integral role in the development of the <a href='https://www.getnexar.com'>Nexar Smart Dash Cams</a> within the SDK team.
    </li>
    <li>
        Successfully integrated a neural model into the device, enabling on-device processing and analysis of traffic images captured by the DVR.
    </li>
</ul>
"
stack = "Swift, CoreML, Object Detection, Vision, YOLO"
%}

<br>

{% 
include experience.html 
company = "Igloo" 
link = "https://www.linkedin.com/company/iglootest/about/"
position = "Senior iOS Developer"
time = "Jul 2020 → Apr 2022"
city = "Saint-Petersburg, Russia"
description = "
<ul>
    <li>
        Joined a biotech startup as the main senior developer and developed two applications from the ground up, catering to medical and veterinary applications.
    </li>
    <li>
        Ensured seamless operation and user-friendly experience by implementing the entire functionality of the applications.
    </li>
    <li>
        Actively engaged in cross-functional collaboration with hardware developers, resulting in effective coordination and successful project outcomes.
    </li>
</ul>
"
stack = "Swift, RxSwift, Realm, Core Bluetooth, Protobuf, Fastlane"
%}

<br>

{% 
include experience.html 
company = "Yandex" 
link = "https://yandex.com/company/"
position = "iOS Developer"
time = "Apr 2018 → Jul 2020"
city = "Saint-Petersburg, Russia"
description = "
<ul>
    <li>
        Made significant contributions to the development of Yandex Mail by enhancing the app's accessibility: implemented Dynamic Type and a Dark Theme across its interface.
    </li>
    <li>
        Mentored junior developers, providing guidance and support in their professional growth.
    </li>
    <li>
        Led the development of key features as the head, collaborating closely with managers, developers, and designers.
    </li>
</ul>
"
stack = "Objective-C, Swift, Core Data"
%}

<br>

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

<hr>

### Professional hobbies

- Led a programming [Telegram channel](https://t.me/readaggregator), curating valuable content for a diverse audience.
- Maintained own open-source projects on [Github](https://github.com/otbivnoe), showcasing continuous learning and contribution to the developer community.
- Authored [articles](/) on programming in Apple ecosystems, providing practical insights for fellow developers.
- Actively assisted the community by answering programming questions on [Stack Overflow](https://stackoverflow.com/users/3733734/nikita-ermolenko?tab=profile).

<hr>

### Education 

2011 → 2015 Bachelor's degree (Faculty of Mathematics and Information Technology) in [Omsk State University](https://omsu.ru/about/structure/general/omp/eng/)

<hr>

### Open Source Projects

 - [CodableAlamofire](https://github.com/Otbivnoe/CodableAlamofire): An extension for Alamofire that converts JSON data into Decodable objects.
 - [NxEnabled](https://github.com/Otbivnoe/NxEnabled): Library that allows you binding enabled property of button with textable elements ﴾TextView, TextField﴿
 - [XShared](https://github.com/Otbivnoe/XShared): Xcode extension that allows you copying the code with special formatting quotes for social ﴾Slack, Telegram﴿
 - [Framezilla](https://github.com/Otbivnoe/Framezilla): Elegant library that wraps working with frames with a nice chaining syntax.
 - [Framer](https://github.com/Otbivnoe/Framer): Framer is a good framework that wraps working with frames with a nice chaining syntax.

<hr>

### Articles

{% for post in site.posts | sort: "date" %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}

<hr>

### Contacts 

- <a href="mailto:{{ site.email }}">Email me</a>
- [X](https://x.com/{{ site.x_username }})