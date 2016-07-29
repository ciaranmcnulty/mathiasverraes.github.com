---
title: Domain-Driven Design is Linguistic
slug: domain-driven-design-is-linguistic
date: 2014-01-01
layout: post
published: true
tags: [blog]
abstract: "Just because two behaviours lead to the same outcome, does not mean they are replaceable."
image: "http://verraes.net/img/posts/2014-01-01-domain-driven-design-is-linguistic/hoarding%20small.jpg"
---

<img style="float:left;margin-right: 10px"  src="/img/posts/2014-01-01-domain-driven-design-is-linguistic/hoarding small.jpg" title="Hoarding is an extreme form of loss aversion. http://jamesgulliverhancock.com/projects/hoarding/">

If I give you half a million euros, you will be quite happy. If I give you a million euros, but on the way home, you get robbed and lose half a million, you will be unhappy, even though the end result is the same. Even if you only lose 200k, you will be unhappier than if you only had the half million to begin with. In psychology, this is called [Loss Aversion](http://www.amazon.com/gp/product/B00555X8OA/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=B00555X8OA&linkCode=as2&tag=verraesnet-20&linkId=WRTRBQWNNPU4DUAC). That is, loss has a higher impact on us than gains. We are not purely rational economic beings. The behaviour that leads to a certain state is more important than the actual state.

Businesses don't make decisions based on the money they have in the bank. It's the successes and the failures that lead that profit, that are going to determine what you're going to do next.

This is what, to me, is so appealing in Domain-Driven Design. Software development has traditionally been mostly about mathematics, about technical matters. DDD makes the linguistics the central concern. To express it in a formula:

`f(x) = y and g(x) = y =/=> f = g`

Or: Just because two behaviours lead to the same outcome, does not mean they are replaceable. In Domain-Driven modelling, state is a side effect of behaviour. State is only interesting as far as it has the potential to influence new behaviour. An Aggregate is not just an composition of objects that belong together to represent a certain state. An Aggregate manages behaviours that belong together, and keeps state only where they are relevant to those behaviours. Event Sourcing takes the idea to the its logical conclusion: We forget about persisting state altogether, and instead, we only persist the Domain Events that lead up to that state. We preserve the meaning, instead of the outcome. When we need the state, to make a decision, we replay the events and interpret them. The beauty of this, is that if we change our minds about how to interpret the behaviour, we are not stuck with the state the we decided to persist in the past.

**Update May 11, 2014** I just had to add [this quote from Rinat Abullin](https://twitter.com/abdullin/status/465463982347026432): 

<blockquote>Behavior is not data. To be more precise, data is often an approximation of observed behaviors under fixed circumstances.</blockquote>

## Read more

- [Model Storming Workshop](/2014/01/model-storming-workshop/)
