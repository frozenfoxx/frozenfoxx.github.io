---
layout: post
title: Adding Contributors to Concrete5
author: FrozenFOXX
tags:
 - howto
 - concrete5
 - website
---
So the other day as you might have noticed I had my first contributed blog entry for the Church of Foxx (yay).  I'd always hoped that eventually I could open the site to quite a few people to write editorials, guides, and other such informative things and at long last we've started to get some people who want to post something.  So that's great.

Small problem is that Concrete5 makes it an unholy pain in the ass in order to add contributor accounts that aren't administrators.  Seriously, this functionality is NOT located anywhere in the basic interface, something kind of surprising given it's a piece of BLOGGING software...you know, something you'd expect people to use to BLOG.  < sigh > Anyway, eventually I got it sorted thanks to a totally awesome guide [here](http://skybluesofa.com/blog/configuring-concrete5-composer-advanced-permissions/).  Technically I think it's slightly out of date but that's okay, it's very close and gives you everything you need.

Basically what you need to do is enable advanced permissions, add a group that contributor accounts can be a part of, and then assign that group the correct permissions for the composer.  This isn't nearly as easy as it sounds since editing Composer's permissions isn't enough.  Instead you need the group to be able to visit the Writer, visit Drafts, View Drafts, Edit page properties, and a few dozen other permissions.  If you don't follow it to the letter then chances are pretty good that you'll get your contributors able to log in, able to see the drafts window, but anytime they click, "write," it just kicks them back to Drafts.

Something else to consider that's great for security is the [Blog Author Hub](http://www.concrete5.org/marketplace/addons/blog-author-hub/).  Simply take this stack, put it on your front page somewhere unobtrusive, and set the permissions of that page such that only you and your contributor group can "view" it.  This means casual site visitors will be wholly unaware but any of your contributors will see it immediately.  It's basically a way to allow contributors to directly submit articles by jumping straight into the Composer.  This is a Good Thing (tm) because otherwise (at the current time) you have to provide full view access to the entire Dashboard, something you generally don't want your friends and partners having access to if you wish them to remain friends.

Anyway, while it took some messing around given the above guide it wasn't so bad.  Enjoy.
