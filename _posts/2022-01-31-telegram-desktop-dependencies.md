---
layout: post
title: Telegram Desktop Dependencies
author: FrozenFOXX
tags:
 - linux
---

On some Linux distributions [Telegram](https://telegram.org/) is packaged as `telegram-desktop` but doesn't actually include all dependencies to make it run properly. You can see this pretty plainly with messages like this:

```
(telegram-desktop:12417): GLib-GObject-WARNING **: 08:22:03.129: cannot register existing type 'GdkDisplayManager'

(telegram-desktop:12417): GLib-CRITICAL **: 08:22:03.129: g_once_init_leave: assertion 'result != 0' failed

(telegram-desktop:12417): GLib-GObject-CRITICAL **: 08:22:03.129: g_object_new_with_properties: assertion 'G_TYPE_IS_OBJECT (object_type)' failed

(telegram-desktop:12417): GLib-GObject-WARNING **: 08:22:03.129: invalid (NULL) pointer instance

(telegram-desktop:12417): GLib-GObject-CRITICAL **: 08:22:03.129: g_signal_connect_data: assertion 'G_TYPE_CHECK_INSTANCE (instance)' failed

(telegram-desktop:12417): GLib-GObject-WARNING **: 08:22:03.129: invalid (NULL) pointer instance

(telegram-desktop:12417): GLib-GObject-CRITICAL **: 08:22:03.129: g_signal_connect_data: assertion 'G_TYPE_CHECK_INSTANCE (instance)' failed

(telegram-desktop:12417): GLib-GObject-WARNING **: 08:22:03.129: cannot register existing type 'GdkDisplay'

(telegram-desktop:12417): GLib-CRITICAL **: 08:22:03.129: g_once_init_leave: assertion 'result != 0' failed

(telegram-desktop:12417): GLib-GObject-CRITICAL **: 08:22:03.129: g_type_register_static: assertion 'parent_type > 0' failed

(telegram-desktop:12417): GLib-CRITICAL **: 08:22:03.129: g_once_init_leave: assertion 'result != 0' failed

(telegram-desktop:12417): GLib-GObject-CRITICAL **: 08:22:03.129: g_object_new_with_properties: assertion 'G_TYPE_IS_OBJECT (object_type)' failed
```

So what's wrong? It's missing a libappnotifier library for GTK3. Simply install it with your distribution's package manager. Some examples of this are as follows:

* **Arch Linux**: `pacman -S libappindicator-gtk3`
* **Debian / Ubuntu**: `apt install libappindicator3-1`
* **Raspberry Pi OS**: `apt install libappindicator3-1`
