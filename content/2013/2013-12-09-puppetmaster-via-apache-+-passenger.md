---
layout: post
title: Puppetmaster via Apache + Passenger
author: Foxx
tags:
 - sysadmin
 - puppet
 - linux
 - passenger
 - automation
 - ruby
---
I've updated the wiki [documentation](http://wiki.churchoffoxx.net/index.php?title=Puppet#Apache_.2B_Passenger) on Puppet with tested instructions on how to set up your Puppetmaster with Apache + Passenger.  The built-in WEBrick works fine for a good 25~50 nodes, but once you get bigger installations of Puppet WEBrick isn't really fast enough to serve them all.  Passenger is a module for Apache and nginx that allows you to run Ruby code in the form of Rails and Rack applications.  Setting up the Puppetmaster this way allows you to serve thousands and thousands of nodes from a single Puppetmaster and managing it is as simple as *service httpd restart*.  Enjoy!
