---
layout: post
title: "A Flexible Routing Approach in an iOS App"
subtitle: "At Rosberry we've given up on using storyboards, except the Launch Screen, of course, and configure all layout and transition logic in code. In order to understand our reasons - read the Life without Interface Builder written by 's team, I hope you will find this post useful."
date: 2018-02-01 19:47:12 +0300
image: /assets/img/articles/routing.jpeg
---

At Rosberry we've given up on using storyboards, except the Launch Screen, of course, and configure all layout and transition logic in code. In order to understand our reasons - read the Life without Interface Builder written by 's team, I hope you will find this post useful.
In this article, I'm going to introduce a new approach to routing between view controllers. We'll start with a problem and step by step will come to a final decision. Enjoy reading!

## Dive into the problem
Let's figure out the problem using a specific example. For instance, we're going to build a social app with profiles, list of friends, chats and so on. Definitely, we can notice that it's necessary to show a user's profile from many controllers and it would be great to implement this logic once and reuse it. [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself), we remember you! We can't achieve it by using storyboards, you may imagine how it'd look within a storyboard - weeeeb. 😬
At present, we're using the *MVVM+Router* architecture. *ViewModel* says to *Router* that a transition to another module is needed and the router does it. In our case Router just takes all the transition logic in order not to come to the massive view controller (or view model). If it looks unclear to you at the beginning, please don't worry! I'm going to explain this solution in a plain way, so it will be easily adapted to the simple *MVC* as well.

## Solution
<span>1.</span> At first it would possibly look like an obvious solution is adding an extension to ViewController:

```swift
extension UIViewController {
    func openProfile(for user: User) {
        let profileViewController = ProfileViewController(user: user)
        present(profileViewController, animated: true, completion: nil)
    }
}
```

and it works as needed — write once, then reuse. But it’ll become messy when many transitions come. Xcode autocompletion doesn’t work well I know, but sometimes it can show you a lot of unnecessary methods. Even if you don’t want to show a profile from this screen, it’ll be there. So, go further and try to improve this one.


<span>2.</span> Rather than writing an extension to ViewController and have tons of methods in one place, let’s implement every route in a separated Protocol and use the awesome feature of swift — protocol extension.

```swift
protocol ProfileRoute {
    func openProfile(for user: User)
}

extension ProfileRoute where Self: UIViewController {
    func openProfile(for user: User) {
        let profileViewController = ProfileViewController(user: user)
        present(profileViewController, animated: true, completion: nil)
    }
}

final class FriendsViewController: UIViewController, ProfileRoute {}
```

Now, this approach is more flexible — we can extend a view controller with needed routes only (avoid tons of methods), just add a route to the controller’s inheritance. 🎉

<span>3.</span> But of course there’re some improvements:
 - What if we want to have a modal transition to the profile controller from all places except one? (it’s a rare case, but anyway).
 - Or more importantly — If I change the presentation type, I should change the type of a dismiss transition as well (present / dismiss).

	
We don’t have a chance to configure it for now, so it’s time to implement a Transition abstraction with few implementations — *ModalTransition* and *PushTransition*:

```swift
protocol Transition: class {
    weak var viewController: UIViewController? { get set }

    func open(_ viewController: UIViewController)
    func close(_ viewController: UIViewController)
}
```

