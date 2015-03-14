---
title: Facilitating Event Storming
slug: facilitating-event-storming
date: 2013-08-28
layout: post
published: true
tags: [blog]
abstract: "Domain-Driven Design modelling for Agile teams, Brandolini-style."
image: "http://verraes.net/img/posts/2013-08-28/event-storming.jpg"
---

Event Storming is a technique where you get the developers and the business stakeholders in a room, and visualize the
business processes. You do this using stickies with domain events and causality (such as commands), and end with drawing
boundaries for aggregates, bounded contexts and subdomains. It's developed by [Alberto Brandolini](http://ziobrando.blogspot.be/) and is deeply rooted in
 Domain Driven Design, and CQRS/ES.

<img style="float:left;margin-right: 10px" src="/img/posts/2013-08-28/event-storming.jpg" alt="Event Storming at Qandidate.com">

I facilitated an Event Storming session with the team of Qandidate.com in Rotterdam. I've read an early draft of Alberto's
paper on Event Storming, that inspired most of what I did. It was my first time doing this as a facilitator, so I'd thought
I'd share my notes. It's just the raw material -- I was going to process them into something more
readable first, but [Jef Claes](http://www.jefclaes.be/) convinced me to just post them as is. So forgive me if they are vague; I promise I will blog about
Event Storming more later.

Despite my rookie mistakes (too much intervention, then too little) the whole thing went very well. The team was very
excited, they want to do it again with their founders. We also did a bit of context mapping for the legacy systems of
their parent company, but time ran out.


## Notes

I think there are three main reasons for Model Storming:

- Discovering complexity early on, finding missing concepts, understanding the business process;
- Modelling or solving a specific problem in detail;
- Learning how to model and think about modelling.

Facilitator tips:

- Hang the first sticky yourself (a tip from Alberto, works really well)
- Know when to step back. Don't do the modelling, guide the modelling
- Ask questions:
   * Is there something missing here? Why is there a gap?
   * How does this make money?
   * How does the business evaluate that this is working? What are the targets, how will we know we've reached them?
   * For whom is this event of importance (end user, business, tenant,…) ?
   * I can't see this particular role, or type of user, in this model. Should they be on here somewhere?
- Change the direction, e.g. start at the end of the flow, move back in time, then later start at the beginning and move forward.
- Interrupt long discussions. Visualise both opinions, and, very important: ask both parties if they feel their opinion is accurately represented.
- Timebox,  using pomodoro's (25 minutes). After each pomodoro, ask what is going well and what isn't. It's a good opportunity to move to the next phase (e.g. from adding events to adding causality, to drawing aggregate boundaries). You may want to move on even if you don't feel the model is complete.
- Constantly move stickies to create room for hotspots.
- Hang red stickies with exclamation marks, question marks, or other notes, anywhere you feel there's an issue.
- At the end, make a photo. Then tell them to throw the model away, and to do it over the next day. If possible in the presence of other stakeholders.
- I personally prefer sentence-style event names ("A product was added to a basket" vs "Product added to basket"). I believe this makes the business people feel more comfortable.

## Read More

- [Event Storming: on Fake Domains and Happy Paths](/2014/07/event-storming-fake-domains-happy-paths/)
- [Event Storming, Storytelling, Visualisations](/2015/03/event-storming-storytelling-visualisations/)
- [Domain-Driven Design is Linguistic](/2014/01/domain-driven-design-is-linguistic/)
- [The DDDBE Modellathon](/2013/09/dddbe-modellathon/)
- [Model Storming Workshop](/2014/01/model-storming-workshop/)

