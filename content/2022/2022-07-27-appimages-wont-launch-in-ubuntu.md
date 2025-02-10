---
layout: post
title: AppImages Won't Launch in Ubuntu
author: FrozenFOXX
tags:
 - linux
 - appimage
---

Did your old AppImages work just fine previously but now don't launch properly on Ubuntu 22.04? Launching from the command-line you'll likely see something like this:

```
➜  apps ./Plexamp-4.2.1.AppImage 
dlopen(): error loading libfuse.so.2

AppImages require FUSE to run. 
You might still be able to extract the contents of this AppImage 
if you run it with the --appimage-extract option. 
See https://github.com/AppImage/AppImageKit/wiki/FUSE 
for more information
```

This is because Ubuntu 22.04+ no longer includes FUSE by default. To fix this, install the library:

```
➜  apps sudo apt install -y libfuse2
```
