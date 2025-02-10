---
layout: post
title: Convert data and ASCII
author: FrozenFOXX
tags:
 - shell
 - devops
---

Ever have to supply the contents of a binary file from a text source like Hiera? It's rare but it's happened to me, particularly with Jenkins. Turns out there's a quick and easy way to do so using our old friend `dd`.

To convert from a binary data file to an ASCII file: `dd conv=ascii if=[data file] of=[ascii file]`.

To convert from an ASCII file back into a binary data file: `dd conv=ebcdic if=[ascii file] of=[data file]`

Hopefully you'll never find yourself in a position like this but it can be useful for all sorts of times when binary information needs to be read or written using tools that just aren't used to handling it.
