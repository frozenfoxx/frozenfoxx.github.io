---
layout: post
title: Proxmox- cloud-init Template
author: FrozenFOXX
tags:
 - tools
 - devops
 - linux
 - qemu
---

Creating a cloud-init template on Proxmox isn't a complicated process but can be a little inscrutable. This set of steps from the command line (CLI) will give you a ready-to-use QEMU template of Ubuntu Lunar with the guest agent for Proxmox installed:

```shell
cd /opt/images
https://cloud-images.ubuntu.com/noble/current/noble-server-cloudimg-amd64.img
virt-customize -a noble-server-cloudimg-amd64.img --install qemu-guest-agent
qm create 9000 --memory 2048 --name ubuntu-2404 --bios ovmf --net0 virtio,bridge=vmbr0 --scsihw virtio-scsi-pci
qm set 9000 --scsi0 pool:0,import-from=/opt/images/noble-server-cloudimg-amd64.img
qm set 9000 --ide2 pool:cloudinit
qm set 9000 --boot order=scsi0
qm set 9000 --serial0 socket --vga serial0
qm template 9000
```

**Note**: `ide2` is chosen here due to working with the Proxmox API through the Terraform module. Due to a [bug](https://github.com/Telmate/terraform-provider-proxmox/issues/704) it will complain about not being able to mount on this target when cloning if it's set to `ide0`. This may likely be set back to `ide0` if this is ever fixed.

More on these steps can be found at [https://pve.proxmox.com/wiki/Cloud-Init_Support](https://pve.proxmox.com/wiki/Cloud-Init_Support).
