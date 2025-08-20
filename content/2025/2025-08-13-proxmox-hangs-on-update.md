---
title: Proxmox Hangs on Update
tags:
  - devops
  - linux
  - troubleshooting
---
I use [Proxmox](https://proxmox.com) for my clustering. Unfortunately when upgrading it with a standard `apt upgrade` recently it would hang when it would get to `pve-ha-manager`. Nothing I did would resolve this, and it would hang the system.

To fix this, I did the following.
- kill the update process from another session (`ps -ef | grep apt`, find the process, `kill [process ID]`)
	- **Note**: it's possible you'll have to kill `dpkg` or similar, but basically ensure the update has stopped completely
- Mask the VM guests from starting with (`systemctl mask pve-guests.service`)
- Reboot the system
- Fix broken package updates (`dpkg --configure -a`)
- Rerun the updates (`apt dist-upgrade`)
- Re-enable VM guests on boot (`systemctl unmask pve-guests.service`)
- Reboot the system again

This should resolve the issue and let you continue normally.