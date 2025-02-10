---
layout: post
title: Puppet Module - whisper
author: FrozenFOXX
tags:
 - sysadmin
 - puppet
 - opensource
 - github
---
Just finished a small but useful Puppet module, [puppet-whisper](https://github.com/frozenfoxx/puppet-whisper).  If you've ever had to try and set up Graphite (which IS totally awesome) then you know there's a bunch of pieces and parts to configure and install.  Ideally you should be able to use Puppet, Chef, or similar to get it all done for you but so far I haven't seen any Graphite modules I particularly like and definitely no individual modules for Whisper (I really hate it when it's just auto-bundled, what if another project needs it?).  Please feel free to try it out, post up any problems or merge requests and I'll get to it as soon as I can.

I'd say my only real issue with this module is that it requires Hiera.  I don't like requiring that kind of thing since it excludes some Puppet 2.6 users but honestly it's SO much simpler to specify this stuff with Hiera and keep the module code down for simplicity, not to mention Hiera's included by default in Puppet 3.0+.  Anyway, enjoy!
