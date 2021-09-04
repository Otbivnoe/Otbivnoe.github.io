---
layout: post
title: "Type Eraser for Rx Subjects"
subtitle: "Let's continue the series of quick articles! Recently, I've had a wish to erase a type of my Subject outside of a class definition. To be clear what I mean I prepared a small class that observes the battery level of my bluetooth device and emits new values on time."
date: 2021-09-04 20:00:00 +0300
badge: Quick tip
image: /assets/img/articles/eraser-subject/logo.png
---

Let's continue the series of quick articles!

Recently, I've had a wish to erase a type of my Subject outside of a class definition. To be clear what I mean I prepared a small class that observes the battery level of my bluetooth device and emits new values on time.

```swift
class BatteryObserver {

    let batterySubject = ReplaySubject<Int>.create(bufferSize: 1)

    func loadBatteryLevelEveryNSeconds() {
        // battery level is 55 now - emit this value
        batterySubject.onNext(55)
    }
}
```

I use `ReplaySubject` in my example, and it'd be great to erase this type (convert to `Observable`) for the outside definition. I don't want to bother that someone else could send a battery level by mistake, who knows... Only this class is responsible for emitting new values.

Roger! One more property with the erased type for help:

```swift
class BatteryObserver {

    var batteryObservable: Observable<Int> {
        return batterySubject.asObservable()
    }

    private let batterySubject = ReplaySubject<Int>.create(bufferSize: 1)
}
```

Looks solid, does it? I've always done this before, but this time I decided to over-engineering a bit - I really tired of this approach by creating two properties every time.


Taking advantage of the relatively new feature - property wrappers, I incapsulated the subject type only for in-class use.

```swift
@propertyWrapper
struct SubjectEraser<Subject>: ObserverType where Subject: SubjectType {

    typealias Element = Subject.Observer.Element

    var wrappedValue: Observable<Subject.Element> {
        return subject.asObservable()
    }

    private let subject: Subject

    init(subject: Subject) {
        self.subject = subject
    }

    func on(_ event: Event<Element>) {
        subject.asObserver().on(event)
    }
}

class BatteryObserver {

    @SubjectEraser(subject: ReplaySubject<Int>.create(bufferSize: 1))
    var batteryObservable

    func loadBatteryLevelEveryNSeconds() {
        // ...
        // battery level is 55 now - emit this value
        _batteryObservable.onNext(55)
    }
}
```

With this solution we have a `wrappedValue` for outside use - the type is erased and using underscore for getting a property wrapper itself we send values for handling further.