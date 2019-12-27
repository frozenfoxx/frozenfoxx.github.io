---
layout: post
title: OpenSSH + Cygwin
author: FrozenFOXX
tags:
 - howto
 - linux
 - opensource
 - wiki
 - automation
 - windows
 - cygwin
 - gnu
 - ssh
---
Finally a new entry on the Wiki!  I recently went through quite an ordeal trying to get SSH access to a Windows system and eventually settled upon a solution involving Cygwin.  It's a little painful to set up but once you DO have it functional it works pretty well and without fail.  The new entry is located [here](http://wiki.churchoffoxx.net/index.php?title=Cygwin) and includes notes about how to get remote users logged into the system if the Windows machine is part of a domain.  For some strange reason it seems that the standard instructions you find by using Google/Bing/Whatever generally stop with "ssh -v localhost." This I discovered doesn't work when the system is attached to a domain because the domain user's UID/GID need to be within the /etc/passwd and /etc/group files within the Cygwin environment.  If you DON'T do that with the mkpasswd/mkgroup commands as noted then it simply refuses login and ends up VERY frustrating.
