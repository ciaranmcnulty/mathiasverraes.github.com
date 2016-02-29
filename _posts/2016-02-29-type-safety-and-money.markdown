---
title: "Type Safety and Money"
slug: type-safety-and-money
date: 2016-02-29
layout: post
published: true
tags: [blog]
abstract: "All good design is redesign."
image: "http://verraes.net/img/posts/TODO"
---

Below is an attempt at illustrating a design/redesign process I went through at a client, who's started refactoring the core systems their business depends on. Design is the part of software development that is the most messy, the hardest to fit into rules or well-defined processes. In fact, while writing this post, I tweeted:

> ["There are surprisingly few software design books that recommend taking a walk, a shower, or a nap, as an important step."](https://twitter.com/mathiasverraes/status/704250689107709952)

None of the solutions offered below should be taken as truth. I may have already changed my mind on some of them by the time you read them. 

## History

In 2010, I wrote a small library with some Value Objects to represent money, and in 2011, I published a first version. It was a typical open source scratch-your-own-itch thing, that solved a problem with incorrect rounding and conversion in an e-commerce project I was working on. It seems to have scratched many itches, as it got quite popular.

I’ve been a bad parent to the project lately. It's in a [new GitHub organisation](https://github.com/moneyphp/money) now, with @sagikazarmark as the new maintainer, who's blowing new life into it.

Over the years, a bunch of forks and variations appeared. I welcomed this, as many of them serve a need that my library didn’t. It freed me from having to support all possible use cases, and avoided feature bloat. 


## Design Flaws

Apart from being incomplete, the library suffers from fundamental design flaws, some of which can’t be fixed without breaking it.

- It doesn’t support higher precision than the currency’s default precision.
- As a result of this, it always rounds, so if you chain operations, you end up with rounding errors.
- It doesn’t support blockchain currencies, or virtual, vendor-specific currencies, like Facebook Credits (now discontinued), casino chips, game credit…
- Having to enter values in integers was often impractical (eg €1 had to be written as 100.)
- Some currencies have mills (1/1000<sup>th</sup> of a unit) as their official division (although they are becoming rare). Bitcoin supports 10<sup>−3</sup> (a millibitcoin), 10<sup>−6</sup> (a microbitcoin or a bit), and 10<sup>−8</sup> (a satoshi). Reportedly, the community is considering to introduce even smaller divisions. The library doesn't support any of this.

## Requirements

This is what we expect from our model:

1.	We need to support about 10 currencies, possibly more in the future. When we do add new currencies, it is assumed that the in-house developers will add support in the code. There’s no UI for adding new currencies.
2.	All operations need to done with a precision of 8 decimals. This is a business decision.
3.	When showing amounts to users, or pass them along over an API, we always stick to the currency’s official division. In the case of Bitcoin, that does indeed mean we’re keeping a precision of 1 satoshi aka BTC 10−8.
4.	In some markets, our software will need to do specific compliance reporting.
5.	All internal reporting needs to be in EUR, no matter what the original currency was.
6.	We’re using some legacy and third-party systems, that also publish revenues to our internal reporting tool. Most of them only support the currency’s official division, and can’t deal with higher precision.
	
## Assumptions
 
To understand this post, I'm assuming you know:
- what a Value Object is, and why you'd want some;
- how you can make your objects immutable;
- why they _should_ be immutable;
- how you can make operations safer, eg by disallowing $1+€1 without explicit conversion;
- why splitting €0.05 between two parties is not €0.025+€0.025 but €0.03+€0.02.

## Fowler-style

Let's design!
 
Our first instinct is to have the `Money` object from [Fowler's PoEAA](http://amzn.to/1TN7Tq4), consisting of a float and a currency, which in turn could be an object as well. Both are immutable.

<img style="float:right;margin-left: 10px" src="/img/posts/2016-02-29-type-safety-and-money/Money.png" alt="Money">

`Currency` is an enum type supporting our 10 currencies (A simple assertion will do if your language doesn't have enums). We can't initialise the `Currency` object with ay other value than our 10 currencies. It only uses the 3-letter ISO symbols, anything else is an error. That satisfies requirement 1.

<img style="float:right;margin-left: 10px" src="/img/posts/2016-02-29-type-safety-and-money/MoneyFormatter.png" alt="MoneyFormatter">

`Money`’s constructor can round floats with more than 8 decimals. We can add some operations, like `add(Money other) : Money` and `multiply(Money other) : Money`. They also round to 8 decimals if needed. 


Requirement 3 (showing pretty-printed, rounded values to the users), is clearly presentation logic. We should avoid muddying up our domain model for this. So we add a `MoneyFormatter` in the presentation layer, which takes a `Money` argument and returns a string, such as €5.00 or 5,00€, depending on local standards. Perhaps we even make a HTML widget or helper of sorts, so that our templates don’t need to worry about it.



## Itch

Our current design doesn’t satisfy the problem of exposing the rounded values over an API. Of course, we could `round(money.getAmount())`, but `money.round() : Money` seems more in line with our existing methods. 

This is where you should start feeling the itch. Have a close look at the type of this method. `money.round() : Money`

{% highlight java %}
a = new Money(1.987654321, eur)
// The constructor rounds it own to 1.98765432
b = a.round()
// round() rounds it up to 1. 99 and instantiates a new Money
// Money’s constructor rounds it to 1.99000000
{% endhighlight %} 

Technically, most languages don’t distinguish between 1.99 and 1.99000000, but logically, there is an important nuance here. `b` is not just any `Money` , it is a fundamentally different type of money. Our design doesn’t make that distinction, and just mixes up money, whether it was rounded or not.

Let's make the implicit explicit. We can rename `Money` to `PreciseMoney`, and add a new type called `RoundedMoney`. The latter always rounds to whatever the currency's default division is.

The `round()` method now becomes very simple:

{% highlight java %}
PreciseMoney {
	round() : RoundedMoney {
		return new RoundedMoney(this.amount, this.currency)
     }
}
{% endhighlight %} 


The chief benefit is strong guarantees. We can now typehint against `PreciseMoney` in most of our domain model, and typehint against `RoundedMoney` where we explicitly want or need it. 

It's easy to underestimate how valuable this style of granular types can be.

- **It's defensive coding, against a whole category of bugs**. Methods and their callers now have an explicit contract, about what kind of money they are talking about. 
- **It's declarative coding**. Declarative style require a lot less tests. There's no point in testing something that is obviously correct. Code with an explicit contract like this, is obviously correct. This is why proponents of strong static type systems like to talk about *Type Driven Development* as an alternative to *Test Driven Development*. 
- **It communicates to different people working on the code, that we care specifically about the difference.**
- **It introduces a concept from the domain into the Ubiquitous Language and the model**. Before we did that, it would likely go unnoticed that terms like "precision" were in fact part of our language. Co-evolving the language, the models, and the implementation, is central to Domain-Driven Design. 
- This design also **helps to apply the Interface Segregation Principle**.

## Type Juggling
   
We're not building this as a generic library, so we don't need to supply a "complete" API. We can (and we should) only add the methods that we are actually going to use. In our case, `PreciseMoney` has a `round()`, but `RoundedMoney` has no `toPrecise()`. In other words, we can cast `PreciseMoney` to `RoundedMoney`, but we can't cast `RoundedMoney` to `PreciseMoney`. It's a one way operation. 

There's an elegance to that constraint. Once you round something, the precision is lost forever. The lack of `RoundedMoney.toPrecise()` fits our understanding of our domain.

Perhaps we'll eventually need some operations on `RoundedMoney`, (although I'd rather avoid having them). We can choose some smart return types: 

{% highlight java %}
RoundedMoney.add(RoundedMoney other) : RoundedMoney
RoundedMoney.multiply(Float operand) : PreciseMoney
{% endhighlight %} 

Notice the different return types. 

Perhaps your writing some client code that wants to do multiplication, but doesn't care about high precision. Chaining methods would suffice here:

{% highlight java %}
someCash.multiply(0.3333).round()
{% endhighlight %} 

Again, this style is very explicit. The compiler or type checker can protect us when we make mistakes against the types, such as passing `PreciseMoney` when `RoundedMoney` is required.

## Conversions
   
Converting between different currencies depends on today's exchange rates. We'll need to fetch those from some third party web API. Or perhaps, another process is putting that information in a file or database somewhere. We don't want to leak that kind of detail into our model, so we can have an Exchange interface, and have one or more implementations of our choice. The method on that interface that we're interested in, takes a `PreciseMoney` and a target `Currency`, and does the conversion.

{% highlight java %}
interface Exchange {
   convert(PreciseMoney source, Currency target) : PreciseMoney
}
{% endhighlight %} 


The `Exchange` implementations will also need to deal with concerns such as caching today's rates, to avoid unnecessary traffic on each call. 

If fetching + caching + converting sounds like a lot of responsibility for one service, that's because it is (and this one is actually still very clean as compared to most service classes I see in actual code). These kinds of service classes are fundamentally procedural code wrapped in an object. Even though leakage is reduced thanks to the Exchange interface, the rest of our code still needs to depend on that interface. This makes testing harder, as all those tests will need to mock `Exchange`. And finally, (although I agree it's subtle here), all implementations of `Exchange` will need to duplicate the actual conversion, or somehow share code. A heuristic that helps, is to figure out if we can somehow isolate the side-effectful code (fetching, caching) from the domain logic (conversion). 

You guessed it, we need to identify a missing concept. Instead of having the `Exchange` do the conversion, we can make it return a `ConversionRate` instead. This is a Value Object that represents a source `Currency`, a target `Currency`, and a factor (a float). Value Objects attract behaviour, and in this case, it's the `ConversionRate` object that becomes the natural place for doing the actual calculation.

{% highlight java %}
interface Exchange {
   getRate(Currency source, Currency target) :: ConversionRate
}

ConversionRate {
  convert(PreciseMoney source) : PreciseMoney
}
{% endhighlight %}

The `convert` method makes sure that we never accidentally convert USD to EUR using the rate that was actually meant for GBP to BTC. All it needs to do is throw an exception if the arguments have the wrong currency.

The other cool thing here is that we don't need to pass around `Exchange`. Instead, we pass the around the much smaller, simpler, `ConversionRate` objects. They are, once again, more composable. Already the possibilities for reuse become obvious: for example, a transaction log can store a copy of the `ConversionRate` instance that was used for a conversion, so you get accountability.

## Simpler Elements 

An `Exchange` is now something that represents the collection of `ConversionRate` objects, and provides access (and filters) on that collection. Sounds familiar? This is really just the *Repository* pattern! Repositories are not just for Entities or Aggregates, but for all domain objects, including Value Objects. 

(We could rename `Exchange` to `ConversionRateRepository`, which would make the pattern more explicit, but remove a name from our Ubiquitous Language. It's helpful in some contexts. Personally, I'd be more in favour of keeping the name. We can annotate `Exchange` as a @Repository, or have it implement a marker interface called `Repository`.)

We split up our procedural original `Exchange` service into the two simpler patterns, Repository and Value Object. Notice how we have at no point removed any essential complexity, and yet each element, each object, is very simple in its own right. To understand this code, the only pattern knowledge a junior developer would need, is in a few pages of chapters 5 & 6 of [Domain-Driven Design](http://amzn.to/1LrjmZF).

## Currency Type Safety
   
We still have some issues. Remember that some currencies have a division of 1/00, 1/1000, or 1/100000000. `RoundedMoney` needs to support this, in our case for 10 currencies. The constructor is starting to look pretty ugly:

{% highlight java %}
switch(currency) 
    case EUR: this.amount = round(amount, 2)
    case USD: this.amount = round(amount, 2)
    case BTC: this.amount = round(amount, 8)
etc
{% endhighlight %} 

Every time we need to support a new currency, we need to add code here, and possibly in other places in our system. Not a serious issue, but not ideal. A switch statement is often a smell for missing types. (Yes, we could replace this with a map, but it's really the same thing. And a map wouldn't fit if we'd be dealing with more interesting behavioural differences between currencies than rounding.)

What happens if we make `PreciseMoney` and `RoundedMoney` abstracts or interfaces, and factor the variation out into subtypes for each currency? 

<img src="/img/posts/2016-02-29-type-safety-and-money/FactorOutCurrency.png" alt="Factoring out currency">

Each of the `PreciseEUR`, `PreciseBTC`, `RoundedEUR`, `RoundedBTC` etc classes have very local knowledge about how they go about their business, such as the rounding switch example above.

{% highlight java %}
RoundedEUR {
   RoundedEUR (amount) {
       this.amount = round(amount, 2)
   }
}
{% endhighlight %}

Again, we can put the type system to work here. Remember that, per requirement 5, our reporting needs to be in EUR? We can now typehint for that, making it impossible to pass any other currency into our reporting. Similarly, the different compliance reporting strategies we need for requirement 4 can each be limited to the currencies they support.

Sure, we get a bit of class explosion, but lots of small classes are preferred over long methods. They're not only easier to test. Short, declarative code, that is obviously correct, doesn't even need as many tests as long code blocks do. 



