---
title: "Code Reviews and Blame Culture"
slug: code-reviews-and-blame-culture
date: 2016-04-09
layout: post
published: true
tags: [blog]
abstract: "A common believe is that gated reviews lead to blaming individuals. The opposite can be true."
image: "http://verraes.net/img/posts/2016-04-09-code-reviews-and-blame-culture/treasure.jpg"
---

*Gated pre-merge code reviews are bad. Always refactor on master. Always do pair programming. Don't use branches. Apply Continuous Integration.*

Best practices likes these are great. They encode experiences of many individuals and teams into memorable slogans, and help others to make decisions on how to run their teams processes. Unfortunately, the not-so-great practices also get their share of the slogans. Phrases like *"TDD is dead"* and *"Test-induced design damage"* are just as catchy. 

The human brain likes to rationalise. We like to mix and match the advices that fit our narrative. When for example, our code makes writing unit tests hard, adopting "TDD is dead" as a slogan is much more comfortable than learning how to do safe refactors and introduce a test harness. Combine a lack of automated tests with the advice to refactor on master, and you'll get a recipe for disaster.  

As a consultant, sometimes I get hired to help out in amazing teams that deploy great quality software daily. More often though, the environment is less than ideal. If the idea of bringing in external help was a tough sell to management, I'm rarely there for more than a few days. I could come in, recite all the best practices, and be gone (and I'm afraid I've made that mistake on occasion). More valuable is to look for the small improvements that can have a lasting impact and put the team on the path to improvement. If that's just enough wind in the sails to show that progress is possible, that might be enough to get them to take control of their own situation.

<img style="float:right;margin-left: 10px" src="/img/posts/2016-04-09-code-reviews-and-blame-culture/treasure.jpg" alt="Gradual improvement">

I advocated pre-merge code reviews @TODO LINK on this blog a couple of years ago, and I still stand by it. A common believe is that this kind of gated reviews lead to blame culture. If you work in a high performing team, and do a lot of pair programming (or even mob programming!), then shifting to reviews is indeed a step down, that removes trust. But simply shouting on Twitter that peer reviews are bad, displays a lack of appreciation for the fact that context matters.

Many teams have no tests, no refactoring skills, and a high degree of blame culture, where individuals get blamed for breaking the code. Not coincidentally, managers in these teams tend to believe pair programming will cut the productivity in half. Refactoring is high risk, so nobody does it out of fear of getting blamed. 

In my experience, reviews are often sellable here. And, counter-intuitively,  **pre-merge code reviews can actually reduce blame culture!** You need some simple rules:

1. Open pull requests early;
2. Never merge your own code;
3. Never merge code you don't understand or disagree with;
4. Always review others' code before starting on new tasks;
5. Commenting on others' pull requests is good, adding commits is better.

(That last one is a form of asynchronous pair programming if you wish.)

The interesting effect is that bugs, regressions, bad design, and lack of tests, are now no longer the fault of the person who wrote the code. **It is the team that has failed to catch the issues.** If you fail collectively, you'll be induced to find solutions collectively. To reduce the effort of reviews, perhaps you'll start adding some automated smoke tests. The senior team members might put more effort into teaching the juniors. Maybe two people will spend a Friday afternoon figuring out how to get that test framework up and running.

I believe this is a valuable lesson for all of us: When we advocate a best practice, or take advice from someone else, we need to assume there's always a context. There's a history of gradual improvement, a path taken from where we were to where we are. Best practices show the destination, but we can all put some more effort into putting up signposts along the way.