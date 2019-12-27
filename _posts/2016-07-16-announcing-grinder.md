---
layout: post
title: Announcing Grinder
author: FrozenFOXX
tags:
 - development
 - python
 - devops
 - saltstack
---
I'm announcing a new tool [Grinder](https://github.com/frozenfoxx/grinder) that hopefully people in the SaltStack community will find useful.  Grinder is a pretty shameless rip of [r10k](https://github.com/puppetlabs/r10k) but I noticed that a truly dynamic environment deployment tool didn't exist for SaltStack.  Given some of my work on it recently I found it an excellent project to both solve a real problem and learn some Python 3.

So what *is* Grinder?  It's a Salt Grinder naturally which breaks down the giant chunks of Salt code, mixes them with some seasoning, and distributes it in a pleasing fashion on your file system.  You simply point Grinder at your *states* and *pillar* repositories, modify the repos to include a `seasoning.yml` file and a standalone `top.sls`, and type in `./grinder`.  It will dynamically pull and create a directory tree structure for each branch that it finds in the *states* repository (as well as *pillar* repository, have to make sure those branches are identical for deployment) and eventually it will even reconfigure and kick the Salt Master.

Why do this?  For anyone that's used Dynamic Environments in Puppet with r10k it'll seem pretty obvious but the basic idea is to prevent multiple people from stepping on each other while developing their configuration management code.  Another benefit though is that this allows you to run `salt '*' state.apply saltenv=<branchname>` so you can dynamically test completely and wildly different code branches against the same systems without worrying about serious damage (if it didn't work, just run `salt '*' state.apply` and it's back to the way it was).  It also allows you to create new branches to test out normally-conflicting pillar data, wildly alter the layout of your codebase, and all sorts of other development things, all without causing massive issues for your regular production systems.

So what works?  Well almost everything now.  It will properly clone your *states* and *pillar* repositories, it will read the `seasoning.yml` file to determine what Salt formulas to pull in and distribute them in each branch, which is the vast majority of the work.  The last thing that doesn't work yet is to alter the `/etc/salt/master` configuration file to update the `file_roots` and `pillar_roots` definitions and kick the salt master.  So yeah, almost done already!  Naturally it would also be nice to figure out how to install it into a `pip`-able package.

Anyway, there's a __ton__ of awesome information on dynamic environments with Puppet out there that I'm not going to be able to adequately cover in this specific post but I strongly recommend looking into it and trying out Grinder if you're a SaltStack developer.  The tl;dr is that it lets you break things without breaking your production and makes management of the development pipeline significantly less complicated.  I hope you folks enjoy Grinder but please feel free to submit a pull request if you do not and want to make it better!
