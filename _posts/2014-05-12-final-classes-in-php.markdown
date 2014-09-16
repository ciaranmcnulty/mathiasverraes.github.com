---
title: "Final Classes: Open for Extension, Closed for Inheritance"
slug: final-classes-in-php
date: 2014-05-12
layout: post
published: true
tags: [blog]
abstract: "Composition, strategies, callbacks, plugins, event listeners, ... are all valid ways to extend without inheritance."
image: "http://verraes.net/img/posts/2014-05-12-final-classes-php/explicit-interface.png"
---



I make all my classes final by default. I even configured the templates in my IDE prefix new classes with 'final'. I've often explained my reasoning to people. A blog post is in order!

## Clarity of Intent

A guiding principle here is Clarity of Intent. Making a machine do what we want is not so hard. The reason we need clean code, is not for the compiler. It's to help our fellow developers, third parties, and even ourselves in six months time, understand the purpose and the design of our system. Many design principles help us with that: picking good names for our elements, create low coupling and high cohesion, SOLID principles, ...

<img style="float:left;margin-right: 10px" src="/img/posts/2014-05-12-final-classes-php/explicit-interface-small.png" alt="Explicit escalator interface">

Code is often part of something bigger. This can get really complex, really quickly. The constraints we impose, help us to limit the cognitive load for developers working with our code. We encapsulate the details, and expose only what outside code needs to talk to us.

Another such constraint is the Open/Closed Principle. It states that software entities should be open for extension, but closed for modification.
Closed for modification, in this context, means that when your code exposes some behavior to the outside world, that interface should be stable.  Providing an API is a responsibility: by allowing other code to access features of your system, you need to give certain guarantees of stability or low change frequency. The behaviour should be deterministic. It does not mean your implementation can not change. The changes should not affect outside code.



It's important to understand that "Open for extension", does not mean "Open for inheritance". Composition, strategies, callbacks, plugins, event listeners, ... are all valid ways to extend without inheritance. And usually, they are much preferred to inheritance -- hence the adage "favour composition over inheritance". The latter creates more coupling, that can be hard to get rid of, and that can make understanding the code quite tough.

<img style="float:right;margin-left: 10px" src="/img/posts/2014-05-12-final-classes-php/not_how_to_extend_a_car.jpg" alt="Not how to extend a car">

Extension points are part of your public API. By making a class inheritable, you are saying to the outside world: this class is meant to be extended. In some cases that may be the best option, but it is not the only option. Declaring explicitly what the extension points are, is part of the contract your code has with the rest of the system. Final classes help to enforce this contract.

 The fewer behaviours and extension points you expose, the more freedom you have to change system internals. This is the idea behind encapsulation.


## PHP

Making a class final has some drawbacks. For example, you cannot mock it anymore with PHPUnit. This is a limitation of the infrastructure. One option is to remove the final keyword, but add an @final annotation. We loose the strict enforcement, but we still communicate intent. The class is still conceptually closed down for inheritance.

Similarly, we can mark elements with @internal or @api. @internal indicates to the outside world that, even though you can technically access the element, you're not supposed to use or consume it outside of the boundaries of the module or library. @api means it is part of the public contract and can be trusted not to break easily.

## Conclusion

Still in doubt? Consider this: when a class is closed for inheritance, or a method is private, or an element is @internal, you are free to change it or remove it. This freedom removes a lot of the burden of refactoring. Should a valid reason come up to open the class or method, it will be easy to do so, because nothing depends on it being closed. On the other hand, if you start by making everything open or inheritable, it will be very hard to close it later. Somewhere, perhaps invisible to you, something might depend on it. Closing it, or even making a small change, will break that code.

Finally, I have a hypothesis. If, like some other languages, classes in php were final by default, and needed an "inheritable" keyword to open them up, many people who are now opposed to the final keyword, would have no problem with it at all.

**Update June 24, 2014** From [Programmers Exchange](http://programmers.stackexchange.com/a/92771/29370)

<blockquote>"(...) designing a class that can be meaningfully inherited from takes more than just removing a final specifier; it takes a lot of care." &mdash; Konrad Rudolph </blockquote>


