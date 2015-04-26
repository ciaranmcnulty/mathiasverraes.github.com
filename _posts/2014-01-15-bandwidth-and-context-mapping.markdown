---
title: Bandwidth and Context Mapping
slug: bandwidth-and-context-mapping
date: 2014-01-15
layout: post
published: true
tags: [blog]
abstract: "Context Mapping is about the whitespace between systems, and the bandwidth of the communications."
image: "http://verraes.net/img/posts/2014-01-15-bandwidth-and-context-mapping/context-map-bandwidth.jpg"
---


Context Mapping, first described by Eric Evans in Domain-Driven Design, is lightweight method of drawing out the relations between systems, and parts of systems. It's not technical per se. It exposes the politics of the organisations and the teams building the systems. As you start mapping, every edge that connects two systems, defines an upstream, and a downstream point; the latter system being the one that is affected by changes in the former. Or, to put it more graphically: if the upstream people piss in the river, the downstream people are drinking it. Pardon my French. In the book, Eric defines a number of patterns, that characterise those relations: Anticorruption Layer, Custom/Supplier, Open Host Service, Published Language, Shared Kernel, etc.

Mind that it's not about the direction of the data. You might be sending data to another system, but still be downstream, because they decide what that data should look like. Eric, and [Alberto Brandolini](https://twitter.com/ziobrando/status/423531883893252096), have [explained it better](http://www.infoq.com/articles/ddd-contextmapping) than I could, so I won't go into detail.


In the workshop today, and over dinner, [Alberto Brandolini](https://twitter.com/ziobrando/status/423531883893252096) explained his recent idea about Bandwidth. For me, that ties the whole of Context Mapping together. I will try to do it justice here.

<img src="/img/posts/2014-01-15-bandwidth-and-context-mapping/the-deepest-explanation-of-ddd.png" alt="I think today I gave the deepest explanation ever of Context Mapping ...and I feel good!">

## Whitespace and Bandwidth

Claude Debussy predicted funk music:

<blockquote>Music is the space between the notes.</blockquote>


When you draw a Context Map, you visualize the different Bounded Contexts of the system. This is of course useful information, especially when tackling a legacy system. Invisible to the untrained eye, it is in fact more than a static representation. In the whitespace between the parts, there is implicit information about the future success of the project. Say you are downstream from another team's system. You've labeled the relationship as Conformist on your side. Now someone wants you to make a significant change, and this change touches on this relationship with the upstream system. Will this project succeed?

You can not answer this from just looking at the Context Map. Some missing concept is hiding in the whitespace. The patterns describe the character of the communication, but not how much communication is going on between the two teams. Alberto names this quantity the Bandwidth.

<img src="/img/posts/2014-01-15-bandwidth-and-context-mapping/context-map-bandwidth.jpg" alt="Context Map with Bandwidth">

Bandwidth will affect you a lot. If the upstream team is in a different country, and you only have one monthly call with them, the Bandwidth is low. If they are in the office next door, and you have lunch with them daily, your Bandwidth is going to be higher. Even though in my example, you are mostly Conformist, you still have a chance of altering the relation slightly, perhaps pushing it more in the direction of a Customer/Supplier pattern. The Bandwidth allows more communication, more opportunity for shared understanding, more opportunity for having at least some say in the service they provide. Many things affect this Bandwidth: obvious things like physical availability, frequency of communications, responsiveness... But also things like seniority: people who have been in the organisation a long time, are usually the ones setting the rules. Sometimes it will be determined by which system is more essential to the business, although, in some case, a less important system might still call the shots. If one system is legacy, it will be upstream, simply because it is too hard to change. Or perhaps the upstream team simply has too much ego to want to empathise with other teams.

Making this Bandwidth explicit, by drawing fatter and thinner edges, helps you make more informed decisions. Low Bandwidth means higher risk for projects that depend on communications between Bounded Contexts. With Bandwidth, the Context Map is more complete: it now shows the structure, the character of the relations, and the quantity of the communications.

**Update 2014-01-16**
I missed a small nuance. It's not about the amount of communication going on per se, but rather about the *potential*, or the *capacity*, for communication.

## Read more

- [Buzzword-free Bounded Contexts](/2014/02/buzzword-free-bounded-contexts/)
- [Context Mapping: Life Expectancy](/2015/04/context-mapping-life-expectancy/)