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

Creating a cloud-init template on Proxmox isn't a complicated process but can be a little inscrutable. This set of steps from the command line (CLI) will give you a ready-to-use QEMU template of Ubuntu Jammy with the guest agent for Proxmox installed:

```shell
cd /opt/images
wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img
virt-customize -a jammy-server-cloudimg-amd64.img --install qemu-guest-agent
qm create 9000 --memory 2048 --name ubuntu-2204 --net0 virtio,bridge=vmbr0 --scsihw virtio-scsi-pci
qm set 9000 --scsi0 pool:0,import-from=/opt/images/jammy-server-cloudimg-amd64.img
qm set 9000 --ide2 pool:cloudinit
qm set 9000 --boot order=scsi0
qm set 9000 --serial0 socket --vga serial0
qm template 9000
```

More on these steps can be found at [https://pve.proxmox.com/wiki/Cloud-Init_Support](https://pve.proxmox.com/wiki/Cloud-Init_Support).
