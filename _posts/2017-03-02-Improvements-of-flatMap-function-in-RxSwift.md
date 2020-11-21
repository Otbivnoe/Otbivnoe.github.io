---
layout:   post
title:     "Improvements of flatMap function in RxSwift"
subtitle:  "Hey! If you read it, you probably good in RxSwift and ARC, understand differences between weak and unowned keywords. If no, please get acquainted with it and come back!"
date:      "2017-03-02 15:47:12 +0300"
---

Hey! If you read it, you probably good in **RxSwift** and **ARC**, understand differences between `weak` and `unowned` keywords. If no, please get acquainted with it and come back!

In my first post I want to tell you about my `flatMap` improvements. I start immediately with problem…

For example we have two requests: Login and LoadPosts. After login I want to get posts for current user:

```swift
login(username: "Username", password: "Password")
    .flatMap { [weak self] user -> Observable<[Post]> in
        return self.loadPosts(for: user)
    }
    .subscribe()
```

So, here compiler will swear us, that we have to use `!` and `?` for `self`. Of course, you may say: “Why don’t we use `unowned` keyword?” Yes, we can, but I think it’s wrong to use it in that case.

*The unowned or weak discussion boils down to a question of lifetime of the variable and the closure that references it. So, there is a [big article](https://www.uraimo.com/2016/10/27/unowned-or-weak-lifetime-and-performance/) about choosing between unowned and weak*

**Weak**, I Choose You! Now you probably want to change previously code to this one:

```swift
login(username: "Username", password: "Password")
    .flatMap { [weak self] user -> Observable<[Post]> in
        guard let `self` = self else {
            return Observable.empty()
        }
        return self.loadPosts(for: user)
    }
    .subscribe()
```

but when you have many `flatMap` functions it looks so messy — we don’t want to see messy code! Only elegant, beautiful code!

It’s time for an awesome extension:

```swift
extension ObservableType {
    
    func flatMap<A: AnyObject, O: ObservableType>(weak obj: A, selector: @escaping (A, Self.E) throws -> O) -> Observable<O.E> {
        return flatMap { [weak obj] value -> Observable<O.E> in
            try obj.map { try selector($0, value).asObservable() } ?? .empty()
        }
    }
}
```

and now our requests look so:

```swift
login(username: "Username", password: "Password")
    .flatMap(weak: self) { obj, user -> Observable<[Post]> in
        return obj.loadPosts(for: user)
    }
    .subscribe()
```

There is a full snippet for all flatMap functions:

```swift
extension ObservableType {
    
    func flatMap<A: AnyObject, O: ObservableType>(weak obj: A, selector: @escaping (A, Self.E) throws -> O) -> Observable<O.E> {
        return flatMap { [weak obj] value -> Observable<O.E> in
            try obj.map { try selector($0, value).asObservable() } ?? .empty()
        }
    }
    
    func flatMapFirst<A: AnyObject, O: ObservableType>(weak obj: A, selector: @escaping (A, Self.E) throws -> O) -> Observable<O.E> {
        return flatMapFirst { [weak obj] value -> Observable<O.E> in
            try obj.map { try selector($0, value).asObservable() } ?? .empty()
        }
    }
    
    func flatMapWithIndex<A: AnyObject, O: ObservableType>(weak obj: A, selector: @escaping (A, Self.E, Int) throws -> O) -> Observable<O.E> {
        return flatMapWithIndex { [weak obj] value, index -> Observable<O.E> in
            try obj.map { try selector($0, value, index).asObservable() } ?? .empty()
        }
    }
    
    func flatMapLatest<A: AnyObject, O: ObservableType>(weak obj: A, selector: @escaping (A, Self.E) throws -> O) -> Observable<O.E> {
        return flatMapLatest { [weak obj] value -> Observable<O.E> in
            try obj.map { try selector($0, value).asObservable() } ?? .empty()
        }
    }
}
```

<hr>

Hope you enjoyed this article! Feel free to [contact me](/tabs/contact.html) if you have any question, feedback or best practices!