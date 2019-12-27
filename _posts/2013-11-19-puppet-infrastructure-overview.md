---
layout: post
title: Puppet Infrastructure Overview
author: FrozenFOXX
tags:
 - sysadmin
 - puppet
 - wiki
 - automation
---
Added a new entry to the [Puppet](http://wiki.churchoffoxx.net/index.php?title=Puppet) entry on the Wiki that I've been meaning to do for awhile.  It's a diagram that basically outlines how a complete Puppet infrastructure will work (not including MCollective, that's for another time).  It should be pretty obvious from the diagram how all the pieces fit together so I'm sorry I didn't have one earlier.  Basically you git clone the infrastructure files down to your workstation from the repo, make your commits, push back to the repo.  Then on the Puppetmaster using r10k you deploy all new branches from the repo.  Whenever puppet nodes run they store their exported information on PuppetDB and runs/facts/etc on Foreman (and PuppetDB, but that's less important for this part of the puzzle).

Anyway, I'm sorry there hasn't been more about Foreman and PuppetDB, I'll be updating those pages soon.
