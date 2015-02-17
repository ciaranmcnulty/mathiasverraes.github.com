---
title: Keep your controllers thin with Doctrine2
slug: keep-you-controllers-thin-with-doctrine2
date: 2011-03-23
layout: post
published: true
tags: [blog]
---

Doctrine2 does such a nice job abstracting everything related to the database, that you might be tempted to do everything else in your controllers. Say we have a Bug entity:

{% highlight php %}
<?php
/** @Entity */
class Bug
{
    /** @Column(type="integer") */
    private $id;
    /** @Column(length=50) */
    private $status;
    //...
}
{% endhighlight %}

To get a list of fixed bugs, we get the Bug repository from the EntityManager and ask for a list of Bugs where status equals ‘fixed’.

{% highlight php %}
<?php
<?php
// $em instanceof Doctrine\ORM\EntityManager
$fixedbugs = $em->getRepository('Bug')
    ->findBy(array('status' => 'fixed'));
{% endhighlight %}

That’s easy enough. Surely there can be no harm in having this code inside a controller? Although this code doesn’t look like one, it is in a fact a database query. It’s a shortcut for this:

{% highlight php %}
<?php
$fixedbugs = $em
    ->createQuery("SELECT b FROM Bug b WHERE b.status = 'fixed'")
    ->getResult();
{% endhighlight %}

Having a query in our controller should ring some serious alarm bells. It means that despite all abstraction Doctrine2 provides, we are at this point still coupling the controller to the database. If one day we decide to change how bug status is represented in the database, we’d need to modify all our controllers.

Let’s have a closer look at the Repository. [Evans](http://domaindrivendesign.org/books/evans_2003) defines it as “an object that can provide the illusion of an in-memory collection of all objects of that type”, that clients talk to using the domain language. In other words, findBy(array(‘status’ => ‘fixed’)) is too generic: in domain language, we want to ask the repository to findAllFixedBugs(). If the database schema changes, we’ll only have to change that method.  Luckily Doctrine2’s repositories can be extended:

{% highlight php %}
<?php
/**
 * @Entity(repositoryClass="BugRepository")
 */
class Bug
{ /* ... */ }

class BugRepository extends EntityRepository
{
    public function findAllFixedBugs()
    {
        return $this->_em
            ->createQuery("SELECT b FROM Bug b WHERE b.status = 'fixed'")
            ->getResult();
    }
}
{% endhighlight %}

Finally we can replace the code in our controller with this

{% highlight php %}
<?php
$fixedbugs = $em->getRepository('Bug')->findAllFixedBugs();
{% endhighlight %}

Our controller is now decoupled from the database. As an added bonus, the code does a much better job of communicating it’s intent than our first version ever could.


## Comments

### Torkil Johnsen - 2012/03/23
Looks extremely nifty, but left me with questions. I'm not familiar with Doctrine, and you probably just wrote this as a crude example, so forgive the silly question! :)

I'm wondering why you write something as specific as findAllFixedBugs()? Seems to me like you'll need a findAllFixedBugsFilteredByUser() and findAllFixedBugsWithStatusCritical() around the next bend?

And why did you not use findBy() in your final version of the method, but instead write a regular SQL statement?

Other than that: Step up the pace on the blog posts, we need more of these :)

### Mathias Verraes  - 2012/03/23
It's indeed a crude example.

You would indeed need more methods, which you would discover as you write tests (see my previous post). The point of having these methods is that your code is speaking the same language as you speak with your customer (called 'ubiquitous language' in DDD). If you decide to move your bugs from a relational database to, say, a remote service, your methods would still be there, but the implementation would be entirely different. And of course, you can easily replace BugRepository with a mock object during testing, so you wouldn't need a database to test your controller.

There's no particular reason I didn't use findBy() in the last snippet, except that I wanted to illustrate that the method can contain complex queries that can't be done with find by.

Off topic: the queries in the example are DQL, not SQL. http://docs.doctrine-project.org/projects/doctrine-orm/en/2.0.x/reference/dql-doctrine-query-language.html

### Alessandro Nadalin - 2012/03/23
Hi Torkil,

yes, the repository patterns was specifically born to incapsulate all the methods you need to retrieve objects from the DB.
So you basically add there all the possible queries you need in your application...

You are obviously able to abstract some methods using a custom class between your repositories and the EntityRepository, which inherits from the latter.

To use OO api and not raw DQL statements you need a QueryBuilder instance:
{% highlight php %}
<?php
$qb = $em->getRepository('Odino\BlogBundle\Entity\Content')->createQueryBuilder('u')
    ->where('u.isActive = 1')
    ->andWhere("u.keywords LIKE ?1")
    ->setParameter(1, "%$tag%");

$qb->getQuery()->execute();
{% endhighlight %}

ciao!

### Torkil Johnsen - 2012/03/23
Thanks to you both, enlightening!


