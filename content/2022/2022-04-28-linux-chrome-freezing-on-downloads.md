---
layout: post
title: Linux Chrome Freezing on Downloads
author: FrozenFOXX
tags:
 - linux
---

Chrome on Linux has an interesting issue, particularly on Ubuntu. When downloading or uploading a file or certain other operations it will freeze entirely, but not crash. Turns out this is related to XDG and Chrome lookig to engage it but not having appropriate packages in some desktop environments. If you're on Ubuntu, install the `xdg-desktop-portal-gnome` package:

```
$ sudo apt install xdg-desktop-portal-gnome
```

Restart Chrome, should stop freezing.
