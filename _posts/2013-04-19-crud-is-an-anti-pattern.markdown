---
title: CRUD is an antipattern
slug: crud-is-an-anti-pattern
date: 2013-04-19
layout: post
published: true
tags: [blog]
abstract: "CRUD doesn't express behaviour. Avoid setters, and use expressive, encapsulated operations instead."
---

CRUD is, as Greg Young calls it, our industry's [Grand Failure](http://herdingcode.com/?p=189). We have deluded our
users, and ourselves, that applications are nothing more than a thin layer around our database. We present the user
with grids and edit forms, and ask them to fill in fields, that we map more or less directly to our relational tables.
We use terms like "data model".

Real processes and businesses are not CRUD though. Real people don't say "I've set the paid amount of the order to '120',
the paid currency to 'EUR', and status to 'paid'". They say "I've paid €120 for the order".

A good domain model is not a data model. A domain model describes behaviour, and the data is an artefact of that. Let's
translate our user story to code.

### CRUD thinking:

{% highlight php %}
<?php
$order->setStatus('paid');
$order->setPaidAmount(120);
$order->setPaidCurrency('EUR');
$order->setCustomer($customer);
{% endhighlight %}

### Step 1: Replace a setter with an expressive method

{% highlight php %}
<?php
$order->pay();
{% endhighlight %}

Note that internally, the Order object still stores the `status` field. That's perfectly fine.

### Step 2: Replace values that belong together with a Value Object:

{% highlight php %}
<?php
$money = new Money(120, new Currency('EUR'));
$order->setPaidMoney($money);
{% endhighlight %}

I've written about Value Objects and Money elsewhere on this blog already.

### Step 3: Encapsulate Operation

{% highlight php %}
<?php
$order->pay($customer, $money);
{% endhighlight %}

It should be clear that the code in step 3 is a lot closer to demonstrating the behaviour and the actual purpose of our application.
We haven't changed anything in the internal representation of our Order object. It still has fields like
`paidAmount`, `paidCurrency`, `customerId`, and `status`, that are mapped to the database. Order no longer exposes it's
internals directly, but instead exposes a public api that is intention-revealing. It matches how users see our application:
as a set of behaviours, instead of a bunch of relational tables.


### Bonus points

Optionally, we can make it even more expressive by stating that it's the Customer who's paying:

{% highlight php %}
<?php
$customer->pay($order, $money);
{% endhighlight %}

The downside of this, is that we are coupling the Customer to the Order. This may or may not be desirable, depending on
context. In any case, it's always worth considering.

## Read more

- [Decoupling the Model from the Framework](/2014/09/decoupling-model-framework/)
- [Decoupling Forms from Entities](/2013/04/decoupling-symfony2-forms-from-entities/)



## Comments



### [Daniel Lo Nigro](http://dan.cx/) - 2013/04/20
Hmm... I don't know if CRUD is the right term here. Your `pay` method is only setting values and saving the entity,
and thus is still a CRUD operation. I think you're complaining about using "raw" getters and setters as opposed to
methods that encapsulate the business or domain logic. There's definite advantages to containing domain logic, but
domain logic and CRUD aren't mutually exclusive.

### [Mathias Verraes](http://twitter.com/mathiasverraes) - 2013/04/20
Care to elaborate why you believe `$order->pay(...)` is still CRUD?

CRUD is the idea that everything in a model can be described using only four verbs: `create`, `read`, `update`, `delete`. In this
post, and in [the next one](http://verraes.net/2013/04/decoupling-symfony2-forms-from-entities/) I show some examples where
I introduce new verbs: `pay`, `hire`, `promote`. Having domain logic in CRUD systems is possible, but it's harder to express.
My point here is mostly that we should build applications that express user intent using a rich language with verbs that users
 actually use. And that is in fact mutually exclusive with CRUD.

### [Daniel Lo Nigro](http://dan.cx/) - 2013/04/20
I think that `$order->pay(...)` might still be CRUD because in the end, it's just doing a single database update (like 
`UPDATE order SET status = 'paid', amount = 120 WHERE order_id = 123`) and no other logic, which is a CRUD 
operation. It's a thin domain model abstraction of the CRUD operation, but it's just a regular update statement with a 
different name, and hence it's still a CRUD operation. I wouldn't consider it CRUD if it had more logic. I could be
totally wrong though!

And in any case, getters and setters aren't CRUD, and calling `$order->setStatus` doesn't automatically imply CRUD.

### [Mathias Verraes](http://twitter.com/mathiasverraes) - 2013/04/20
You're right that setters don't imply CRUD, but they are mostly seen together. The fact that the backend could use an UPDATE
SQL has nothing to do with the problem at hand though. Databases use primitive verbs, because they have no knowledge of the
 meaning of the data.

