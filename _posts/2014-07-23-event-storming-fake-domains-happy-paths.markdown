---
title: "Event Storming: on Fake Domains and Happy Paths"
slug: event-storming-fake-domains-happy-paths
date: 2014-07-23
layout: post
published: true
tags: [blog]
abstract: "More Event Storming insights: picking good domains, modelling branches using duplication, and creative visualisation."
image: "http://verraes.net/img/posts/2014-07-23-event-storming-fake-domains-happy-paths/event-storming-happy-path.jpg"
---

Inspired by [Tim Schraepen’s experience report](http://sch3lp.github.io/2014/07/12/event-storming-exercise/), I thought I’d list some recent experiences with facilitating Event Storming sessions. You might want to read my [older post with notes on facilitating Event Storming sessions](/2013/08/facilitating-event-storming/), and the [huge amount of content Alberto Brandolini has produced on the topic](http://ziobrando.blogspot.be/search/label/EventStorming).


## Pick a good domain


If you’re doing it as an exercise, as opposed to a real project, pick a real(-ish) domain. 


I’ve been using a government/education project for a long time, because it’s complex, vague, messy, and in constant motion. It turned out to be great domain to teach participants about challenging your own assumptions. For example, report cards that do not show grades, but observations and conclusions, some of which are written by the actual pupils! The discovery phase is quite useful, but as soon as we tried working towards a more concrete model, the rather “fluffy” domain was too hard to explain. You just can’t learn anything useful about this domain in a few hours. 


So the last couple of times, when running Event Storming workshops with Jef Claes, Alberto Brandolini, and Stijn Vannieuwenhuyse, we picked a car rental domain. It’s a lot more strict — for example, a car cannot be in two places, driven by two people. That makes it a lot easier to evolve the model with business rules and aggregates. It’s easy to come up with these kinds of business rules, so the role of fake domain expert is easier. That leaves more focus for actually facilitating the exercise. 


(Last week I started consulting for an actual car rental company. Talk about having my own assumptions challenged!)


If you get the opportunity though, apply Event Storming with real projects, and real domain experts. It will be easier than using fake domains and experts. And you’ll all have skin in the game, which will focus the discussions.

<img style="float:left;margin-right: 10px" src="/img/posts/2014-07-23-event-storming-fake-domains-happy-paths/event-storming-wall.jpg" alt="Event Storming">


## Actors


I only add roles or actors to an event when they are not obvious. But before I do, I try to find if there’s a concept I’m missing. There could be a hint in the Ubiquitous Language. Often it turns out that it shouldn't be modelled as one event, caused by different actors. Instead, there are two similar, but slightly different events. If both the customer and the administrator deal with payments, maybe PaymentWasMade is the customer’s event, and PaymentWasRegistered is the administrator’s event. The different language makes it explicit that it’s not the admin making the payment; they’re only recording that it happened. 


## Duplication

You can discover some kind of cyclic behaviour. An event causes the flow to start over at an earlier point. Or flows branch off into different directions. Or a Process Manager listens to events from different places in the flow. Visualizing this turns into a spaghetti of arrows flying everywhere. And as soon as you draw an arrow, you’ve coupled the position of the sticky to the location of the arrow, making it hard to move them around. 


Our terrible developer brains, confused and screwed up by years of “Don’t Repeat Yourself”, fail to see the obvious solution: it’s ok to duplicate events. Relax, everything is going to be fine. It’s just stickies you're copying, not code. Uncle Bob is not going to haunt your nightmares.


Write down the same event on as many stickies as you need. Place them wherever they occur or influence something else. Use the same name, and perhaps some visual indicator, to make them easily recognisable. As a facilitator, as soon as I notice an event is central to the model and highly reusable, I make a bunch of copies. I put them on the wall or give them to the participants. They can keep focusing on modelling the flows.


## Happy Path

<img style="float:left;margin-right: 10px" src="/img/posts/2014-07-23-event-storming-fake-domains-happy-paths/event-storming-with-branches-small.jpg" alt="Happy Path">

Now that we’ve conquered our irrational fear of writing the same event down twice, there’s lots of cool things we can do. Typically, without duplication, you’d show a branches starting from the same place (first picture). With duplication, you can more clearly visualise different flows. Some of them are Happy Paths, i.e. a flow desired to the business or the user. The others are exceptions, edge cases, race conditions, or any possible outcomes. In the second picture, I mark them with green and red stickies. 


<img style="float:left;margin-right: 10px" src="/img/posts/2014-07-23-event-storming-fake-domains-happy-paths/event-storming-happy-path-small.jpg" alt="Happy Path">

You can experiment with variations in flow. What happens if a customer pays the invoice before we sent it?  What happens when the customer contests it before we sent it? Because it’s so cheap to move stickies around, you trigger discussions with non-technical business people, and gain new insights. It’s domain-driven design in its purest form!


## Visual Language


Model Storming is closer to the idea of brain storming: anything can happen, and there’s no standard notation. When done well, it leads to finding your own visual notation for various concepts. On the other hand, Event Storming uses a rather strict notation and ordered set of steps. The clear rules make it easier for people who are not used to be creative in an everything-is-permitted environment. Still, that shouldn’t stop us from adding some imaginative visuals into the mix. Draw simple icons like smiling or sad customers, lightning, time bombs, various traffic signs, objects from the actual domain,... They speak to the emotional side of the brain, and can convey a lot of information without explanation. You’ll find you will remember the bits with drawings a lot better as well.


## Read More

- [Domain Events](/2014/11/domain-events/)
- [Notes on facilitating Event Storming sessions](/2013/08/facilitating-event-storming/)
- [Event Storming, Storytelling, Visualisations](/2015/03/event-storming-storytelling-visualisations/)
- [Posts tagged with Event Storming on Alberto Brandolini's blog](http://ziobrando.blogspot.be/search/label/EventStorming)
- [Tim Schraepen’s Event Storming experience report](http://sch3lp.github.io/2014/07/12/event-storming-exercise/)
- [Messaging Flavours](/2015/01/messaging-flavours/)