I reduced a bit the implementation logic of the *ModalTransition* just for simplicity. A full version is available in [Github](https://github.com/Otbivnoe/Routing/blob/master/Routing/Routing/Transitions/ModalTransition.swift).

```swift
class ModalTransition: NSObject {
    var animator: Animator?
    weak var viewController: UIViewController?

    init(animator: Animator? = nil) {
        self.animator = animator
    }
}

extension ModalTransition: Transition {}
extension ModalTransition: UIViewControllerTransitioningDelegate {}
```

and the similar reduced logic of the [PushTransition](https://github.com/Otbivnoe/Routing/blob/master/Routing/Routing/Transitions/PushTransition.swift):

```swift
class PushTransition: NSObject {
    var animator: Animator?
    weak var viewController: UIViewController?

    init(animator: Animator? = nil) {
        self.animator = animator
    }
}

extension PushTransition: Transition {}
extension PushTransition: UINavigationControllerDelegate {}
```

You definitely have noticed the *Animator* object, so it’s a simple protocol for custom transitions:

```swift
protocol Animator: UIViewControllerAnimatedTransitioning {
    var isPresenting: Bool { get set }
}
```

As I said earlier about a massive view controller, let’s add an object that comprises the whole routing logic and is contained as a property in a view controller. We meet *Router* — a base class for all future routers. 🎉

```swift
protocol Closable: class {
    func close()
}

protocol RouterProtocol: class {
    associatedtype V: UIViewController
    weak var viewController: V? { get }
    
    func open(_ viewController: UIViewController, transition: Transition)
}

class Router<U>: RouterProtocol, Closable where U: UIViewController {
    typealias V = U
    
    weak var viewController: V?
    var openTransition: Transition?

    func open(_ viewController: UIViewController, transition: Transition) {
        transition.viewController = self.viewController
        transition.open(viewController)
    }

    func close() {
        guard let openTransition = openTransition else {
            assertionFailure("You should specify an open transition in order to close a module.")
            return
        }
        guard let viewController = viewController else {
            assertionFailure("Nothing to close.")
            return
        }
        openTransition.close(viewController)
    }
}
```

Please, spend a bit time to understand this code. This class contains two methods for opening and closing, a reference to a view controller and an `openTransition` in order to know how to close this module.

Now using this new class let’s update our *ProfileRoute*:

```swift
protocol ProfileRoute {
    var profileTransition: Transition { get }
    func openProfile(for user: User)
}

extension ProfileRoute where Self: RouterProtocol {

    var profileTransition: Transition {
        return ModalTransition()
    }

    func openProfile(for user: User) {
        let router = ProfileRouter()
        let profileViewController = ProfileViewController(router: router)
        router.viewController = profileViewController

        let transition = profileTransition // it's a calculated property so I saved it to the variable in order to have one instance
        router.openTransition = transition
        open(profileViewController, transition: transition)
    }
}
```

You can see that the default transition is modal and in `openProfile` method we generate new module and open it (of course it’s better to use a builder or factory to generate a module). Also, pay attention to a `transition` variable, `profileTransition` is saved to this variable in order to have one instance.

The next step is updating the *Friends* module:

```swift
final class FriendsRouter: Router<FriendsViewController>, FriendsRouter.Routes  {
    typealias Routes = ProfileRoute & /* other routes */ 
}

final class FriendsViewController: UIViewController {

    private let router: FriendsRouter.Routes

    init(router: FriendsRouter.Routes) {
        self.router = router
        super.init(nibName: nil, bundle: nil)
    }
  
    func userButtonPressed() {
        router.openProfile(for: /* some user */)
    }
}
```

We’ve created the *FriendsRouter* and added the needed routes via *typealias*. Here’s where magic happens! We use protocol composition (**&**) to add more routes and protocol’s extension to use a default implementation for routing. 😎

The last step in this story is how easily and nicely a close transition can be achieved. If you recall *ProfileRouter*, there we’ve configured `openTransition` and now we can take advantage of it.

I created a *Profile* module with only one route — close and when a user clicks a close button, we use the same transition type to close this module.

```swift
final class ProfileRouter: Router<ProfileViewController> {
    typealias Routes = Closable
}

final class ProfileViewController: UIViewController {

    private let router: ProfileRouter.Routes

    init(router: ProfileRouter.Routes) {
        self.router = router
        super.init(nibName: nil, bundle: nil)
    }

    func closeButtonPressed() {
        router.close()
    }
}
```

If necessary to change a transition type, just change it in the protocol extension of *ProfileRoute* and this code will continue working without any changes! Awesome, isn’t it?

<hr>

## Conclusion
At the end, I want to say that this Routing approach can be simply adapted to *MVC, VIPER, MVVM* architectures and even if you use *Coordinators*, they can work together. I’m trying my best to improve this solution and will listen to your bits of advice with pleasure!

For those who are interested in this solution, I prepared the [example](https://github.com/Otbivnoe/Routing) with few modules and different transition between them to understand it more deeply. Download and play!