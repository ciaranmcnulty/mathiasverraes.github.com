---
title: Decoupling (Symfony2) Forms from Entities
slug: decoupling-symfony2-forms-from-entities
date: 2013-04-20
layout: post
published: true
tags: [blog]
abstract: "Avoid tight coupling between your forms and your Entities by using the Command pattern as an intermediate."
---

I usually tell people to stay away from the Symfony2 Form component, or forms libraries in general. The problem is that
all these libraries are designed in a very CRUD mindset. You've done all the work to keep your models, views, and controllers
nicely separated, and then this component comes along and violates every boundary.

The good news is that there is a solution, that allows you to cleanly separate your model from UI.


### Another example of Encapsulating Operations


In my post about the [CRUD anti-pattern](http://verraes.net/2013/04/crud-is-an-anti-pattern/), I showed how to get rid of setters, using the Encapsulate Operations pattern. Let's now take the example of an Employee in an HR system.

{% highlight php %}
<?php
$employee = new Employee;
$employee->setName('Sarah Jones');
$employee->setPosition('Developer');
$employee->setSalaryScale(3);

// a few years later
$employee->setPosition('Lead Developer');
$employee->setSalaryScale(4);
{% endhighlight %}

Imagine we have a good talk with the client's HR director, and we've learned two important things about the domain:

- Once an employee is entered into the system, the name never changes.
- Promotions and Salary increases always go hand in hand.

We want to reflect these new insights in our domain model. First, we get rid of the `setName()` method, and make sure the
name can never be changed, by passing it in the constructor:

{% highlight php %}
<?php
$employee = new Employee('Sarah Jones');
{% endhighlight %}

Next we make sure that an employee always has a position and a salary scale, from the moment they are registered in the system.

{% highlight php %}
<?php
$employee = new Employee('Sarah Jones', 'Lead Developer', $salaryScale);
{% endhighlight %}

We get rid of the other setters, and add a method that communicates that a promotion is always accompanied by a salary change:

{% highlight php %}
<?php
$employee->promote($newPosition, $newSalaryScale);
{% endhighlight %}

Finally, we realize that in the real world, employees are not 'instantiated', they are hired. We make the constructor
private, and add a static method to make that explicit. The resulting code looks something like this:


{% highlight php %}
<?php
class Employee
{
    private $name;
    private $position;
    private $salaryScale;

    private function __construct($name, $position, $salaryScale)
    {
        $this->name = $name;
        $this->position = $position;
        $this->salaryScale = $salaryScale;
    }

    public static function hire($name, $forPosition, $withSalaryScale)
    {
        return new self($name, $forPosition, $withSalaryScale);
    }

    public function promote($toNewPosition, $withNewSalaryScale)
    {
        $this->position = $toNewPosition;
        $this->salaryScale = $withNewSalaryScale;
    }
}

$employee = Employee::hire('Sarah Jones', 'Developer', 3);
$employee->promote('Lead Developer', 4);
{% endhighlight %}


### Towards a task based UI

Now that we have our shiny model, we want to add a create and an edit form for the employee. We try using the
Symfony2 Form component, but very soon we get stuck. The component expects there to be a bunch of setters, and it doesn't
know how to deal with constructor arguments, let alone private constructors and static factory methods.

We don't want to reintroduce the setters, and screw up our domain model for the sake of the UI. But we don't want to make
forms by hand. How do we get out of this situation?

Let's think again about what the HR Director told us, about how they work. The department **hires** and employee, and later **promotes** them.
At no point he mentioned anything about **creating** and **editing** employees. Our own old-fashioned CRUD-thinking added those
words. So why are we still considering a "create" and an "edit" form? We really should be talking about a "hire" form and
 and a "promote" form. This is a central idea in Task Based UI's.

So we need a way to translate the result of our `HireEmployeeForm` and `PromoteEmployeeForm`, into our `Employee::hire()` and `$employee->promote()` methods.
This is starting to sound a lot like the Gang of Four Command pattern. As you may recall, a Command is an object that represents
 all the information needed to call a method. To represent the `Employee::hire()` method, we use a `HireEmployeeCommand`:

{% highlight php %}
<?php
class HireEmployeeCommand
{
    public $name;
    public $forPosition;
    public $withSalaryScale;
}
{% endhighlight %}

You can use public properties, or privates with getters and setters, that's really a matter of taste here. Both are ok, because the Command
 is just a very simple DTO object, with no behaviours. (I personally prefer getters and setters.)

 Let's add our `PromoteEmployeeCommand` as well:


{% highlight php %}
<?php
class PromoteEmployeeCommand
{
    public $employeeId;
    public $toNewPosition;
    public $withNewSalaryScale;
}
{% endhighlight %}

You'll notice that the Command nicely matches with the form fields we need.

The next step is self-evident: Instead of making the Form component work off our Employee Entity, it now uses our Commands.
The Form and the Domain Model have no knowledge of each other, apart from the Commands. The Commands function as a clear,
explicit API that outsiders use to pass instructions to the Domain Model. You can use these Commands for your Symfony2
Form, but they can just as well be used for your REST API or other clients.

That leaves us the matter of getting the data from the Command into the Employee instance. We could do this in our Controller:

{% highlight php %}
<?php
$employee = $this->employeeRepository->find(
    $promoteEmployeeCommand->employeeId
);
$employee->promote(
    $promoteEmployeeCommand->toNewPosition,
    $promoteEmployeeCommand->withNewSalaryScale
);
$this->entityManager->flush();
{% endhighlight %}

A better solution is to move that particular code into a CommandHandler. Maybe something for a later blog post.


## Read more

- [Form, Command, and Model Validation](/2015/02/form-command-model-validation/)
- [When to Use Static Methods](http://verraes.net/2014/06/when-to-use-static-methods-in-php/)

## Comments

### [CryptoCompress](https://twitter.com/CryptoCompress)  - 2013/04/20

Very nice Blog! Thank You!

off-topic:

#### "static function hire()" (1):
Static methods considered harmful. I use static-method-in-object instead in-factory for stateful objects too. Is this a
common approach? Maybe you have more information about it?
    
#### "static function hire()" (2):
A company hire "instantiated" employees. Bonus points for `$company->hire($employee)` :D

#### last code snippet
There is no obvious connection between repository and entity manager.
`$this->employeeRepository->persist($employee);`
or
`$this->entityManager->employeeRepository->find();`



### [Bernhard Schussek](https://twitter.com/webmozart) - 2013/04/21
FYI, the Form component is able to deal with constructors by using the (empty_data)[http://symfony.com/doc/current/cookbook/form/use_empty_data.html] option.

Of course, if you don't want to use any getters/setters, the automatic data mapping is pointless. It is there only as a
RAD helper to save some lines of code (especially when the entities also define validation constraints). But if you don't
 want that, your approach is a very good alternative. Another alternative would be to use an array-backed form.

