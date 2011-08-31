---
layout: post
title: "PHP to Django: Changing the engine while the car is running"
date: 2010-11-15 15:07
permalink: php-to-django-changing-the-engine-while-the-c
comments: true
categories: 
- django
---

It's pretty widely known that web frameworks like
[Django](http://djangoproject.com) and [Ruby on Rails](http://rubyonrails.org)
are amazing. They give developers the ability to build applications quickly and
reliably and there's all sorts of hotness when it comes to tools and community
and reusable applications. But what if *your* application isn't using a web
framework? Are you doomed to be outmaneuvered by your competitors?

## It Takes a Choice

PolicyStat, like most ideas, was born out of a prototype. In our case, the
prototype was written in PHP with the kind of architecture you'd expect from a
prototype. There were 45 .php files in a folder with one file called
class.main.php. No templates. No classes. No consistent database access. No
MVC. No ORM. No tests.

![First Commit](/images/posts/php-to-django/first_commit.png.scaled500.png)

Once our prototype took off and we realized there was a strong market for
policy management software done exactly the way we wanted to do it, we had a
decision to make. Should we focus on improving our existing code base? Port
things to one of the PHP web frameworks? Switch languages all together? As
someone who believes the [Single Worst Mistake](http://www.joelonsoftware.com/articles/fog0000000069.html)
a software company can make is rewriting from scratch, it was still
obvious that moving to Django was in the best interest of our company (and my
sanity). We decided to have our cake and eat it too.

## Integration Version 1

After a lot of hard thinking, we realized that if we could somehow get a Django
project to share sessions with our existing PHP prototype, we could keep both
codebases active at the same time while we slowly ported functionality from PHP
over to Django. Our integration process went something like this:

1. Create models.py files and break them up by functionality in to separate apps
  using Django's great [legacy database](http://docs.djangoproject.com/en/1.2/howto/legacy-databases/)
  support.
2. Build a [Django Session Engine](http://docs.djangoproject.com/en/dev/topics/http/sessions/#configuring-the-session-engine)
  using [phpserialize](http://pypi.python.org/pypi/phpserialize) to speak PHP's
  serialization format.
3. Duplicate the HTML templates spread out in various .php includes to Django's
  superior [Template Inheritance](http://docs.djangoproject.com/en/dev/topics/templates/#template-inheritance)
  system.
4. Separate PHP and Django based on URL paths via the Apache configuration
  (mod_php and mod_python at the time).

In the end, it only took a couple of weeks to get to a very basic level of
integration. A vast majority of PolicyStat was still in PHP, but a couple of
pages were served from Django. The key was that this change was seamless from a
user's perspective. We didn't even need to explain to customers that anything
was changing, because it didn't matter from their end. We were able to continue
delivering normal enhancements and bug fixes while doing the initial
integration.

## Toward 100% Django

Once you get the bare minimum level of integration, the hard, non-technical
problem to solve is what you should do from there. On one end, you can shut
down feature development and bug fixes for a few months while you attempt to
port the remaining PHP portions bit by bit. On the other end, you can choose to
be a dual-backend application forever and ever, amen.

We took the position that the PHP portions of our application were just pieces
of code that needed refactoring. Once we made that decision our process
naturally fell out; we already knew how to handle less-than-perfect code. When
we added new features, they were done in Python and Django. When we needed to
polish or otherwise fix functionality that lived in PHP, we looked at it just
like you'd look at that super-ugly method you wrote a couple of years ago when
you were in a hurry. If it was a very small change, we tended to just make the
change in PHP. If it was a decent chunk of change, or the change would be
easier in python, we first wrote unit tests on what we expected the behavior to
be using the existing code as a guide, then we ported the code to Django and
made the fix. During that time, we continued to deliver regular product
updates.

## 22 Months Later

So finally, after 22 months of regular updates and massive product improvements
(including a major architectural revampt to support multi-tenancy), we finally
removed the last bit of PHP. It was a very, very, very happy day for the
development team. To our customers though, it was just another update. That's a
good thing :)

![last php removal](/images/posts/php-to-django/last_php.png)

## Notes

*   Django and Ruby on Rails are the best examples, but there are plenty of
    other great frameworks. The major decision is between using a solid web
    framework and not using one.
*   Yes there are reasons not to move your app to a web framework, but if
    you're basing your business around an application, there aren't many
    situations where your organization wouldn't stand to benefit.
*   If anyone is interested, I'd be happy to do a followup post with a more
    technical look at the django session engine we used and maybe what our
    apache vhost file looked like.

Edit: Finally finished the follow-up post with technical details on [migrating
from php to django](/php-to-django-how-we-did-it).
