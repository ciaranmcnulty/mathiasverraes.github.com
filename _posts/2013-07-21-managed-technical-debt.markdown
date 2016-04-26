---
title: Managed Technical Debt
slug: managed-technical-debt
date: 2013-07-21
layout: post
published: true
tags: [blog]
abstract: "We should make a distinction between Managed and Unmanaged Technical Debt."
image: "http://verraes.net/img/posts/2013-07-21-managed-technical-debt/map-of-technical-debt.jpg"
---

[Alberto Brandolini](https://twitter.com/mathiasverraes/status/326435664319111170) said at IDDD Belgium (quoting from memory):

<blockquote>We've always explained 'technical debt' badly to the business. If you have debt with a bank, you can talk to someone,
negotiate, and agree on a payment plan. But technical debt is like debt with the mob: they come at night, pointing a gun
to your head, and they want their money NOW.</blockquote>

[Greg Young](http://codebetter.com/gregyoung/2013/03/06/startups-and-tdd/) on the other hand, has talked on several occasions about how technical debt is not necessarily bad. Businesses
take loans all the time. They are a powerful tool to make an investment now, and pay for it later. It's a fundamental
aspect of our economy.

### Shortcuts


I get very irritated when a non-technical person tells me to take shortcuts to meet a deadline. Or -- even worse -- when
 a developer-turned-project-manager does so. Technical debt is often what made the project slow in the first place. And as
 a consultant being more and more specialized in "problem projects", I've seen a lot of crappy code that grinds projects
 to a halt.

And yet, I myself tell teams to take shortcuts all the time. But there's a difference. I know the codebase intimately.
I know where it's problematic, and where it's under control. I know what is covered with tests, which parts are hurting us,
and which parts are simply not elegant. And most importantly: when I introduce technical debt, I know how to get rid of
it, and I refactor as soon as I feel it's needed. I add comments in the code suggesting how to change it, and often I
hang a sticky note in the backlog. Whenever new code is affected by earlier debt, I bump the priority of the backlog item.

<img style="float:left;margin-right: 10px" src="/img/posts/2013-07-21-managed-technical-debt/map-of-technical-debt.jpg" alt="Mapped Technical Debt (bottom)">


### Team discipline

When opinions are at odds, like in the quotes above, there's often a problem in the language. There's one term, and two
concepts. In the interest of making
the implicit explicit, I'd like to propose that we make a clear distinction between **Managed Technical Debt**, and **Unmanaged
Technical Debt**. The former is defined as technical debt where most of the following conditions are present:

- The team is disciplined: it applies techniques such as refactoring, pair programming, iterative development, CI, TDD (or other testing schemes) ...
- The codebase is well structured, well tested, and stable.
- When introducing new technical debt, the team estimates the cost/benefit.
- Even though the introduced solution is not optimal, the team understands what the optimal solution would be, and how
the current solution can later be evolved towards that goal.
- Technical debt is documented.
- Technical debt is not introduced on top of existing technical debt.
- Existing technical debt is paid off as soon as there's a clear need.

For Unmanaged Technical Debt on the other hand, most of the opposite conditions are present:

- The team lacks maturity.
- Hacks are built as workarounds for the effects of previous hacks.
- The team is unaware that a newly introduced solution is not optimal,
- or the team is aware, but does not understand what an optimal solution would be or how to achieve it.

### How we manage it

(Update September 12, 2013)

The photo above shows a project's story map, and the bottom part is the map of technical debt, in columns per categories
(blue stickies). The green stickies are prioritized, and the priorities regularly change: whenever we find that something
makes us slower, the stickies move up. Sooner or later they move to the kanban board to be addressed in the sprint. We informally use the "three
strikes and you refactor" rule of thumb, but mostly it's gut feeling, experience, and discussion that help us decide
whether something needs fixing -- for example when we believe that future stories will be affected.

Visualizing the story map and the technical debt, is incidentally a great way to relieve stress: sure, there's a lot of
 work ahead, but it never feels unmanageable.

(Update July 22, 2014)

Some people have started using the technique after seeing my lightning talk about it. Some early pictures:

<img style="float:left" style="width:340px; border: 1px solid gray" src="/img/posts/2013-07-21-managed-technical-debt/technical-debt-ramon.png">
<img style="width:340px; border: 1px solid gray" src="/img/posts/2013-07-21-managed-technical-debt/technical-debt-pascal.png">

Be sure to let me know your experience with if you've tried it as well!

## Read more

- [Beautiful Code](http://verraes.net/2011/04/beautiful-code/)
- [Managed Technical Debt - Revisited](/2014/07/managed-technical-debt-revisited/)
- [The Repair/Replace Heuristic for Legacy Software](/2016/04/repair-replace-heuristic-for-legacy-software/)