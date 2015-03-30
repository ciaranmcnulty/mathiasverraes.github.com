---
title: "Named Constructors in PHP"
slug: named-constructors-in-php
date: 2014-06-12
layout: post
published: true
tags: [blog]
abstract: "Don't limit yourself by PHP's single constructor. Use static factory methods."
---

PHP allows only a single constructor per class. That's rather annoying. We'll probably never have proper constructor overloading in PHP, but we can at least enjoy some of the benefits. Let's take a simple Time value object. Which is the best way of instantiating it?

{% highlight php %}
<?php
$time = new Time("11:45");
$time = new Time(11, 45);
{% endhighlight %}

The only correct answer is "it depends". Both are correct from the point of view of the domain. Supporting both is an option:
 
{% highlight php %}
<?php
final class Time
{
    private $hours, $minutes;
    public function __construct($timeOrHours, $minutes = null)
    {
        if(is_string($timeOrHours) && is_null($minutes)) {
            list($this->hours, $this->minutes) = explode($timeOrHours, ':', 2);
        } else {
            $this->hours = $timeOrHours;
            $this->minutes = $minutes;
        }
    }
}
{% endhighlight %}

This is terribly ugly. It makes using the Time class rather confusing. And what happens if we need to add more ways to instantiate Time?

{% highlight php %}
<?php
$minutesSinceMidnight = 705;
$time = new Time($minutesSinceMidnight);
{% endhighlight %}

Or if we want to support numeric strings as well as integers?

{% highlight php %}
<?php
$time = new Time("11", "45");
{% endhighlight %}

(Note: in production code, I would make my Time class a lot more idiot-proof.)

## Refactor to named constructors

Let's add some public static methods to instantiate Time. This will allow us to get rid of the conditionals (which is always a good thing!).
 
{% highlight php %}
<?php
final class Time
{
    private $hours, $minutes;

    public function __construct($hours, $minutes)
    {
        $this->hours = (int) $hours;
        $this->minutes = (int) $minutes;
    }

    public static function fromString($time)
    {
        list($hours, $minutes) = explode($time, ':', 2);
        return new Time($hours, $minutes);
    }

    public static function fromMinutesSinceMidnight($minutesSinceMidnight)
    {
        $hours = floor($minutesSinceMidnight / 60);
        $minutes = $minutesSinceMidnight % 60;
        return new Time($hours, $minutes);
    }
}
{% endhighlight %}

Every method now satisfies the Single Responsibility Principle. The public interface is clear and understandable interface, and the implementations are straightforward. Are we done?

Well, something is bothering me: `__construct($hours, $minutes)` kinda sucks: it exposes the internals of the Time value object, and we can't change the interface because it is public. Imagine that for some reason, we want Time to store the string representation and not the individual values.
 
{% highlight php %}
<?php
final class Time
{
    private $time;

    public function __construct($hours, $minutes)
    {
        $this->time = "$hours:$minutes";
    }
    
    public static function fromString($time)
    {
        list($hours, $minutes) = explode($time, ':', 2);
        return new Time($hours, $minutes);
    }
    // ...
}
{% endhighlight %}

This is ugly: we go through all the trouble of splitting up the string, only to rebuild it in the constructor.

Do we even need a constructor now that we have named constructors? Of course not! They are just an implementation detail, that we want to encapsulate behind meaningful interfaces. So we make it private: 

{% highlight php %}
<?php
final class Time
{
    private $hours, $minutes;

    private function __construct($hours, $minutes)
    {
        $this->hours = (int) $hours;
        $this->minutes = (int) $minutes;
    }

    public static function fromValues($hours, $minutes)
    {
        return new Time($hours, $minutes);
    }
    // ...
}
{% endhighlight %}

Now that the constructor is no longer public, we can choose to refactor all the internals of Time as much as we want. For example, sometimes you'll want every named constructor to assign properties without passing them through a constructor:
 
{% highlight php %}
<?php
final class Time
{
    private $hours, $minutes;

    // We don't remove the empty constructor because it still needs to be private
    private function __construct(){} 

    public static function fromValues($hours, $minutes)
    {
        $time = new Time;
        $time->hours = $hours;
        $time->minutes = $minutes;
        return $time;
    }
    // ...
}
{% endhighlight %}

## Ubiquitous Language

Our code begins to clean up nicely, and our Time class now has some very useful ways of being instantiated. As it happens with better design, other, previously hidden design flaws, start to become visible. Look at the interface for Time:
 
{% highlight php %}
<?php
$time = Time::fromValues($hours, $minutes);
$time = Time::fromString($time);
$time = Time::fromMinutesSinceMidnight($minutesSinceMidnight);
{% endhighlight %}

Notice anything? We're mixing no less than three languages:

- `fromString` is a PHP implementation detail;
- `fromValues` is a sort of generic programming term;
- and `fromMinutesSinceMidnight` is part of the domain language.

Being a language geek and Domain-Driven Design aficionado, I can't let this pass. As Time is part of our domain, my preferred style is to find inspiration in the Ubiquitous Language. 

- `fromString` => `fromTime` 
- `fromValues` => `fromHoursAndMinutes`

(If you worry about the extra characters you'd need to type, get an editor with contextual code completion.)

This focus on the domain, gives you some great options:

{% highlight php %}
<?php
$customer = new Customer($name); 
// We can't "new a customer" or "instantiate a customer" in real life.
// Better:
$customer = Customer::fromRegistration($name);
$customer = Customer::fromImport($name);
{% endhighlight %}

Granted, that's not always better. In the case of Time, I might stick to fromString, because maybe at this level of detail in our code, we want to serve the programmer more than the domain. I might even provide both options. But at least, thanks to named constructors, we now *have* options.  


## Read more

- [When to Use Static Methods](/2014/06/when-to-use-static-methods-in-php/)
- [Accessing private properties from other instances](/2011/03/accessing-private-properties-from-other-instances/)
- [Casting Value Objects to String](/2013/02/casting-value-objects/)
- [Final Classes: Open for Extension, Closed for Inheritance](/2014/05/final-classes-in-php/)




