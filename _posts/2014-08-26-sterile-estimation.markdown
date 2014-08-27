---
title: Sterile Estimation
slug: sterile-estimation
date: 2014-08-26
layout: post
published: true
tags: [blog, estimation-series]
abstract: "The Anchoring Bias, well-known to psychologists, prevents you from making quality estimates."
image: "http://verraes.net/img/posts/2014-08-26-sterile-estimation/clock.jpg"
---

{% include estimation-series.html %}

*To reduce the risk of incidents, the FAA imposed the so-called "sterile cockpit" regulation. Most air traffic accidents happen while taking off or landing, or in the vicinity of airports. The rule instructs pilots and cabin crew to refrain from non-essential activities and conversations while the plane is below 10.000 feet.*

## Anchoring Bias

Let's try an experiment. Do you believe me when I tell you that Mahatma Ghandi died at the ripe old age of 140? Probably not, because that would have been rather spectacular. Just make a guess of what his actual age was, and write it down.

Now go to a colleague (you are reading this at work, aren't you?) and ask them whether Ghandi died at age 9. Then ask them to estimate his age was. My prediction is that their answer will be significantly lower than yours (assuming neither of you knows the real answer). The effect at play here is called the **Anchoring Bias**.

Car salesmen have known about it for a long time of course. They'll start by quoting a high price for a car, more than it's worth. Because they've anchored this number in your mind, any lower price they will quote you next, will seem quite reasonable to you. You are not comparing the number to whatever you're willing to spend, but to whatever they put in your head. (I bet you can see where I'm going with this.)

Tversky and Kahneman (and others), have studied this phenomenon. The book ["Thinking Fast and Slow"](http://www.amazon.com/gp/product/B00555X8OA/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=B00555X8OA&linkCode=as2&tag=verraesnet-20&linkId=WRTRBQWNNPU4DUAC) describes some interesting discoveries.

- The anchoring bias really works, in all contexts, with all kinds of numbers and estimates. When your manager comes in and asks: "Could you finish this new feature in two weeks?", your estimate will be somewhere around two weeks.
- The bias also works when the anchoring number is mentioned out of context. I could say something like "It's almost five o'clock, I need to go pick up my kids." Following that, I ask you how long it will take to fix a certain bug. You're now very likely to answer between 4 and 6.
- It even works when the numbers are not time related: "I had 50 guests at my parties." vs "How many M&M's go in a single pack?"
- And the strangest thing: it works, **even when you first explain anchoring to somebody, and then ask them to make an estimate!**

## Planning Poker

Planning Poker is an estimation method based around the idea of preventing anchoring. The team members first discuss a story, and then choose a card with the number of their estimate. They all turn the cards simultaneously. If the numbers vary, a discussion is needed to resolve the difference. You reach consensus, or defer the estimate and await more information.

However, with an anchoring bias looming over our heads, and knowing it affects even the most hardened psychologists, we may need to introduce some extra care. It's very easy to either accidentally influence the results, or manipulate them on purpose. (In fact, some managers believe that if they can trick you into estimating lower, the work will be done faster. And some customers believe that if they can get a project done cheaper, they will still get the same quality. Ah, the joys of our industry!)

<img style="float:left;margin-right: 10px" src="/img/posts/2014-08-26-sterile-estimation/clock-small.jpg" alt="Sterile Estimation">

## Sterile Estimation

If we must have estimates, whether time or story point based, we need a clean environment. **Sterile Estimation**, like sterile cockpits, prevent unfortunate accidents. Tell all the participants up front of the problem of anchoring bias, and ask them not to mention numbers. When someone does mention a number, consider whether this contaminates the planning session.

After the first round, the consensus estimate is now the new anchor. You can use this to your advantage. Make sure to pick an average story for the first estimation of the session. Even better is to look at some completed stories first. That way, new stories are estimated in relation to older ones. The anchoring bias helps you to keep estimating on the same scale throughout a project.

Of course, creating a truly sterile environment is hard. For me, explaining anchoring bias to clients, is just a tool: it illustrates the idea that estimates are not data, but just guesses. Costly, easily manipulated, and sometimes dangerous, guesses.

{% include estimation-series.html %}
