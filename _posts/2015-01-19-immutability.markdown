---
title: Immutability
slug: immutability
date: 2015-01-19
layout: post
published: true
tags: [blog]
abstract: "Immutable objects make code easier to reason about. They remove entire categories of potential bugs."
image: "http://verraes.net/img/posts/2015-01-19-immutability/2001-a-space-odyssey-large.jpg"
---

Immutable objects make code easier to reason about. They remove entire categories of potential bugs. This is hard to appreciate if you've never worked on a system with lots of immutable objects, so hopefully these examples will shed some light.

{% highlight php %}
<?php
// Mutable
$dateA = $dateB = new DateTime("2014-01-01");
$dateA->add(new DateInterval("P1D"));
echo $dateB->format("Y-m-d"); // 2014-01-02
{% endhighlight %} 

`$dateB` has also changed. Is it intentional or by accident? Only the original developer knows. The bug is blatantly obvious here, but in real programs, it may be a lot subtler.

Immutability makes intentions clear.

{% highlight php %}
<?php
// Immutable
$dateA = $dateB = new DateTimeImmutable("2014-01-01");
$dateA2 = $dateA->add(new DateInterval("P1D"));
echo $dateB->format("Y-m-d"); // 2014-01-01
{% endhighlight %} 

No possible confusion: clearly the developer wanted to add a day to `$dateA` only. 

Note that I assign the new date to a new variable `$dateA2`. In functional languages, variables are bind-once, which means that to understand a block of code, you don't need to walk through all possible paths.

{% highlight php %}
<?php
$a = 1;
$foo = false;
if(bar()) { $foo = true; }
if($foo) { $a = 2; }
echo $a;
{% endhighlight %} 


To know the value of `$a`, you also need to understand the value of $foo. To know `$foo`, you need to understand `bar()`, etc. Part of the difficulty of reading code, is keeping in mind the different paths (hence the metric 

Single assignment can't be enforced in PHP. With some discipline however, it's easy to avoid multiple assignment. If the assignments never change, and the objects assigned to those variables are immutable, then every variable becomes dead easy to understand: you only need to look at the first assignment, and you can be sure it didn't change under your feet.

Having to assign an object to a new variable all the time, may seem a lot of work. In reality, you won’t do it all that often though. If you do, it indicates a block of code is doing too much. Refactor!

{% highlight php %}
<?php
final class Card
{
    private $suit;
    private $rank;
    public function __construct($suit, $rank)
    {
        if($rank > 13) {
            throw new Exception("Cards only go up to 13 (King)");
        }
        $this->suit = $suit;
        $this->rank = $rank;
    }

    public function nextCard()
    {
        return new Card($this->suit, $this->rank + 1);
    }
}
{% endhighlight %} 


With immutable objects, invariants are easier to protect as well. The object's invariants are established during construction. Every operation results in a new object, and as the invariants are checked for every new object, you can guarantee that every operation results in a valid object. The programming language does all of this work for you for free.

Apart from the bug reduction this gives you, it makes testing easier. Of course you still need to test the operations on the object, but you don't have to test whether the operation can render the object's state invalid.

In the above Card example[^1], we forgot to put an upper bound in `nextCard()`. The issue is easily discovered, because it is impossible to instantiate Card with illegal numbers. If we add operations later, they will benefit from the same protection.

The object has “failure atomicity”. If an exception is thrown in an immutable object, it is never left in an invalid intermediate state. The operation either succeeded and resulted in a new object, or the object never sprang into existence. 

## Applicability

Where to use immutable objects? For Value Objects and messages, immutability is definitely desired, and almost always suited. For other objects, in an OOP language, it often still makes sense to allow mutation. A good heuristic is to try to design objects as immutable first, and only open them up if there's a clear need. Another heuristic is to find out if the domain concept your object represents,   

[^1]: In real code, Card would be composed of Suit and Rank, which in turn are immutable value objects.



@TODO:

// most of the time, don't touch req
f(req, next) {
   do stuff
   next(req)
}

// sometimes, pass along a new version of req
f(req, next) {
   do stuff
   next(
        req->withHeader("X-Foo", "my header")
   )
}

This api is not worse than the alternative, not even subjectively. It is an improvement: this expresses exactly what happens. It's like a game of chinese whispers: you either pass along the message you received, or a new modified copy. If halfway down the chain the message is modified, it doesn't have an effect on what the original message was. 




