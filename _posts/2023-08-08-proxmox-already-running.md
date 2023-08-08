---
layout: post
title: Proxmox- already running
author: FrozenFOXX
tags:
 - tools
 - devops
 - linux
 - qemu
---

Using the [terraform provider](https://github.com/Telmate/terraform-provider-proxmox/tree/master) for Proxmox definitely functions but due to handling from the API there's still some bugs, at least as of `2.9.14`. When creating a QEMU VM with it, even if you've added the agent tools into the image, you have to set `agent = 0` in the Terraform provider. If you do not then after the VM boots Proxmox will attempt to boot it again, giving a "VM already running" error.

```shell
resource "proxmox_vm_qemu" "samplehostname" {
  os_type      = "cloud-init"
  count        = 1
  clone        = var.template
  name         = "samplehostname"
  target_node  = var.target_node
  onboot       = true
  agent        = 0
  qemu_os      = "other"
}
```
