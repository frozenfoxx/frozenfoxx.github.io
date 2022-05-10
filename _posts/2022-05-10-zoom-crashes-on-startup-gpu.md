---
layout: post
title: Zoom Crashes on Startup (GPU)
author: FrozenFOXX
tags:
 - linux
---

Zoom on Linux has a habit of crashing immediately on startup, particularly after an upgrade. We can see the logs for it in `~/.zoom/logs`:

```
$ ~/.zoom/logs: ls
total 284
-rw------- 1 frozenfoxx frozenfoxx 4722008 Apr  2  2022 crash_5.6.1918.0420_box_2022-0402-211414.dmp
-rw-rw-r-- 1 frozenfoxx frozenfoxx 4353378 May 12 19:45 zoom_stdout_stderr.log
```

Looking in the `zoom_stdout_stderr.log` we will likely find a message similar to the following, complaining about the GPU:

```
[...]
[0510/293745.1923229:FATAL:gpu_data_manager_impl_private.cc(414)] GPU process isn't usable. Goodbye. zoom
[...]
```

Zoom's sandboxing for the application is preventing access to the GPU and thus it crashes. So what do we do about it? While attempting reinstallation or an older version is certainly a viable approach I haven't found it helps much. Instead, disabling the sandboxing seems to function best for all the peril that may bring.

First, test if the following works:

```
$ zoom --disable-gpu-sandbox
```

If Zoom fires up and you'd like to keep this behavior, modify the .desktop file to make it permanent:

**/usr/share/applications/Zoom.desktop**:
```
[...]
Exec=/usr/bin/zoom --disable-gpu-sandbox %U
[...]
```

Save the file, relaunch Zoom.
