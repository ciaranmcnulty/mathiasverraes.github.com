---
title: "The Repair/Replace Heuristic for Legacy Software"
slug: repair-replace-heuristic-for-legacy-software
date: 2016-04-26
layout: post
published: true
tags: [blog]
abstract: "How understanding the history of systems aids strategic decisions"
image: "http://verraes.net/img/posts/2016-04-27-repair-replace-heuristic-for-legacy-software/david-d-angers.jpg"
---

<img style="float:right;margin-left: 10px" src="/img/posts/2016-04-27-repair-replace-heuristic-for-legacy-software/david-d-angers-small.jpg" alt="Galerie David d'Angers">


Technical Debt is a great metaphor. It shares many analogous properties with financial debt: loans, accrued interest, token payments, bankrupty… There is a key difference however. **We take financial debt with another party.** This party desires to get their money back eventually, and then some. Over centuries, a system was developed to measure debt fairly unambiguously. We called it money, and we're still iterating and experimenting with new designs, like Blockchain. A second system was developed, to create strong incentives for parties to honour debt agreements. We called this system Property Laws. 

Technical Debt has no measure like money, and no ruleset like Property law, and, more importantly, **with Technical Debt there is no other party**. The organisation is **both the creditor and debtor**. This takes away all incentive to control how Technical Debt is taken out and paid back. If you ask your CFO how much financial debt your organisation has, she'll show you spreadsheets, graphs, payment plans, and projections. She makes quarterly reports for the shareholders. Ask your CTO about the Technical Debt in your company, and you'll get a vague "um… a lot", or "lol idk, u can't measure that bro". 



<img style="float:right;margin-left: 10px" src="/img/posts/2016-04-27-repair-replace-heuristic-for-legacy-software/tied_by_its_shoelaces.png" alt="Technical Debt by Ruth Malan">


In "[Managed Technical Debt](/2013/07/managed-technical-debt/)", I propose a cheap, imprecise, but surprisingly effective method for mapping and measuring debt. In short, it involves posting stickies whenever progress is impeded by debt, and keep marking the stickies for every incident. Over time, we build a map that is not based on perceived quality, but on the actual cost of debt. The *Wall of Technical Debt* serves as a decision tool for focusing our repayment efforts.

## Heuristics

So now we have identified our worst Technical Debt, and made estimations on its relative size. What should we do, replace it or repair it?

The Wall of Technical Debt is a **structural model**, but I like to build **temporal models**. Move beyond "*What debt do we have?*" towards "*How did we get here? What happened before? What was it like back then?*". Call it archaeology, or historical thinking if you will. The point is, you learn a lot from temporal models that you don't get from a mere structural model of the here and now.

When evaluating our legacy systems, these questions become "*What was this system like before? Did people like building it? Did users like using it? Did it meet goals, did it balance the trade-offs well? Has the purpose of the system changed, or have the constraints changed?*" Perhaps the system was well-designed, but not designed for scale, or for mobile, or for multi-tenancy. Businesses change, markets change, regulation changes, technology changes. 

<blockquote><p>A complex system that works is invariably found to have evolved from a simple system that worked. A complex system designed from scratch never works and cannot be patched up to make it work. You have to start over, beginning with a working simple system.<small>Gall's Law, from <a href="http://amzn.to/1Qz1Yhg">Systemantics, John Gall, 1975</a></small></p></blockquote>


Based on this, I propose:

**The Repair/Replace Heuristic for Legacy Software:**
 
- **If the system had good design, repair it;**
- **If the system had bad design, replace it.**

If the system used to work well, and it was well-designed, meaning its structure was congruent with the purpose and the trade-offs, there might still be a lot of value in it. There's a lot of **learning encapsulated in its design**, and we don't want to throw out the (middle-aged) baby with the bathwater. We compare the purpose and the constraints the system had back then with the current ones. From that, we might learn which adaptations can give it a second life. With luck, the system was designed with flexibility in mind, and can be changed cheaply.


On the other hand, a system that never worked really well, is likely beyond saving. Replacement is a more attractive alternative.
 
 
That said, it's a **heuristic, not a rule**. There are many other forces that affect the outcome of a Replacement project, and you'll need to understand them to succeed. [Second-System Syndrome](http://amzn.to/1VQSNRW) is real, and this industry has spent insane amounts of money on failed attempts.

Even when you're not trying to decide on **Repair/Replace** — perhaps the decision was already made by others — the *process* of mapping its history will teach you more about the system and and its design. And one deep insight you learn from temporal modelling, is what is captured in *Boulding's Backward Basis*:

<blockquote><p>Things are the way they are because they got that way.<small>
<a href="http://amzn.to/234iIUW">The Secrets of Consulting, Gerald M. Weinberg, 1985</a></small></p></blockquote>

Or, an ounce of prevention is better than a pound of cure.

<small><a href="http://www.ruthmalan.com/Journal/2013/2013JournalMarch.htm#Technical_Debt">Shoelaces cartoon by Ruth Malan</a></small> 


## Read More

- [Managed Technical Debt](/2013/07/managed-technical-debt/)


