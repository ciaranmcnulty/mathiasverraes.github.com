---
title: Buzzword-free Bounded Contexts
slug: buzzword-free-bounded-contexts
date: 2014-02-13
layout: post
published: true
tags: [blog]
abstract: "My recent attempt at explaining Bounded Contexts at @DDDBE, was criticised for being too heavy on terminology. Rinse, adjust, repeat."
image: "http://verraes.net/img/posts/2014-02-13-buzzword-free-bounded-contexts/m-and-ms.jpg"
---



Context is everywhere. The human brain interprets context immediately, without effort. Try this:

- A product of my company
- A product of prime factors
- A product of my imagination

Note how easy it was for you to derive context. For machines, this is hard, which is one reason we feed them models with unambiguous names.

The difference is often even a lot subtler.

- A product in the sales department is a thing with a weight and dimensions, a description, a few pictures and some customer reviews.
- For the inventory, it's a box that takes space on a shelf and that you always have too many or too few of.
- For the shipping department, it's a package you need to deliver to a customer. It has a weight and dimensions, but this time they include the packaging.

 In each of these departments, people know the context perfectly well. They don't bother finding a different name, because even though it's all called product, there's never any misunderstanding.

## A Model within a Context

<img style="float:left;margin-right: 10px" src="/img/posts/2014-02-13-buzzword-free-bounded-contexts/m-and-ms-small.jpg" alt="Separate into Bounded Contexts">

And then the software developers come along, and we put all these totally different things that are all called product, in the same database table and model. We're supposed to be the smart guys, but we've been fooled by the fact that they share the same name. We've totally missed the context. We end up with a huge model, that tries to be all these things at once, for every department, despite the fact that these departments are quite independent in real life anyway.

Now imagine that instead of one team, building one application and one model, we have three teams, in three different companies, building three separate systems: Salesify, SaasInventory, and SpeedyShip. It's up to the users to integrate them. Nobody is calling the other teams going "Hey, we should all share one database!". That's a recipe for disaster. They use different infrastructure, different storage, different models. What you would do, is agree on a narrow, strictly defined way to integrate the systems. You could use messaging, an API, import scripts, or dumping files on an FTP.

Whether the three teams will easily reach agreement on those integration contracts, is [a whole different discussion](/2014/01/bandwidth-and-context-mapping/). But apart from that, it's great not to share your database, or any other infrastructure. It gives you freedom. There is no complication from trying to put all the things in one model.

So why do we make it so hard on ourselves when we are just one team? We can borrow the benefits of having multiple teams (clarity, clean separation), without the downside (communication overhead). That's essentially what Bounded Contexts are about:

- Split up your system according to natural divisions in the domain, such as departments, userbases, lifecycles, goals, ...
- Make the contexts explicit. ("A Product in the context of Shipping is ...")
- Make the boundaries explicit. ("Shipping uses database A, Inventory uses database B...")
- Keep the models consistent within a Bounded Context. Well, keeping your models consistent is a good idea anyway.

## Read more

- [Bandwidth and Context Mapping](/2014/01/bandwidth-and-context-mapping/)
- [Context Mapping: Life Expectancy](/2015/04/context-mapping-life-expectancy/)

