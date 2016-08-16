---
title: Sensible Interfaces
slug: sensible-interfaces
date: 2013-09-24
layout: post
published: true
tags: [blog]
abstract: "How did we get from 'Program to an interface, not to an implementation' to 'Just slap an interface on there, it's the fashionable thing to do'?"
---


Here are some guidelines to help you live in friendly cohabitation with your fellow rockstar ninja code monkeys. I'll update them
as my ideas about the matter evolve.

## Naming

In my team, suffixing your interface with the word `Interface`, is a sackable offence. Well,
maybe it's not *that* bad, but don't bother trying it. The same goes for prefixing it with `I` -- looking at you Microsoft.
Here's the reasoning.

If you have an interface, then you are suggesting
that multiple implementations of that interface are possible. Usually, when you see something like `TranslatorInterface`, chances
are there's an implementation called `Translator implements TranslatorInterface`. It makes me wonder: what makes the `Translator` so special, that it has
the unique right of being called `Translator`? Every other implementation needs a descriptive name, such as `XmlTranslator` or
`CachedTranslator`, but that one is somehow "the default", as suggested by it's preferential treatment in being named `Translator`
without a description.

Is that bad? I believe it is. It confuses people, as they tend to misunderstand whether they should typehint for `Translator`
or `TranslatorInterface`. So both are being used in the client code. Program sometimes to an interface, sometimes to an implementation?

Just as bad, is that it suggests that the implementation is the real thing, and the interface is a label. It should be the
other way around. Look at it from the point of view of the client code:

{% highlight php %}
<?php
class KlingonDecoder {
   public function __construct(TranslatorInterface $translator)
{% endhighlight %}

This constructor definition is saying: "I need a translator **interface** to operate". But that would be silly. It needs an
 object that is a `Translator`. It does not need an interface. And that object has a certain role, a certain contract, namely that of a `Translator`.
I hope I'm making this clear. The interface `Translator` is the essential concept, the thing that clients use. They don't care
whether `Translator` is a concrete class or an interface, and they don't care how it's implemented. The client wants to be
decoupled from all those details. That's the power of interfaces.

## Default implementations

The burden of having a descriptive name then lies with the implementations. If we rename `TranslatorInterface` to `Translator`, our
 former `Translator` class needs a new name. People tend to solve this problem by calling it `DefaultTranslator`. Again, what makes it so
special to be called `Default`? Don't be lazy, think really hard about what it does, and why that's different from other
possible implementations. You might even discover a thing or two about that class, such as having too many responsibilities.

## Nameable

Another bad habit, is using the `-able` suffix for interface names. I guess I can live with something like `Translatable`,
or maybe `Serializable`. But `Timestampable`? `Jsonable`? Is that the world we want our children to inherit? English [censored],
do you speak it? Try making a sentence, it's so much nicer.

{% highlight php %}
<?php
class Product implements CastsToJson, HasTimestamp
{% endhighlight %}

Say it out loud: "Product casts to json and has a timestamp". It's beautiful, it's -- dare I say it? -- Shakespearian.


## Respect the contract

PHP, having grown organically (to put it politely), is rather permissive when it comes to interfaces. Look at this code:

{% highlight php %}
<?php
interface Animal {
    public function makeNoise();
}
class Dog implements Animal {
    public function makeNoise() {}
    public function fetchStick() {}
}
// elsewhere:
public function myClient(Animal $animal) {
    $animal->fetchStick();
}
{% endhighlight %}

Even though `myClient()` accepts an `Animal`, and should have no knowledge whether the `$animal` is a `Dog`b, PHP allows you to call
 `fetchStick()` if `$animal` has a method by that name. That flexibility can be very useful, in a very limited set of cases.
 For all normal cases: never call a method on an object that is not part of the interface you are typehinting for.

## Interface segregation

If you find that your client code depends on an interface with many methods that your client doesn't care about, your
interface may be too big. The same is true if your classes implementing an interface, have a lot of unused stub methods:

{% highlight php %}
<?php
class Fish implements Animal {
    public function makeNoise() {
        throw new NotImplemented("Fish don't make noise");
    }
}
{% endhighlight %}

This is a good sign that you need to split off the `makeNoise()` method into a separate interface. Perhaps `MakesNoise`, or `Noisy`?

## Roles

Interfaces can be a nice way to share code without the client knowing, by seeing them as roles. Say the product prices
are in a database. You have some logic in `OrderBuilder`, but you don't want `OrderBuilder` to know that the prices are in the
database, because that might change in the future. You could solve this with composition.

{% highlight php %}
<?php
interface ProductRepository {
    /* defines find(), add()... */
}
interface ProductPricer {
   public function priceProduct(Product $product);
}
class DbProductPriceRepository implements ProductRepository {
    /* implements find(), add()... */
}
class DbProductPricer implements ProductPricer {
    public function __construct(ProductRepository $productRepository){ /* ... */ }
    public function priceProduct(Product $product) { /* ... */ }
}
{% endhighlight %}

To save a bit of typing, you can give the `ProductPricer` role to `DbProductPricer`. Sure, it's doing double duty, but the clients
don't know that. In this example, it's probably not a good long term solution when your application grows, but if nothing else,
it's a great timesaver when prototyping.

{% highlight php %}
<?php
class DbProductPriceRepository implements ProductRepository, ProductPricer {
    /* implements find(), add()... */
    public function priceProduct(Product $product) { /* ... */ }
}
{% endhighlight %}

Roles of course work great with entities:

{% highlight php %}
<?php
class Teacher implements User {}
class Pupil implements User {}
class Parent implements User {}
{% endhighlight %}

## One implementation

`ProductPricer` is a great use case for an interface, because it's easy to imagine different business rules applying in
different circumstances: `GermanProductPricer`, `BelgianProductPricer`. There could be different technical implementations:
 `DbProductPricer`, `SoapProductPricer`, or a `CachedProductPricer` that wraps one of the other implementations.

But often, it's less clear. If your business has only One True Way to calculate prices, and One True Datasource to store them.
My rule of thumb would be that if you can imagine that there could be more implementations than just the one, it's good to
have an interface. If you can't imagine different implementations, don't have an interface. An example could be `OrderTotalCalculator`:
There's only one valid way to sum the different prices, so an interface does not make sense.


## Aware

I'm sort of undecided about the `Aware` suffix. I don't think it's a problem per se. But of course `ContainerAware` is evil
and should never have been in Symfony. But that is a Dependency Injection anti-pattern, not an interface naming problem. Thoughts?


Read next: [Interface discovery with PHPUnit’s Mock objects](/2011/03/interface-discovery-with-phpunit-mock-objects/) - March 21, 2011
