---
title: Small Controlled Experiments
slug: small-controlled-experiments.markdown
date: 2014-03-22
layout: post
published: true
tags: [blog]
abstract: "How we made continuous improvement truly continuous, using stickies, a timeline, and few minutes each day."
image: "http://verraes.net/img/posts/2014-03-22-small-controlled-experiments/chemistry-show.jpg"
---


You're in a meeting. Maybe it's a daily stand-up meeting, or an agile retrospective. Somebody has an idea. A heated debate follows. Everybody is stating opinions, trying to convince the others, explaining why their idea is going to work, and the alternative is stupid. Because everybody is focused on making their point, nobody is actually listening to the others. Eventually the decision is either postponed, or the person with the highest salary makes the final decision, or the one who pushes the hardest. The decision is final, and it may or may not be actually get implemented. And if it is, it is never properly evaluated, or adapted.

I humbly admit to being guilty of having done all of these. And it's all such a terrible waste.

The cure is to set up **small controlled experiments**.

<img style="float:left;margin-right: 10px" src="/img/posts/2014-03-22-small-controlled-experiments/chemistry-show.jpg" alt="experiment">


## Continuous Improvement

I used to think that simply trying to do a good job, was sufficient to incrementally improve the way you do things in your organisation, your process, your culture. Having worked closely with [@VincentVanderh](http://twitter.com/VincentVanderh) for a year, I've learned to appreciate that, even when you think you're doing just fine and you work with the greatest people, a relentless focus on making small improvements every day, is going to pay off massively.

The basic principle is to identify problems, find the simplest thing you could try to improve it, try it for a short time, and evaluate. After that, you either implement the change permanently, dismiss it, or try a new cycle. The idea is of course not new. Deming calls it the *PDCA-cycle (Plan-Do-Check-Act)*. The Japanese, spearheaded by Toyota, have made *Kaizen*, meaning "change for the best", a central part of business culture.

Our first attempts were clumsy at best. We had improved a lot since the start of the project, so we felt we didn't need a formal process. We didn't identify major problems, let alone major solutions. We were ready to dismiss the whole idea of PDCA as just another management buzzword.

We were partly right. The process was too formal for our context, and the last thing you want to do is burden knowledge workers in a tight schedule with more processes to worry about. More importantly, we were trying too hard: we focused on finding major, critical problems, and grand, all-encompassing solutions. And that only works in the movies. We did keep trying however. We came up with experiments, wrote them on a sticky, put them in a corner of the whiteboard, and started doing them. Well, sometimes. More often than not, the stickies hung there for a while, until someone would feel they were pointless, and they were thrown out. Some ideas did get implemented -- after all we had been improving somewhat steadily before we had a name for it -- but it was all rather vague and unpredictable.

## Make it lightweight

Some changes had to be made. The biweekly retrospectives made the improvement process too slow. By this time, we had pretty much gotten rid of Scrum(ish) sprints, in favour of a more continuous Kanban-flow. Saving up all the improvement ideas and discussion for the retrospective, started feeling like a remnant from the biweekly sprint cadence, something that did no longer fit in our flow. So we needed the benefits of evaluating our process, without the slowness of retrospectives.

We changed the rules of the experiments:

- You can write down any idea for an experiment on a sticky and put it in the drop zone of the Kanban board.

- It will be discussed at the stand-up meeting, the following morning.

- The discussion is kept as short as possible. You're not asking if it will work -- the point is that the experiment will prove if the idea works. You're answering the questions: "Will we try this? For how long? Can we do it in a cheaper, lower effort way, that doesn't affect our velocity?"

- The sticky is posted on a timeline, usually about 10-14 days in the future.

- Every day during stand-up, we check whichever stickies are on today's slot on the timeline.

- The experiment is evaluated, we discuss positive or negative side-effects, and decide on making the change permanent or trying more experiments.

A number of things turned out to be critical here:

1. It works best if the experiment takes as little effort as possible. Most of them do not take any time at all, as they only made a small change to something we were already doing. Others take a minute a day, or even a  minute a week. The **"small"** in "small controlled experiment".

2. Sometimes one person is assigned to an experiment, but usually it's **for the whole team**.

3. When starting an experiment, make sure everybody understands that the experiment is **limited in time**, and that they **will be heard** and allowed veto when evaluating it. This is especially important with more **introvert team members**, especially if you have one or two loudmouths like yours truly.

4. Make sure the experiments are **for everybody**. I tended to focus on improving conditions for the developers, but of course, testers, analyst, the project manager, ... should all benefit.

5. Deciding, and **visualising exactly when the evaluation will happen**, is probably the best thing we did. It gives a sense of urgency and deadline to the experiment, preventing it from slowly fading into oblivion. That's the **"controlled"** in "small controlled experiment".

6. We'd usually have a couple of experiments running **simultaneously** at any given time.

Here's a sample of some of the things we experimented with, to give you some ideas. Some happened before we tried PDCA or Kaizen, before we settled on the small controlled experiments above, but making it a core part of our standup has greatly increased the rate of improvement.

## Hacking our productivity

- When a story was estimated as a big one, say an 8-pointer, more time would be spent on it to refactor it. When it was a 3-pointer, fewer time was spent. I'm all for [refactoring](/2013/09/extract-till-you-drop/), but not everything requires the [same level of perfection](/2013/07/managed-technical-debt/). We wanted the quality of a story to depend on value, not on the original estimate. In the experiment, we moved the estimated number from the front of the sticky, to the back. Because of this, the developers could no longer see the estimation, so they were no longer influenced by it. We made the experiment permanent.

- We noticed how at the end of a two-week sprint, the pace would speed up to reach the sprint target, but as soon as it was reached, the tempo slowed down. The fact that the last day was a Friday, didn't help either. Kanban came to rescue here: We stopped setting sprint targets, set strict work-in-progress limits, and made sure we had a pull-based system. (More on Kanban in a future post.)

- The testers work for multiple teams. As our delivery of stories was very irregular, they couldn't predict when they would need time to test our stories. This caused stories to be blocked in testing, creating too much of a gap between development and testing. We experimented with the commitment to deliver exactly one story a day. This worked great for predictability for the testers, but the unwanted side-effects were that stories were being prioritized to reach this goal, along with other subtle ways to cheat the system. The experiment was stopped.

## Technical Debt

- The [Wall of Technical Debt](http://verraes.net/2013/07/managed-technical-debt/) started as an experiment. Cost: a bunch of stickies, and a few minutes. Gain: Having a complete map of everything in the system that needs improving, how to do it, and how important it is.

- To reduce technical debt, we've tried different approaches. We're now on a mix of:
    - having a limit of one technical debt ticket in progress at any time,
    - yet being allowed to pull a technical ticket, if we judge it would be really beneficial to the functional story that we're working on.

## Visualisation

- The Kanban board is regularly redrawn as we get more insight in our process. I think we are in the seventh iteration by now. I think it is beautiful :-) Many of the changes may seem minimal at first, but the accumulated effect is that this board is pure bliss to work with. One example is that the "expedite" swimming lane moved from an afterthought at the bottom of the board, to a prominent place at the top.

<img style="float:left;margin-right: 10px" src="/img/posts/2014-03-22-small-controlled-experiments/kanbanboard-small.jpg" alt="Kanban board">

- We fooled around with the idea from Core Protocols to make our emotions explicit during standup: "I'm glad-mad-sad-afraid that ...". We had no idea what we expected of this experiment, but we felt it was worth trying (it is, once again, dead cheap to try for few weeks). Current status: undecided. The PM will research it better and we'll try it again.

- Hanging up a monitor to show build statuses, sending mails when builds fail, ... nothing extraordinary there, as more and more teams do CI, but you need to make an effort to get it set up.


- We've experimented with different ways of visualising our metrics. It turns out that the best graphs are the ones that are extremely simple, and show you a key metric in a glance, without requiring you to understand how to interpret it first.


- The timeline that we use to visualise the small controlled experiments, was itself a small controlled experiment. It's expanded into showing all important events, like people being on holiday, deadlines, ...

## Process

- We made it dead easy for testers to deploy a pull request to testing environments, so they can do it whenever it suits them, without dependencies on the developers.

- We have a process for testing a pull request in isolation, then merging it, and having a different person testing it as part of the whole application. We have lowest defect rate I've ever experienced on a project.

- We spent time improving the test fixtures, so testers can have a good, realistic starting point, that is exactly the same for every test run.

- It happens more and more often that we work remotely. We've had digital boards before, but they never really worked as well as the physical board. We're now trying Trello. Undecided.

- We moved from occasionally scheduled releases, to weekly releases, using feature branches. Deploying to production has gradually evolved into a non-event.

- (Added March 29 2014) We start every story with pair programming. No matter how hard or easy you think it will be. After inital work has begun, you can decide to continue pairing, split the work, or finish it alone. The benefit here is of course that potential pitfalls are found earlier. There's less need for code reviews. The pairing partner can take over the story later if needed. Even without full-blown pair programming, starting a story in pair is one of those little magic tricks that create huge gains in productiveness and collaboration.

## Analysis

- Stories evolved from vague, widely scoped features, to very specific, atomic descriptions of the smallest idea that provides business value. Because they are small, unnecessary ones can easily be identified and dropped. They can be tested and agreed upon in isolation. Different stories can be worked on in parallel. The predictability of the flow is a lot better. They come with a list of acceptance criteria that we copy into the Github pull request, so we can check off the ones we've implemented.


## Exposing invisible problems

I could go on for a bit, but I hope I made the point clear. According to the Toyota Way, any process, no matter how good you think it is, has waste. The waste is usually not obvious, because if it was, it would have been improved already. Many experiments are not focused on solving a known problem, but on trying things to expose unknown problems. Make sure they are small, low cost, low risk. The benefit may be anywhere between zero and enormous. You won't know in advance, so opinions have little value. You are the scientist of your own culture.


## Optionality

As an aside:

Humans favour courses of action that have a well-known, limited set of potential gains, with an unknown, unlimited potential cost. We ignore this cost because it is invisible. In **"Thinking, Fast and Slow"**, Daniel Kahneman refers a lot to the brain's habit of ignoring the unseen, and calls this "What you see is all there is".

The opposite is a course of action that has a well-known, limited potential cost, with an unknown, unlimited potential gain. In "[Antifragile](/2013/08/antifragile-nassim-nicholas-taleb/)", borrowing from the finance world, Nicholas N. Taleb calls this "optionality". Keep this in mind: many many very small experiments with a very limited cost, will eventually produce totally unexpected outcomes.

## Read More

[My talk on Small Controlled Experiments](/2014/10/small-controlled-experiments-revisited/)