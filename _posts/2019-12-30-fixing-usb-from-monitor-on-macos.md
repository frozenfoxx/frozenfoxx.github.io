---
layout: post
title: Fixing USB From Monitor on MacOS
author: FrozenFOXX
tags:
 - systems
 - macos
 - troubleshooting
---

Something super annoying to me with my MacBook is that sometimes when I plug it into a hub and monitors at work it doesn't recognize my USB keyboard. The monitor is plugged into my laptop via two methods, DisplayPort to USB-C for the video and USB-A to USB-A for the monitor's built-in USB hub. My MacBook Pro has USB-C ports and in one of those I have a port replicator. Naturally I plug my keyboard into the monitor and the monitor's hub is plugged into the port replicator; the monitor's video is plugged directly into one of the USB-C ports on the laptop.

When it doesn't recognize the keyboard I have video just fine, so what gives? Looking in the Console.app I can see a message `invalid destination port` repeated over and over every time I plug it in. This is likely the problem but I can't find anything about what to _do_ about it.

What's the fix? Unplug the monitor's USB-C cable connecting it to the MacBook directly, wait a few seconds, then plug it back in. Why does this work? I have absolutely no idea but it definitely fixes the problem. I'm guessing there's some sort of conflict between the two but can't find a good way to determine it. Hopefully if this happens to you it'll save you some pain.
