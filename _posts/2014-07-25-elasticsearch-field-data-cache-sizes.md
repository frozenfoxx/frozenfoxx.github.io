---
layout: post
title: ElasticSearch Field Data Cache Sizes
author: FrozenFOXX
tags:
 - logstash
 - hacking
 - reference
 - wiki
 - elasticsearch
 - tuning
---
Do you have a Logstash/Kibana/ElasticSearch setup?  Does your ElasticSearch occasionally seem to just keep allocating memory far beyond its bounds?  Do you have issues where during a short-term search in Kibana everything's fine but hours later ElasticSearch is brought to its knees with heavy swap usage?

Then you need [Field Data Cache Size Limits](http://wiki.churchoffoxx.net/index.php?title=Logstash#Limit_Field_Data_Cache_Sizes)!

At the bottom of the Logstash page is a quick section where adding just two lines into the elasticsearch.yml and kicking ElasticSearch will stop this embarrassing situation from happening.  Basically by default ElasticSearch sets the field data cache size to unbounded.  This means that it's VERY easy to accidentally issue a query that's going to require significantly more storage in memory than you had planned on.  In the worst case this may even be more memory than exists on the machine!

Simply setting the percentage of allocated memory to use for the field data cache and a time to expiration ensures that this sort of overflow isn't going to be an issue for your installation.  It's fast, easy, and hopefully will lead to less sleepless nights trying to figure out what in the heck ES is doing.

Update 1:  my terrible apologies, I meant field data cache, not index buffer sizes.  This has been corrected as well as the links.
