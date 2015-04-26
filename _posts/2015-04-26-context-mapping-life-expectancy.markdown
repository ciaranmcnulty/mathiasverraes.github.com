---
title: "Context Mapping: Life Expectancy"
slug: context-mapping-life-expectancy
date: 2015-04-26
layout: post
published: true
tags: [blog]
abstract: "When Context Mapping a complex environment, indicate the Life Expectancy of each Bounded Context, as well as its sponsors and other relevant metadata."
---


I was at a client recently, a large insurance and banking company. The organization is over 200 years old. Some of the software is only slightly younger: they first started automating some 40 years ago. Some of the people I spoke with, have been around almost as long. They have, depending on who you're asking, 600 applications in production. The majority is written in Java and COBOL. For them, dealing with legacy is not just about removing some code, it's about phasing out entire systems. 

We did some Context Mapping, but due to time constraints, it was mostly as an exercise, rather than a serious attempt to make sense of it all. Yet already some interesting ideas arose, so I thought it might be valuable to describe them here. If there's any prior art about these patterns, please let me know.

## Reuse - Buy - Rebuild

Many systems serve a common need, so a lot of other systems depend on them. When making strategic decisions about adding a new system, the company has a mantra of "Reuse - Buy - Build". It means of course that reusing an existing system is preferred to buying a new one, which in turn is preferred to building a system. A Context Map is good place to make these forces explicit, for example through some color code or symbol.  

## Life Expectancy

When reusing an existing system, there is a risk of creating a lot of rework: when we phase out an old system, we need to rewrite the integration to work with the newer system. We started indicating the life expectancy of each Bounded Context on the map. It helped us to spot whether making a new integration with an old system still makes sense. Knowing that in reality, large organizations can be slow, we even went as far as to write down both the "official" life expectancy of a system (as decided by the powers that be), and the "realistic" expectation (often 2-4 times as long). 

## Sponsorship

You apply Context Mapping in part to figure out how the company's communication structures can influence the success or failure of a project. A system is usually sponsored by one specific department, and in some cases by a number of different departments. It makes a lot of sense to bring the sponsors or owners of each system into the picture. Knowing who's paying for what (or not) can make all the difference, when you want to determine whether a project is feasible.

## Tragedy of the Commons

Different departments often come together to solve a common problem. With the best of intentions, they jointly build a system to address that. (This pattern is called a "Partnership" in the [Domain-Driven Design book](http://www.amazon.com/gp/product/0321125215/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0321125215&linkCode=as2&tag=verraesnet-20&linkId=DW5WV5IVDVK7SMFN).) Over time, the effect of the Tragedy of the Commons kicks in. More parties join the Partnership, they all adapt the system to their own needs, but no one invests in keeping the system maintainable. Quality declines. By indicating which systems suffer from this effect, we can make decisions about phasing those systems out, or forming a standalone team to take ownership. The Partnership dissolves and is replaced by a Customer/Supplier Relationship (another pattern from the book). 

## Visualization 

I'm moving away from using whiteboards for Context Mapping. They are generally too small, and quickly become unrefactorable anyway. Find a large wall or glass surface, or move some tables together. Use stickies, agree on a color code, and buy some sticky shapes. [Arrows](http://www.post-it.com/wps/portal/3M/en_US/PostItNA/Home/Products/~/Post-it-Arrow-Flags-Bright-Colors-1-2-in-Wide-100-On-the-Go-Dispenser?N=4327+3294647258&rt=rud) are especially useful of course.

Trying to get everything on a single Context Map is impossible. A Context Map is a model, and, as any model, it's a problem solving tool. We judge a model by its usefulness. Of course, when you're new to the organization, or you've never visualized it, your problem is a lack of insight. When the goal is discovery, I don't mind if we end up with an unreadable model. Throw the map away afterwards, because the new shared understanding is the greatest artifact here. Over time, you might grow Context Maps that are stable. Only then you should treat the map as a document that you keep around.


## Read more

- [Bandwidth and Context Mapping](/2014/01/bandwidth-and-context-mapping/)
- [Buzzword-free Bounded Contexts](/2014/02/buzzword-free-bounded-contexts/)

