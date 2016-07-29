---
title: Semantic HTML with Twitter Bootstrap
slug: 2012-11-02-semantic-html-with-twitter-bootstrap
date: 2012-11-02
layout: post
published: true
tags: [blog]
---

The downside to using front-end frameworks like [Twitter Bootstrap](http://getbootstrap.com),
is that you're likely to end up with a HTML that contains a lot of styling logic. The
whole point of CSS was to get that styling out of the HTML! It's not the framework's fault, because
the framework obviously can't provide for your specific application. But it's very unfortunate
that the Bootstrap documentation does nothing to point you in the direction of proper semantic HTML.

### Semantic HTML

Here's a quick example showing semantic html. Let's make a button that no self-respecting spaceship can go without:

![Default Button](/img/posts/2012-11-02/default_button.png)

Bad:
{% highlight html %}
<button class="red">Auto-destruct</button>
{% endhighlight %}
{% highlight css %}
<!-- in the css file: -->
button.red { background-color:red; }
{% endhighlight %}

Good:
{% highlight html %}
<button class="autodestruct">Auto-destruct</button>
{% endhighlight %}
{% highlight css %}
<!-- in the css file: -->
button.autodestruct { background-color:red; }
{% endhighlight %}

### Encapsulate what varies

Now let's add Twitter Bootstrap to make the spaceship totally Web 3.0-compliant. If we do it by the book,
we'd write something like this:

![Default Button](/img/posts/2012-11-02/bootstrap_button.png)

{% highlight html %}
<button class="btn btn-warning">Auto-destruct</button>
{% endhighlight %}

We're now back to square one, because we coupled our HTML to the CSS implementation detail. Just like with code,
we want to [encapsulate what varies](https://www.google.com/?q=encapsulate%20what%20varies). In that last example,
if we want to change the button to yellow, we'd have to change `btn-danger` to `btn-warning`
for every auto-destruct button we have.

### LESS to the rescue

The way to solve this, is by using [LESS's](http://lesscss.org/) mixin feature. If you're using Bootstrap's
precompiled CSS files (bootstrap.css or bootstrap.min.css), you'll have a bit of extra to work to do. Get the
[LESS files from the GitHub repo](https://github.com/twitter/bootstrap/tree/master/less) and put them somewhere
in your project. You'll need a LESS compiler (or less-middleware in Node).

Now make a master.less file, have it compiled to master.css, and include only the master.css in your HTML's \<head\>.
Inside, put this:

{% highlight css %}
@import "bootstrap/less/bootstrap.less";
@import "bootstrap/less/bootstrap-responsive.less";
@import "spaceship.less";
{% endhighlight %}

This is important because we want to spaceship.less to have access to the styles defined in Bootstrap. This is how we
use a mixin in LESS:

{% highlight css %}
button.autodestruct {
    .btn;
    .btn-danger;
}
{% endhighlight %}

{% highlight html %}
<!-- html: -->
<button class="autodestruct">Auto-destruct</button>
{% endhighlight %}

The auto-destruct button will now inherit the styles from both the btn and the btn-danger classes. Using
this approach consistently will give you cleaner html that's easier to maintain.

### Bonus points

There's a lot of other advantages to compiling Bootstrap from LESS yourself. In the master.less file, you can replace
the first imports with a custom list. You can leave out what you don't need, set your own variables, ...

{% highlight css %}
@import "reset.less";
@import "your/own/copy/of/variables.less";
@import "mixins.less";
@import "scaffolding.less";
/* Etc ...  See bootstrap.less */
{% endhighlight %}

