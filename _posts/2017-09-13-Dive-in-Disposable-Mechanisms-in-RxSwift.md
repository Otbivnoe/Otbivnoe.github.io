---
layout: post
title: "Dive in Disposable Mechanisms in RxSwift"
subtitle: "Hey everyone! As a Reactive Lover, I’m going to continue a series of articles dedicated to RxSwift. In my first reactive post, I reasoned about some improvements of flatMap function (you can check it here) and today I want to extend my knowledge of RxSwift by exploring all Rx Disposables."
date: 2017-09-13 11:47:12 +0300
image: /assets/img/articles/disposable.png
---

Hey everyone! As a *Reactive Lover*, I’m going to continue a series of articles dedicated to RxSwift. In my first reactive post, I reasoned about some improvements of `flatMap` function (you can check it [here](https://medium.com/@otbivnoe/improvements-of-flatmap-function-in-rxswift-5d70add0fc88)) and today I want to extend my knowledge of RxSwift by exploring all **Rx Disposables**.

<hr>

This article will explore all Disposables that are available in Rx with short explanations and usage examples.

So, let’s start! 🚀

### 1. Disposable.Empty

*This can be useful whenever you need to fulfil an interface requirement and don’t have any resource management that needs to take place.*

Under the hood it uses the private class — *NopDisposable* (Nop = No Operation)

```swift
static public func create() -> Disposable {
     return NopDisposable.noOp
}

let disposable = Disposables.create() // useful when disposable mechanism is not necessary
disposable.dispose()
```

### 2. Disposable.Create (action)

*The second static method provides a disposable with action that is called when the Dispose method is invoked. It’s handy to use this method with server requests — you can control the lifetime of request with dispose block.*

In this cases we can see another private class — *AnonymousDisposable*. If somebody says something about AnonymousDisposable, don’t be afraid — it’s just a Disposable with some dispose action :)

```swift
public static func create(with dispose: @escaping () -> ()) -> Cancelable {
    return AnonymousDisposable(disposeAction: dispose)
}

let observable = Observable<Void>.create { observer -> Disposable in
    // run request
    observer.onCompleted()
    return Disposables.create {
        // request.cancel()
        print("disposed ☠️")
    }
}

let disposable = observable.subscribe()
```

⚠️ I want to note that in the previous one, create.empty function returns the `Disposable` rather than `Cancelable` object like in this `AnonymousDisposable`. The differences between them are just in the ability to check the disposed state — `isDisposed`.

```swift
/// Respresents a disposable resource.
public protocol Disposable {
    func dispose()
}

/// Represents disposable resource with state tracking.
public protocol Cancelable: Disposable {
    var isDisposed: Bool { get }
}
```

### 3. BooleanDisposable

*This class simply has the Dispose method and a read-only property `isDisposed` that is false when the class is created, and is set to true when the Dispose method is invoked.*

```swift
let disposable = BooleanDisposable()
print("isDisposed = \(disposable.isDisposed)")

print("dispose")
disposable.dispose()

print("isDisposed = \(disposable.isDisposed)")

// log:
// isDisposed = false
// dispose
// isDisposed = true
```

### 4. CompositeDisposable

*Represents a group of disposable resources that are disposed together. Calling Dispose on the CompositeDisposable will call dispose on each of these resources in the order they were provided.*

```swift
let disposable1 = Disposables.create { print("disposed-1 ☠️") }
let disposable2 = Disposables.create { print("disposed-2 ☠️") }
let disposable3 = Disposables.create { print("disposed-3 ☠️") }

let disposable = CompositeDisposable(disposables: [disposable1, disposable2, disposable3])

print("dispose")
disposable.dispose()

// log:
// dispose
// disposed-1 ☠️
// disposed-2 ☠️
// disposed-3 ☠️
```

There’s one more way of creating a group of disposables using the private `BinaryDisposable` class, but not directly, via the static function of Disposables:

```swift
public static func create(_ disposable1: Disposable, _ disposable2: Disposable) -> Cancelable {
    return BinaryDisposable(disposable1, disposable2)
}
```

Additionally, the `CompositeDisposable` class allows you to add and remove resources. After the `CompositeDisposable` has been disposed of, any further resources that are added, will be disposed immediately.

```swift
let disposable1 = Disposables.create { print("disposed-1 ☠️") }
let disposable2 = Disposables.create { print("disposed-2 ☠️") }
let disposable3 = Disposables.create { print("disposed-3 ☠️") }

let disposable = CompositeDisposable(disposable1, disposable2)

print("dispose")
disposable.dispose()

print("insert disposable3")
disposable.insert(disposable3) // It'll be disposed immediately, because we called `dispose` in the parent disposable above.
// log:
// dispose
// disposed-1 ☠️
// disposed-2 ☠️
// insert disposable3
// disposed-3 ☠️
```

Any item that is removed from `CompositeDisposable` is also disposed of, regardless of whether the main disposable itself has been disposed of.

```swift

let disposable1 = Disposables.create { print("disposed-1 ☠️") }
let disposable2 = Disposables.create { print("disposed-2 ☠️") }
let disposable3 = Disposables.create { print("disposed-3 ☠️") }

let disposable = CompositeDisposable(disposable1, disposable2)

print("insert disposable3")
let key3 = disposable.insert(disposable3)

if let key = key3 {
    print("remove disposable3")
    disposable.remove(for: key)
}

print("dispose")
disposable.dispose()

// log:
// insert disposable3
// remove disposable3
// disposed-3 ☠️
// dispose
// disposed-1 ☠️
// disposed-2 ☠️
```

### 5. RefCountDisposable

*Represents a disposable resource that only disposes its underlying disposable resource when all dependent disposable objects have been disposed.*

```swift
let disposable1 = Disposables.create { print("disposed-1 ☠️") }

let refCountDisposable = RefCountDisposable(disposable: disposable1)

let holdDisposable1 = refCountDisposable.retain()
let holdDisposable2 = refCountDisposable.retain()

print("main dispose")
refCountDisposable.dispose()

print("hold1 dispose")
holdDisposable1.dispose()

print("hold1 dispose")
holdDisposable2.dispose()

// log:
// main dispose
// hold1 dispose
// hold2 dispose
// disposed-1 ☠️
```

### 6. ScheduledDisposable

*Represents a disposable resource whose disposal invocation will be scheduled on the specified scheduler.*

```swift
let queue = DispatchQueue.global()
let scheduler = ConcurrentDispatchQueueScheduler(queue: queue)

let observable = Observable<Void>.create { observer in
    observer.onCompleted()
    return ScheduledDisposable(scheduler: scheduler, disposable: Disposables.create {
        print("thread - \(Thread.current)")
    })
}

observable.subscribe()

// log:
// thread - <NSThread: 0x7f9631d62e60>{number = 3, name = (null)}
```

the thread you see is not a main here.

### 7. SerialDisposable

*Represents a disposable resource whose underlying disposable resource can be replaced by another disposable resource, causing automatic disposal of the previous underlying disposable resource.*

```swift
let disposable1 = Disposables.create { print("disposed-1 ☠️") }
let disposable2 = Disposables.create { print("disposed-2 ☠️") }

let serialDisposable = SerialDisposable()

print("set disposable1")
serialDisposable.disposable = disposable1

print("set disposable2")
serialDisposable.disposable = disposable2

print("dispose")
serialDisposable.dispose()

// log:
// set disposable1
// set disposable2
// disposed-1 ☠️
// dispose
// disposed-2 ☠️
```

### 8. SingleAssignmentDisposable

*Represents a disposable resource which only allows a single assignment of its underlying disposable resource.*
<br>
*If an underlying disposable resource has already been set, future attempts to set the underlying disposable resource will throw an exception.*

```swift

let disposable1 = Disposables.create { print("disposed-1 ☠️") }
let disposable2 = Disposables.create { print("disposed-2 ☠️") }

let singleAssignmentDisposable = SingleAssignmentDisposable()

print("set disposable1")
singleAssignmentDisposable.setDisposable(disposable1)

print("set disposable2")
singleAssignmentDisposable.setDisposable(disposable2) // ⚠️⚠️⚠️ fatal error 
print("dispose")
singleAssignmentDisposable.dispose()

// log:
// set disposable1
// set disposable2
// ⚠️⚠️⚠️ fatal error
```

## Conclusion

Finally I want to say that there’re so many different disposables, but it’s hard to find a really good usage for them IMO 😭. Please, leave a comment if you use some of the following disposable approaches and it helps you a lot.
<br>
If you want to find out more about these disposables, check my [playground](https://github.com/Otbivnoe/Rx).

<hr>

Thanks for reading! I hope you’ve enjoyed my article!