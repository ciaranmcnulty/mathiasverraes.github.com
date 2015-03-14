---
title: "Event Storming, Storytelling, Visualisations"
slug: event-storming-storytelling-visualisations
date: 2015-03-14
layout: post
published: true
tags: [blog]
abstract: "About telling non-linear stories with stickies, and visualising complications in Event Stormed models."
image: "http://verraes.net/img/posts/2015-03-14-event-storming-storytelling-visualisations/storytelling.jpg"
---


## Visual storytelling through Event Storming

Last week, my client's Chief Product Owner announced to the team they would introduce a set of new requirements using Event Storming, "as Mathias has taught us". (I've visited there regularly for the past half year or so). The session took about ten minutes, and included no brainstorming or collaboration of any kind. The only thing it had in common with Event Storming, was that the PO used colorful stickies.

I was of course sceptical at first, but it turned out to be quite interesting in it's own right, as a presentation technique. The Chief PO and his team were well prepared. They had researched the regulations they had to comply with to enter a new market, and third party services they'd need to integrate with. They had done Event Storming (or something similar) amongst themselves, cleaned up the resulting artifact, and ordered and collected the stickies. During the short session with the development team, the Chief PO told the story, while the assistants supported it by adding stickies to wall as the story evolved. It was not a linear story. You could easily understand were a new branch in the story began, because the PO's attached new stickies at that location. 

Surely, for the developers, it didn't create the collaborative deep learning experience that Event Storming usually does. They were only spectators. But this style of viaully supported presentation, was definitely much better than some of the alternatives, like purely oral storytelling, or a written set of requirements.  

<img src="/img/posts/2015-03-14-event-storming-storytelling-visualisations/storytelling.jpg" alt="Storytelling">

## Model Storming visualisations

Event Storming as described by Alberto Brandolini is rather prescriptive, and that's a good thing. A fixed set of steps allows you to get started on exploring and modelling from the first minute. There's no long introductions, no discussions on what the right approach is, and no analysis paralysis. 

The first few steps go something like this:

- Write down a Domain Event on a sticky (preferably something that happens at the end, like "Bill was paid"). 
- Ask what happened before that ("Bill was send") and before that, until you get to the beginning. 
- Ask what caused the events to happen. Those are usually Commands (aka user intentions), other events, or simply the passage of time. 
- Ask for what reason a Command can fail, and find the rules that impact the outcome of the Commands. This pushes you to discover alternative scenarios, missing events, connections between distant events...

The list goes on for a bit (I should probably do a proper writeup of how I run Event Storming sessions). The point I want to make here though, is that it's ok to deviate. When a team has some maturity with the domain, more complex scenarios might emerge that are not easily captured with basic Event Storming. At this point, you should invest some energy in coming with new visualisations, new elements that capture the essence of what you're trying to achieve. This brainstorming about the modelling itself, is crucial to evolving deeper insight. 

Describing the experience of reaching a breakthrough, is hard. Good models are always obvious in retrospect. As an example, at this client, we were cutting our teeth on a complex customer accreditation process that varies highly in different markets. To add to the complexity, the process varies depending on whether the customer is using the site, talking to customer support by phone, or visiting an office. And finally, a process that started through one medium, could be finished through another. 

For our first model, we focused too much on the design we thought we wanted: Reusable Specification and Policy objects, Adapters and Strategies, with the intent of injecting the behaviour for the different markets into the process. The next morning, while attempting a PoC implementation, we realized we had painted ourselves into a corner. Event Storming to the rescue. Based on nothing but a gut feeling, I proposed we started with Commands instead of Events though. Then we evolved that in to some sort of matrix, with one swim lane per Market x Medium combo. Finally it hit us that the implementation shouldn't start from a single accreditation process where we injected strategies. Our visual model suggested that we needed multiple processes, implemented independently, and only share elements between those processes if it absolutely made sense. I don't have a good picture of the resulting model, but it wouldn't have made sense anyway. As with all brainstorming, you had to be there.
 
I don't believe we would have come to this conclusion so fast in any other way. As developers, we are so often blinded by the way the requirements are described, and our own propensity to design according to preconceived notions of what a good design must look like. Find the elements that complicate a problem, find a visualisation, put it front and center, and brainstorm from there. Without visualisation, you spend too much energy synchronizing each other's mental models, usually unsuccessfully. 


## Read More

- [Domain Events](/2014/11/domain-events/)
- [Notes on facilitating Event Storming sessions](/2013/08/facilitating-event-storming/)
- [Posts tagged with Event Storming on Alberto Brandolini's blog](http://ziobrando.blogspot.be/search/label/EventStorming)
- [Tim Schraepen’s Event Storming experience report](http://sch3lp.github.io/2014/07/12/event-storming-exercise/)
- [Messaging Flavours](/2015/01/messaging-flavours/)
