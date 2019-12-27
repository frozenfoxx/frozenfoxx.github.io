---
layout: post
title: Puppet 4 Upgrade
author: FrozenFOXX
tags:
 - puppet
 - development
 - automation
 - git
 - howto
---
The Church of Foxx is finally running Puppet 4!  Woooo!  The [repo](https://github.com/frozenfoxx/puppet-churchoffoxx) has had the *puppet4* branch merged into production, the server's been upgraded, and now set to run and deploy appropriately.  Honestly there's not a whole lot to it but you'll notice that now I'm using the *roles and profiles* programming pattern.  This isn't strictly speaking required for Puppet 4 but I __strongly__ recommend it.  It's extendable, flexible, and once you understand how nodes only include one role, and roles include multiple patterns it makes a lot of sense.

To get this working it's critical that you make an *environment.conf* or just use the one I've supplied in the repo.  Without this Puppet won't know where to look for its components and will get all pissy.  This speaks to my only real complaint about Puppet 4, namely that the *roles and profiles* pattern isn't automatically set up.  It goes so far to get you there but the last pieces you have to drop in yourself.  For Church of Foxx I did the following:

- removed Puppet 3
- installed Puppet 4 (puppet-agent)
- *mkdir /etc/puppetlabs/r10k*
- created an r10k.yaml that specified the :datadir: as */etc/puppetlabs/code/hieradata*
- created my */etc/puppetlabs/code/hieradata/common.yaml*
- ran */opt/puppetlabs/puppet/bin/gem install r10k*
- ran */opt/puppetlabs/puppet/bin/r10k deploy environment -pv*
- commited my *environment.conf* into the base of the repo (deployed by *r10k*)
- ran */opt/puppetlabs/puppet/bin/puppet apply /etc/puppetlabs/code/environments/production/manifests/site.pp*

That's not alot of awful steps, but I had to research a few of them my first time deploying Puppet 4.  It really feels like a few of these should be set up automatically, like a skeleton of the *site/* directory and the *environment.conf*. Still, with these minor complaints aside at my work I've been rocking out a more robust version of this setup for months without issue.  This site's repo should give you a very good idea once more about how you can get started in rolling out your own awesome Puppet setup.
