---
title: Objects as Contracts for Behaviour
slug: objects-as-contracts-for-behaviour
date: 2014-09-29
layout: post
published: true
tags: [blog]
abstract: "Even in the real world, the potential behaviours of an object are essential to that object."
image: "http://verraes.net/img/posts/2014-09-29-objects-as-contracts-for-behaviour/Action_Design_Over_Time_01_gallery_edit.jpg"
---

<!-- image source http://www.dexigner.com/news/20067 -->

A lot of so-called Object-Oriented Programming[^1] is in fact nothing more than procedural code wrapped in objects. The funny thing is: many people actually defend this style of programming!  This is one of the chief arguments:

“*Objects should just be bags of state and nothing more. An invoice does not pay itself. An appointment does not reschedule itself.*[^2] `invoice.pay(anAmount)` *and* `appointment.reschedule(aDate)` *do not match the real world. Something on the outside drives the action. A service of some kind should do the paying and the rescheduling, and then update the state of those objects:* `invoice.setPaidAmount(anAmount)` *and* `appointment.setDate(aDate)`. *By consequence, not in the objects, but the services should contain the business logic.*”

<img style="float:left;margin-right: 10px" src="/img/posts/2014-09-29-objects-as-contracts-for-behaviour/Action_Design_Over_Time_01_gallery_small.jpg" alt="Anaemic objects">

Of course invoices do not pay themselves, but that’s not what an object model is trying to do. An invoice **exposes the behaviour of being able to be paid**. That ability is in fact essential to what it means to be an invoice. The behaviour is an inherent property of an invoice. If an invoice doesn’t have the ability of being paid, there’s no point in issuing invoices at all. In other words, the contract of an invoice object declares that  its interface includes payment as a feature. Its promise to the outside world is that it allows an outsider to pay for it.

Encapsulation of state and behaviour is the core idea behind objects. Go forth and encapsulate!

## Language

Is the paradigm of Object-Oriented to blame for this widespread misunderstanding, or is it merely a matter of education? Part of the confusion is caused by the language. In languages like English, in an imperative sentence, the verb comes before the object. In OOP languages, it’s the other way around. I’d love to see a programming language that puts method calls before the object, but leaves the method arguments at the end  [^3]: `pay.invoice(anAmount)` and `reschedule.appointment(aDate)`.

## Footnotes

[^1]: The term Object-Oriented Programming is rather overloaded. There’s the style that is used in many popular languages like Java and PHP; and there’s the original Smalltalk-style or Alan Kay-style object-oriented programming, which is a misnomer because it puts the focus on messaging and not on objects. I’m talking about the former.

[^2]: The traditional argument used to involve “a car doesn’t drive itself”. Obviously by now people have realised that a car should indeed drive itself. Technology has failed us for so long, but change is coming.

[^3]: Functional programming does that to some degree, because it’s all functions operating on data: `pay(invoice, anAmount)`. But the `pay` function and the invoice data are not strictly coupled to each other. That opens the possibility for another function to manipulate the data without respecting the business rules that pay() implements. Apart from programmer discipline, I haven’t figured out yet if FP allows to somehow combine the benefits of strict encapsulation, with the fluency of the language.
