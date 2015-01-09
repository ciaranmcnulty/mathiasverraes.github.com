---
title: The DDDBE Modellathon
slug: dddbe-modellathon
date: 2013-09-05
layout: post
published: true
tags: [talk]
image: "https://speakerd.s3.amazonaws.com/presentations/105cfa00f86b0130e851227600bb7d26/slide_0.jpg?1378394644"
---


### Goal

Many teams are not spending enough time on modeling. Often, they're not particularly good at it, and often, the business
considers time spent at the whiteboard "unproductive". The goal of the Modellathon workshop, was to give people a safe
environment to play around with different modeling techniques, without the pressure of producing something of immediate
value. For more experienced attendees, it's an opportunity to exercise their modeling skills on a new and unfamiliar domain.

By collaborating with people, and by learning from the models of the other teams, the attendees get a glimpse of the myriad
 of techniques that exist (often not documented anywhere) to explore, grasp, and visualize, their understanding of the domain.

### Slides

<script async class="speakerdeck-embed" data-id="105cfa00f86b0130e851227600bb7d26" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>
[See the slides on Speakerdeck](https://speakerdeck.com/mathiasverraes/dddbe-modellathon-2013)

### What we did

<img style="float:right;margin-left: 10px" src="/img/posts/2013-09-05-modellathon/mathias_verraes_at_dddbe.jpg" alt="Mathias at DDDBE">

We formed teams of four people. Each team was provided with plenty of [paper](http://www.ikea.com/be/nl/catalog/products/20152281/),
markers, and stickies. Stijn and me played the role of domain experts. We gave a short introduction about "The United Schools of Kazachstan".
This domain was obviously made up, but most of what we talked about, comes from a real project we are working on. We gave the people a
paper report card for [a Kazach pupil](https://twitter.com/DDD_Borat), and told them we wanted a better system for making the reports. Along the way we introduced
more complexity: the different ways pupils can be evaluated, the laws and governmental regulations, the importance of the progress a pupil
 makes, and the confidentiality of the pupils.

In two rounds of 25 minutes, the teams got to work to try and draw the problem space into a model. Meanwhile, Stijn and I walked around to answer questions.
Just like real domain experts, we sometimes took things for granted, or casually mentioned some new piece of critical information.

For the third round, we shifted the focus from understanding the domain, to modeling a somewhat workable solution. Yves and me
now tried to give guidance on techniques, and give the teams some pointers. For example, I showed one team how to get started with
[Event Storming](http://verraes.net/2013/08/facilitating-event-storming/). Another team had already produced a very detailed
and complete model, so I told them that if this were a real application, the teachers would have to spend too much time entering
data, and too little time actually teaching. This forced the team to re-evaluate some of their assumptions.

<img style="float:left;margin-right: 10px" src="/img/posts/2013-09-05-modellathon/presenting_a_model_at_dddbe.jpg" alt="Presenting a model at DDDBE">

After each round, a couple of teams presented their progress. Some had UML-like drawings, others had mostly stickies with words, some had
drawn UI mockups as a starting point. It was quite wonderful to see how distinct each team's model was, despite the short time frame.
Even after the first 25 minutes, it was surprising to see how much the teams had already captured of the problem space.

At the end, we had an improvised round table discussion, about the challenges involved with modeling complex domains, where many people
contributed their insights.


### What I've learned as a facilitator


#### Ask the right question

In attempt to distribute people of different levels evenly across the teams, I asked who was an experienced modeler. No reaction.
 (Belgians especially can be shy in this regard.)
<del>Somebody (I think it was Jef)</del> Tom then asked "who had previously screwed up a model". Some people raised their hands, so that
proved a much more effective way to find the attendees with modeling experience!

#### Struggling

Somebody told me aftwerwards that he would have preferred to first get an overview of modeling techniques. On the other hand,
struggling with a problem is a really good way to learn. It triggers your instinct to look for better ways. And, when somebody
then shows you a new technique, you immediately recognize why it's better. Perhaps, a good approach would be to have the teams
try it for themselves during one or two rounds, and then have a short presentation on one or two techniques they can try.
You'd probably need more time, 3-4 hours at least.

<img style="float:right;margin-left: 10px" src="/img/posts/2013-09-05-modellathon/tom_janssens_at_dddbe.jpg" alt="Presenting a model at DDDBE">

#### Be very attentive

As a facilitator, it's easy to get carried away with explaining the domain. It's important to be aware of teams that have trouble
 moving forward. It's better to step out of the domain expert role, and help them move forward. Perhaps a good approach would be to
 have different people in the expert and in the facilitator role (which is more or less what we did when Yves starting helping
 out in the third round.

#### Throw away the models

We gave teams the choice of working on the same model round after round. In retrospect, it might have been better to explicitly
ask them to throw everything away. This way, you give yourself the opportunity to start over with a different technique, borrowed
from another team's presentation. Some teams became quite attached to their models, which limited their progress. [Kill your darlings!](http://c2.com/cgi/wiki?PlanToThrowOneAway)

#### Feedback, Feedback, Feedback!

Using the Pomodoro's and having a few minutes of feedback proved extremely useful. I tended to provide the options, and have
the attendees choose, but soon I noticed that they came up with better ideas than me. So as a reminder to myself: the group
is smarter than the individual!

### Conclusion

It was a miracle that we had so much attendees for our brand new [Belgian Domain-Driven Design community](http://domaindriven.be),
and that all these people were prepared to run this experiment with us. In fact, I don't think anything like a modellathon
was ever attempted anywhere else. Let me know if you do.

In any case, I can highly recommend running a modellathon of your own. And, incidentally, I'd love to come facilitate it :-)

### More

- Belgian Domain-Driven Design community [domaindriven.be](http://domaindriven.be)
- [@DDDBE](http://twitter.com/DDDBE) on Twitter
- Read next: [Facilitating Event Storming](/2013/08/facilitating-event-storming/)
