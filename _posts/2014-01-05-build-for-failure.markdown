---
title: Build for Failure
slug: build-for-failure
date: 2014-01-05
layout: post
published: true
tags: [blog]
abstract: "Admit failure. Build for failure. Live failure."
image: "http://verraes.net/img/posts/2014-01-05-build-for-failure/zeromq-book.jpg"
---

We can't avoid failure. Systems fail all the time. The deep assumption of failure needs to be engraved in everything we do or build.

This insight has been hitting me in the face for the past year, with the BuildStuff conference as the pinnacle. Failure in Systems is inevitable ([Gall](/2013/08/john-gall-systemantics-the-systems-bible/)). Predictions fail, and anything that depends on randomness ([Taleb](http://www.amazon.com/Fooled-Randomness-Hidden-Chance-Markets/dp/0812975219)). We're not rational beings, even if we understand to what extent we are not rational ([Kahneman](http://www.amazon.com/Thinking-Fast-Slow-Daniel-Kahneman-ebook/dp/B005MJFA2W)). It's the philosophy of Erlang, to build for failure. It's the idea behind the Lean Startup: fail fast and fail often, until you start to suck at failing. Instead of aiming for small known potential gains (with unknown potential losses), aim for small known losses (with unknown potential gains) ([Taleb](http://localhost:4000/2013/08/antifragile-nassim-nicholas-taleb/) again). Publish blogs first, fix later ([@ToJans](http://twitter.com/ToJans)). Model it wrong, refactor towards deeper insight ([Evans](http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/)). Visualise divergence of interpretations ([@ziobrando](http://twitter.com/ziobrando)). Cause frequent failures, to force yourself to build resilient systems (Netflix Chaos Monkey). And so on...

I've had a sticky note on my team's whiteboard for the past six months, saying "Find at least three solutions". Problems do not have solutions. All they have, are a bunch of random approximations of solutions, each with a bunch of known and unknown drawbacks. The thing with the number three, is that after you've found three solutions, your collective brains are nicely warmed up, and you find three more, by combining parts from the first three. Challenge the solutions, break them down, and when you pick one, build the most minimal version of it. Evaluate. If it's not what you thought it could be, you still have five more scenario's to try.

As if BuildStuff didn't fry my brain as it was, I was lucky to [spend three hours on a plane](http://hintjens.com/blog:73) with [Pieter Hintjens](http://twitter.com/hintjens), the founder of the ZeoMQ community and the author of the book. Here's a quote:

<img style="float:right;margin-left: 10px" src="/img/posts/2014-01-05-build-for-failure/zeromq-book.jpg" alt="ZeroMQ">

<blockquote>
 We pretend to ourselves and others that we can be (could be) perfect, when in fact we consistently make mistakes. Bugs in code are seen as "bad", rather than "inevitable", so psychologically we want to see fewer of them, not uncover more of them. "He writes perfect code" is a compliment rather than a euphemism for "he never takes risks so his code is as boring and heavily used as cold spaghetti".
 <br>
 <br>
Some cultures teach us to aspire to perfection and punish mistakes in education and work, which makes this attitude worse. To accept that we're fallible, and then to learn how to turn that into profit rather than shame is one of the hardest intellectual exercises in any profession. We leverage our fallibilities by working with others and by challenging our own work sooner, not later.
 <br>
 <br>
(...)
 <br>
 <br>
The faster you can prove code incorrect, the faster and more accurately you can fix it. Believing that code works and proving that it behaves as expected is less science, more magical thinking. It's far better to be able to say, "libzmq has five hundred assertions and despite all my efforts, not one of them fails".

</blockquote>


## Read more

- [“Software design is just theory”](/2014/10/software-design-is-just-theory/)
- [The Systems Bible](/2013/08/john-gall-systemantics-the-systems-bible/)