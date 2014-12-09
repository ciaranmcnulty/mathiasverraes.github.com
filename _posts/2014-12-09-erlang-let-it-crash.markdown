---
title: Let It Crash
slug: erlang-let-it-crash
date: 2014-12-09
layout: post
published: true
tags: [blog]
abstract: "The Erlang view of the world is that everything is a process and that processes can interact only by exchanging messages."
image: "http://verraes.net/img/posts/2014-12-09-erlang-let-it-crash/armstrong-erlang.jpg"
---

"[Programming Erlang: Software for a Concurrent World, 2nd ed](http://www.amazon.com/gp/product/193778553X/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=193778553X&linkCode=as2&tag=verraesnet-20&linkId=JM5GZYMS2A36UJTF)" by Joe Armstrong, is not just a great introduction to the language, but also offers a glimpse into the "Let It Crash" philosophy that drives it. It's a great read, even if you have no intention of programming in Erlang. I've selected and reordered a number of passages, along with some comments, that I hope will do the book justice.

<img style="float:left;margin-right: 10px" src="/img/posts/2014-12-09-erlang-let-it-crash/armstrong-erlang-small.jpg" alt="Programming Erlang">


<blockquote>The Erlang view of the world is that everything is a process and that processes can interact only by exchanging messages. Having such a view of the world imposes conceptual integrity on our designs, making them easier to understand.</blockquote>


<blockquote>The real world actually has independent things communicating through messages. I’m an ex-physicist — we perceive the world by receiving messages. Packets of light and sound carry information. All that we know about the world is what we’ve learned by receiving messages. We don’t have shared memory. I have my memory, you have yours, and I don’t know what you think about anything. If I want to know what you think about something, then I have to ask you a question and wait for you to reply.</blockquote>

(In recent years, the idea of messages as the first class citizens of a system, has been rediscovered by the CQRS/EventSourcing community, albeit with a focus on domain models. In fact, Event Sourced Aggregates are a way to model processes, not things -- they are the poor man's actor model. Building CQRS/ES systems is what led me to Erlang in the first place.)

<blockquote>Imagine a system with only one sequential process. If this process dies, we might be in deep trouble since no other process can help. For this reason, sequential languages have concentrated on the prevention of failure and an emphasis on defensive programming. In Erlang we have a large number of processes at our disposal, so the failure of any individual process is not so important. We usually write only a small amount of defensive code and instead concentrate on writing corrective code. We take measures to detect the errors and then correct them after they have occurred.</blockquote>

(A typical Erlang program might have hundreds, thousands, or even millions of processes. Letting processes crash is central to Erlang. It's the equivalent of unplugging your router and plugging it back in -- as long as you can get back to a known state, this turns out to be a very good strategy. To make that happen, you build supervision trees. A supervisor will decide how to deal with a crashed process. It will restart the process, or possibly kill some other processes, or crash and let someone else deal with it. Despite the lack of defensive programming in idiomatic Erlang, you spend more time thinking about errors then when building in other languages.) 

<blockquote>This leads to a clean separation of issues. We write code that solves problems and code that fixes problems, but the two are not intertwined.</blockquote>

<blockquote>How can we write programs that run faster on a multicore CPU? It’s all about mutable state and concurrency. Back in the old days (twenty-odd years ago), there were two models of concurrency: Shared state concurrency, and message passing concurrency. The programming world went one way (toward shared state). The Erlang community went the other way.</blockquote>

<blockquote>(...) all languages such as C, Java, C++, and so on, have the notion that there is this stuff called state and that we can change it. This is fine as long as you have only one process doing the changing. If you have multiple processes sharing and modifying the same memory, you have a recipe for disaster — madness lies here.</blockquote>

<blockquote>To protect against the simultaneous modification of shared memory, we use a locking mechanism. Call this a mutex, a synchronized method, or what you will, but it’s still a lock. If programs crash in the critical region (when they hold the lock), disaster results. All the other programs don’t know what to do. If programs corrupt the memory in the shared state, disaster will also happen.</blockquote>

<blockquote>Erlang has no mutable data structures (that’s not quite true, but it’s true enough). No mutable data structures = No locks. No mutable data structures = Easy to parallelize.</blockquote>

(Erlang was originally written for networks of single core computers, back in the eighties. The fact that it turns out to be great for multicore CPU's, is an interesting side-effect.)

<blockquote>Once started, Erlang/OTP applications are expected to run forever (...)</blockquote>

(This is no overstatement. Erlang allows you to upgrade the system while it is running. [But don't take my word for it](https://www.youtube.com/watch?v=96UzSHyp0F8).)


