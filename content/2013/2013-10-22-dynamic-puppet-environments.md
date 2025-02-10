---
layout: post
title: Dynamic Puppet Environments
author: FrozenFOXX
tags:
 - puppet
 - sysadmin
 - hacking
 - management
 - social
 - github
---
If you're like me and you use Puppet (because Puppet's awesome) then chances are good you don't work alone but in an environment with at least one other admin.  Given the way technical people are, if there's not a clearly defined process for how to work together we end up stepping on each other's toes and before you know it someone ends up with a NSFW image as a desktop background.  The fix for this is obviously a process but any well-defined process for managing software can typically be expressed in terms of tools as well and tools, due to their impartial nature, tend to work as a fantastic middle ground for resolving differences of opinions between tech people.  One of the best tools at the moment for this sort of thing is Git.  It's distributed, it allows for all sorts of workflows, it's fast, supports tons of branching, and it's strong enough even to tame "social" coding (a la Github).  Problem is, how can we bring this wondertool to Puppet?

Enter [r10k](https://github.com/adrienthebo/r10k).

r10k allows you to combine the raw power of environment automation with Puppet with the sanity management of Git.  The super short version is you install the r10k gem into your Puppetmaster(s), configure the /etc/r10k.yaml to point to your existing Git repo, and get ready to do some maintenance.  Once you're doing with migrating things to use Puppetfile and git, the general workflow is you check out your repo via git clone, create a new branch, make your changes like any other software project, and then on the Puppetmaster run "r10k deploy environment -p." r10k then rolls out all branches as individual environments under /etc/puppet/environments/.  Use "puppet agent --test --environment" on your target nodes until you're happy, merge your branch back to the production branch, and do "r10k deploy environment -p."

As you can imagine, this has fantastic possibilities.  You can manage your Puppet environment just like any other software project with lots of developers due to Git.  You can create new modules and new branches for each and every feature if you wish, or do it monolithically, or however you want due to git's flexibility.  When you're ready, just hit the Puppetmaster with an r10k deploy and suddenly environments are created, ready to roll.  There's even Github modules for modifying Puppet with a prerun hook to do this every time it compiles a catalog.

That said, there's a few caveats that will help with understanding how to use and deploy this thing.

* Your "internally developed" modules you should move out of "modules" and into a directory called "dist." In your /etc/puppet/puppet.conf you can specify multiple modulepaths with a colon separator, include both "dist" and "modules" and Puppet will automatically glom them all together.  If you don't, r10k will override your internal modules with ones you specified in the Puppetfile.
* Place the Puppetfile in the root of your repo and use it to specify modules individually from Puppet Forge, Github, your own Repo, or wherever.  BUT, and this is a BIG but, it's for INDIVIDUAL MODULES.  Mass group of modules?  Get ready for a big Puppetfile.
* Generally you're going to want to make your Puppet repo at the root of the environment (aka "/etc/puppet/environments//{manifests,modules,dist}").  This way you get the manifests, internally made modules, and the Puppetfile.

Otherwise while the documentation's weird it should be fairly straightforward.  Gary Larizza's blog posting about how to deploy and work with r10k goes into extensive detail complete with a workflow (his blog post here).  I strongly recommend using r10k, it's just a fantastic method of making sure that everyone interacting with the Puppet setup from developer to administrator can do their work in peace and conflicts will only arise when merging, just like any other project.
