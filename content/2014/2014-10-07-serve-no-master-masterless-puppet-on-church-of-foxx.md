---
layout: post
title: Serve No Master! Masterless Puppet on Church of Foxx
author: FrozenFOXX
tags:
 - puppet
 - opensource
 - wiki
 - github
 - hiera
 - r10k
---
So the site has now finally been moved! We’re no longer on DreamHost and sad as I am to leave as DreamHost has been fantastic we’re now running fantastic on [Digital Ocean](https://churchoffoxx.net/index.php/blog/serve-no-master-masterless-puppet-church-foxx/%E2%80%9Ddigitalocean.com%E2%80%9D). If you’re not familiar, Digital Ocean is a very cheaply-costed VPS provider whose big claim to fame is that all Virtual Private Servers are backed by SSD drives. As a result this makes any disk operations lightning fast. As a side note, my PHP-based blog and wiki happen to run MUCH better on Digital Ocean than DreamHost. I still recommend DreamHost as they are cheap, easy-to-use even for non-admins, and their monthly newsletter is always entertaining.

So, how’d the move go? Interestingly. Since I’m on a VPS now instead of a shared host I thought, “why not use Puppet?” Usually you use Puppet in an infrastructure with a master and a whole bunch of other stuff but there’s nothing stopping you from using Puppet in a completely standalone, “masterless,” setup. In fact it’s actually a fantastic way to have configuration management in any environment where all that scaffolding and management doesn’t make sense, like a laptop or even a personal website. So that’s what I’ve done here.

[https://github.com/frozenfoxx/puppet-churchoffoxx](https://github.com/frozenfoxx/puppet-churchoffoxx)

Right about the only things NOT done via Puppet at this point are the unrolling of the Wiki and the construction of the MySQL databases. I didn't really like any of the modules on [Puppet Forge](http://forge.puppetlabs.com/) and being in a hurry I just did it myself. That said at some point I'll go back and adjust it to do those as well, though I think I'm going to have to do the wiki module myself unfortunately. Still, this is pretty close complete with setting up MySQL, setting root password, setting up Apache with the vhosts, and so forth.

“Okay, so I see the logic, but where’s all the data?” Good question, I’m still using Hiera for that which is standard in Puppet 3.x and I strongly recommend it. It’s fast, it makes changing data values as simple as editing a YAML file and saving, and best of all if you have an issue with your Puppet code you can freely share it with another party without fear of giving up your precious data. This last one, decoupling data and logic, really is a HUGE win and makes issues you may encounter someday significantly easier to diagnose.

I’ve included a [sample](https://churchoffoxx.net/index.php/blog/serve-no-master-masterless-puppet-church-foxx/%E2%80%9Dhttps://github.com/frozenfoxx/puppet-churchoffoxx/blob/production/hiera-example.yaml%E2%80%9D) Hiera YAML file in the base of the directory that’s pretty close to what I’m using for Church of Foxx but obviously with a few values changed. ;) All you’ve gotta do is drop either this or a similar-looking file into /etc/puppet/hieradata/common.yaml and remember to configure your /etc/puppet/hiera.conf to look for that file (it’s a standard, common location so it might even work automatically but I recommend checking the config as it’s really simple and well-documented). If I don’t have this in the Wiki of Foxx already I’ll get it in there soon.

Then all you’ve gotta do is drop a call to “__r10k deploy environment –p__” and then the magic line “__puppet apply /etc/puppet/environments/*branch*/site.pp__” into root’s crontab and DONE! Slick, simple, easy-to-update site with decoupled data that requires no master! SERVE NO MASTER!

Ahem.

Anyway, you can use this as a model for rolling out your own masterless setups really easily, just use r10k to unroll that deployment in place and make that puppet apply call and you’re good to go.
