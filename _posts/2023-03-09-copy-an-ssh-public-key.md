---
layout: post
title: Copy an SSH Public Key
author: FrozenFOXX
tags:
 - tools
---

How often do you have to set up a new host and copy over your SSH public key for later access? It's annoying, can involve copy and pasting or arcane pipes to get it done. Turns out, built into OpenSSH there's a much easier way called `ssh-copy-id`:

```shell
ssh-copy-id -i ~/.ssh/id_rsa.pub user@target.system
```

This will let you log into a `target.system`, specify a local public key, and add it to the `authorized_keys` all in one step. Nice, simple, and effective.
