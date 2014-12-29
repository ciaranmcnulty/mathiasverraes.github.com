---
title: "How Much Testing is Too Much?"
slug: how-much-testing-is-too-much
date: 2014-12-29
layout: post
published: true
tags: [blog]
abstract: "Run an experiment to learn how much unit tests you actually need."
image: "http://verraes.net/img/posts/2014-12-29-how-much-testing-is-too-much/inception-mirrors.jpg"
---

Figuring out how much unit tests you need to write, can be tricky, especially if you are new to Test-Driven Development. Some teams strive for 100% code coverage. Some open source projects even announce their test coverage on their GitHub profiles -- as if coverage is an indicator of quality. Coverage only measures the lines of code that are executed by the test suite. It doesn't tell you whether the outcome of the execution is actually tested, let alone how valuable that test is. 

Because of that, **code coverage of your entire code base is a pretty lousy metric**. If you use TDD as intended, you don't need to measure it. The coverage will be good, as a direct result of the method. Some people claim they use coverage to enforce their team to write tests. The better way to solve that problem, is with a healthy dose of education, [pre-merge code reviews](/2013/10/pre-merge-code-reviews/), and pair programming. 

Code coverage has its uses though. When measured ad hoc, in small doses, it is **a great tool to validate your assumptions** about how a single unit test executes a complex block of code. For an example of how to use code coverage to your advantage, watch [Extract Til You Drop](/2013/09/extract-till-you-drop/). 

## Over-test

The Standard Consultant's Answer to "How much tests do I need?" is "It depends". That won't do. My preferred approach is to ask ["What is the easiest way to find an answer?"](/2014/03/small-controlled-experiments/). Experiments beat opinions. **When it comes to getting comfortable with TDD, be overzealous.** Be religious about it. Don’t write a single line that isn’t covered by a test first. Do it with the entire team -- no code goes to master without being fully tested. Accidentally wrote a code without a test? Delete the code, write the test, implement the code. 

<img src="/img/posts/2014-12-29-how-much-testing-is-too-much/inception-mirrors-small.jpg" alt="Over-designing">

Timebox this experiment, say a couple of weeks. As with all experiments, we ask what's the worst that could happen. Progress on features could stop entirely, you all learn nothing, and the whole thing was a failure. (Or you have learned that TDD is not for you. Learning happened!)
 
More likely, you'll get some interesting results:
 
 - You'll learn that unit testing isn't so scary (provided you make the units small);
 - You'll make smaller units;
 - You'll learn to appreciate that [TDD helps you to get in the Zone](http://www.jefclaes.be/2014/12/tdd-as-crack-cocaine-of-software.html); 
 - You'll discover that some things simply aren't worth writing tests for (getters and setters, empty constructors, ...);
 - You'll have fewer getters and setters anyway, and more methods that actually do something;
 - YAGNI code.
 
Add a couple more weeks of overzealous testing, and **you'll get a feel for how much testing is just right**. Time to back down and bend the rules a little. Sometimes, you'll want to do some exploratory coding, and add tests afterwards (*spike & stabilize*). Sometimes, you can implement some code in a way that is so obviously correct, that it doesn't need tests. Sometimes, if you pair program, having extra eyes on the code beats extensive testing. And some edge cases are so rare that worrying about them is a waste. 

I don't think you can develop these instincts if you consistently under-test. **When in doubt, test more**, and keep balancing on the thin line between insufficient and too much. 
 
After the experiment, you'll also end up with a bunch of low-value tests. Remove them, especially if they start bothering you or require maintenance. There's **no value in maintaining useless tests**. A little bit of cleanup can make the tests so much more pleasant to work with. "Deleting tests" is the new "deleting code".
  
## Over-design

Coincidentally, I give the same advice when it comes to software design and modelling. There will always be bystanders, telling you that your solution is over-designed, especially if they don't have intimate knowledge of the problem domain or the non-functional requirements. They may be right, but they have no way of knowing that -- an neither do you. [Software design has a slow feedback cycle](/2014/10/software-design-is-just-theory/). Over-design just a little, get a feel for it, evaluate, and back down. That last bit is important: **if you don't refactor when gaining deeper insight, you're simply creating technical debt**. It's at the other end of the spectrum now, where the grass is slightly greener, but an over-designed model is technical debt nonetheless.  
  