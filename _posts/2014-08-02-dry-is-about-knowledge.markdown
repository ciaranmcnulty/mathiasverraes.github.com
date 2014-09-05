---
title: DRY is about Knowledge
slug: dry-is-about-knowledge
date: 2014-08-02
layout: post
published: true
tags: [blog]
abstract: "Code duplication is not the issue."
image: "http://verraes.net/img/posts/2014-08-02-dry-is-about-knowledge/find-the-differences.jpg"
---

Have a look at these two classes:

{% highlight php %}
<?php // example 1
final class Basket
{
    private $products;

    public function addProduct($product)
    {
        if (3 == count($this->products)) {
            throw new Exception("Max 3 products allowed");
        }
        $this->products[] = $product;
    }
}

final class Shipment
{
    private $products;

    public function addProduct($product)
    {
        if (3 == count($this->products)) {
            throw new Exception("Max 3 products allowed");
        }
        $this->products[] = $product;
    }
}
{% endhighlight %}


Would you say this is duplicate code? Does it violate the DRY principle (aka "Don't Repeat Yourself")?

If it is, then the solution to get rid of it, could be something like this:

{% highlight php %}
<?php // example 2
abstract class ProductContainer
{
    protected $products;

    public function addProduct($product)
    {
        if (3 == count($this->products)) {
            throw new Exception("Max 3 products allowed");
        }
        $this->products[] = $product;
    } 
}

final class Basket extends ProductContainer {}
final class Shipment extends ProductContainer {}
{% endhighlight %}

The code is identical, but why? As good Domain-Driven Design practitioners, we'd have to check with the business. The product could be a flammable chemical, and for safety concerns, a shipment is not allowed to have more than three of them. As a consequence, we don't allow customers to order more than three at a time. 

In another scenario, the similarity of the code might also be simple coincidence. Maybe supply of the product is limited, and we want to give our customers equal opportunity to buy it.
 
<img style="float:left;margin-right: 10px" src="/img/posts/2014-08-02-dry-is-about-knowledge/find-the-differences-small.jpg" alt="Find the differences">
 
 
## Reasons to Change
 
Whatever the case, it doesn't matter. The problem is that the rules might change independently. The business might realize that they can still sell more than three products at a time, and divide the products over multiple shipments. In example 2, we are now stuck with high coupling of both types of domain objects, to the same business rule, and indirectly to each other. Changing the rule for Basket, changes the rule for Shipment, but potentially has dangerous consequences in the real world. The example is of course blatantly simple to refactor, but legacy code is usually a lot harder.  

You might be tempted to solve the problem by abstracting the limit:

{% highlight php %}
<?php // example 3
abstract class ProductContainer
{
    protected $products;

    public function addProduct($product)
    {
        if ($this->getProductLimit() == count($this->products)) {
            throw new Exception(sprintf("Max %d products allowed", $this->getProductLimit()));
        }
        $this->products[] = $product;
    } 
    
    abstract protected function getProductLimit();
}
{% endhighlight %}

This works to certain extent, but the rules might change in unexpected ways. Example 3 assumes that the limit is always easy to determine with a single method call. New rules might take into account the customer, their history with our company, certain legal conditions, promotions, etc. The new rules can dynamically influence the number of products you can buy, in ways that can't be modelled using our abstract `addProduct()` method.

## Knowledge

The business rule in my example is not "Max 3 products allowed". There are in fact two business rules: "A basket is not allowed to have more than three products" and "A shipment is not allowed to have more than three products". Two rules, no matter how similar, should have two individual representations in the model.

"Don't Repeat Yourself" was never about code. It's about knowledge. It's about cohesion. If two pieces of code represent the exact same knowledge, they will always change together. Having to change them both is risky: you might forget one of them. On the other hand, if two identical pieces of code represent different knowledge, they will change independently. De-duplicating them introduces risk, because changing the knowledge for one object, might accidentally change it for the other object.

Looking at the reasons for change, is a very powerful modelling heuristic.

## Read more

Follow up posts:
- [Why DRY?](http://blog.ploeh.dk/2014/08/07/why-dry/) by Mark Seemann ([@ploeh](https://twitter.com/ploeh))
- [Aim for DRY, but be willing to fall short](http://www.devjoy.com/2014/09/aim-for-dry-but-be-willing-to-fall-short/) by Richard Dalton ([@richardadalton](https://twitter.com/richardadalton)]

Some posts about Domain-Driven Design:
- [Domain-Driven Design is linguistic](http://verraes.net/2014/01/domain-driven-design-is-linguistic/)
- [Why Domain-Driven Design Matters](http://verraes.net/2014/05/why-domain-driven-design-matters/)
