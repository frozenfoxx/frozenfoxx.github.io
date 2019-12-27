---
layout: post
title: SSH Key Failure
author: FrozenFOXX
tags:
 - sysadmin
 - linux
 - opensource
 - hacking
 - ssh
---
Let me start by saying the following:  I *love* OpenSSH.

Quite seriously it's one of the most versatile tools in my toolbox and quite possibly ever created.  Its license allows it to be used on anything from anything to anything.  Its development team is fantastic and maintains rock solid reliability and compatibility like nothing else.  It allows you to build proxies, tunnels, and other encrypted twistings of network-fu.  It's small, it's lightweight, it comes in both server AND client, and oh yeah, you can even perform remote shell commands with it.

That said it's got a problem that's absolutely infuriating.  It's not very helpful when it comes to debugging SSH keypair issues.

Actually that'd be kind.  It's almost ANTI-helpful with regards to this issue. Now I know there's plenty of good reasons why even -vvv doesn't turn up much that's very useful when you're trying to figure out why you can't use a given private key to authenticate with a server but at the end of the day it's just kind of convoluted and EVERY administrator without exception I've run into has had at LEAST once a situation where he or she couldn't figure out why the bloody keypair exchange wasn't happening.

When that happens, it helps to have a list.  Without further ado, here's a list of common issues (at least on Linux systems and such) to check as most of the time it's going to be one of these.

* __Is the SSH server running?__  Seriously, it happens.  Don't worry, we've all done it.
* __Is SELinux Enforcing?__  Unless you know exactly why you're running SELinux this should be set to either "disabled (RHEL 5)" or "permissive (everything else)."
* __Is the firewall allowing port 22?__  If the firewall's running and you don't see an allow rule it's part of the problem.
* __Is the target OpenSSH configuration set to use AllowUsers/AllowGroups and that user/group is not in the list?__  This was a new one to me, but some systems have this value set in the sshd_config.  It's pretty rare which is another way of saying, "extra infuriating."
* __Are the target user's home directory permissions set to something OTHER than 700 or 770?__  If so SSH is going to barf on the key exchange.
* __Is the target user's ~/.ssh directory permissions set to something OTHER than 700?__  SSH is REALLY particular about its information being private.
* __Is the authorized_keys file permission something other than 600/640?__  I've worked on systems where setting this file to 644 causes it not to read the file.  Your mileage may vary.
* __Does the authorized_keys file have line breaks?__  This one REALLY sucks, but it's easy to fix.  Fire up the file, hit "end of line (or $ in vim)," and see if it ends up all the way at the end of the optional user@host flavor text.  Keys need to be one-per-line and sometimes when copy-pasting a public key your editor of choice will simply insert page breaks.

I hope this helps remove a little bit of administrator fury.  If there's really even MORE ways to screw up the key-based authentication I'll create a more updated post.