Whether or not we have additional business logic in our methods, is a different matter. My example is oversimplified, and indeed doesn't have
much logic in the `pay()` method. It would however be a perfect place to put some invariants: What if the paid amount is incorrect?
What if the currency is wrong? What if the order was already paid? This is all logic that is harder to find a place for in a
CRUD system.


### [Tobias Wooldridge](http://tobias.wooldridge.id.au/) - 2013/04/20
One of the most useful things about design patterns and anti-patterns is that, outside of highlighting problems and
solutions, they have a widely understood, unique name.

In this case, my confusion came about because CRUD is an inappropriate term for what you’re describing. The
anti-pattern definition unfortunately takes the word “CRUD” from the database implementation level of design and applies
it to the domain level.

Compared to names such as ‘God object’ and ‘Spaghetti code’, 'CRUD' is ambiguous. While I’m not a fan of
[other names given to this anti-pattern]( http://en.wikipedia.org/wiki/Object_orgy), I don’t feel CRUD is an
improvement, as it fails to clearly identify the problem at hand.

### [Paul Mitchum](http://github.com/paul-m) - 2013/04/20
CRUD is a pattern that encapsulates certain behavior, related to persisting and managing data outside the script.

If you need that pattern, then use it. If you don't, then don't. :-)

Your customer doesn't care whether you implement `$order->setStuff()->persist();` or
`$order->pay(new SemtanticallyCleanObjectName($currency, $amt));` Your work is to abstract these details away
for your customer, in exactly the same way that `setStuff()->persist();` abstracts CRUD away for you.

The real question is what your customer needs, and how you can deliver it to them in an efficient and maintainable way.
CRUD isn't an anti-pattern working against this goal. Viewing it as the only required layer of abstraction is,
however, most likely a design error.

### [Rasmus Schultz](http://twitter.com/mindplaydk) - 2013-04-22

Hi Mathias,

Two things :-)

First, your post explains how to "replace" the code that uses the accessors directly to perform the change - but "replace" is not the right term here, that's not what's happening. You're not "replacing" anything - you're  introducing better encapsulation for an identified type of transaction, and your code is now self-documenting and clearly reflects the fact that the code that uses the accessors is in fact a transcation. Your Order::pay() implementation will still contain the "replaced" code - and possibly other aspects of the transaction, such as logging, statistics, e-mail notifications, etc.

The code that uses the accessors of course should not go directly in a Controller - and I think that was your point? Although you never used the word Controller. But it has got to go somewhere, right? I think we agree on that much, and your post definitely points out something very important. So far so good :-)

Now secondly, you give "bonus points" for implementing the transaction method in the Customer entity instead of the Order entity. But this particular transaction depends on two things equally: an Order and a Transaction - while implementing the method in the Customer entity makes the controller-code read out more like english ("Customer pay order") there is no logical reason why one is better than the other, and the semantics remain essentially the same.

You can find countless examples of this in the real world - for any transaction that involves more than one entity, you will have to make this decision. It gets particularly tricky when you have three or more entities involved in a transaction - which party is going to be responsible? And even worse, when one of those entities are optional in the same type of transaction. Of course, these are all questions you can answer, but your answers and thinking might not be the same as the next guy who has to work with your code.

That's why I prefer to avoid those questions and use a service-oriented approach instead. Introduce a static PaymentService class to act as a mediator for that transaction. Or introduce an encapsulated PaymentTransaction object, since encapsulation was what you were questing for in the first place. This eliminates all of the above questions - the responsible party is now the transaction service or object, not an arbitrary entity. If an entity is optional in that transaction, make it obvious by allowing a required null-argument for that entity in the transaction service or object.

Adhering to this pattern has other advantages too - in particular, it enables composition: more than one type of service or transaction probably sends e-mail notifications or performs logging, so you can encapsulate those requirements in a base-class, and so on. Which furthermore helps with testing, since you can now inject a mock e-mail client or logger during tests.

But perhaps most importantly, it helps with perception - because your transactions are no longer scattered across entities, but encapsulated in services or transaction objects, it's easier for somebody else to gain an overview of all the possible transactions in an application. It's also easier to look at an entity and expect to find only methods that operate on the entity itself - rather than methods that depend on other entities. It scales better in terms of complexity, because each entity and service will have a fixed scope - rather than growing each class to meet new requirements, you introduce more classes, each with an isolated responsibility and fixed scope.

In the case of PHP specifically, it also marginally helps with performance, because you're no longer loading transaction code that doesn't get executed.

Maybe this is just way beyond the scope of your post :-)

And under any circumstances, having done this little write-up, I will probably end up posting that on my own blog. Feel free to post as comments on your blog though, if you wish. Though it's almost longer than your blog-post at this point ;-)

I know, I do go on. Sorry about that.