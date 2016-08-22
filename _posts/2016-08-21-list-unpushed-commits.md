---
layout: post
title:  "List unpushed commits"
date:   2016-08-21 23:00:00
category: article
tags: [git, protip]
---

There are 2 ways to do it: simplified on a single line or not simplified with more details.

## Simplified form

{% highlight bash %}
$ git log --branches --not --remotes --simplify-by-decoration --decorate --oneline
{% endhighlight %}

**output**:

{% highlight bash %}
a0ea499 (HEAD, master) Fix html layout.
49f7b9c (hotfix) Fix javascript validation in contact form.
ff48529 (deletedusers) New model to admin deleted users.
{% endhighlight %}

## Complete form

Simply remove the following: `--oneline`

{% highlight bash %}
$ git log --branches --not --remotes --simplify-by-decoration --decorate
{% endhighlight %}

**output**:

{% highlight bash %}
commit a0ea499... (HEAD, master)
Author: Hernan <hernan@gmail.com>
Date:   Sun Ago 21 22:39:44 2016 -0300
{% endhighlight %}

    Fix html layout.

**but... is so long write this each time, so... we will create an alias :)**

Edit `.gitconfig` file

{% highlight bash %}
[alias]
        unpushed = log --branches --not --remotes --simplify-by-decoration --decorate --oneline
{% endhighlight %}

and then...

Use it!

{% highlight bash %}
$ git unpushed
{% endhighlight %}
