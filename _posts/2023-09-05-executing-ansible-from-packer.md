---
layout: post
title: Executing Ansible from Packer
author: FrozenFOXX
tags:
 - tools
 - devops
 - ansible
 - packer
---

[Ansible](https://www.ansible.com) is a popular configuration management tool which can be handy when building system images with [Packer](https://packer.io) as it's significantly more robust. There's a built-in provisioner for executing Ansible but it works a bit differently from running Ansible directly. We're going to talk a bit about how to run Ansible in Packer efficiently and reliably.

This is a working sample of how to use the Ansible provisioner:

```
  provisioner "ansible" {
    playbook_file = "${var.ANSIBLE_REPO_PATH}/target-playbook.yml"
    collections_path = "${var.ANSIBLE_REPO_PATH}/ansible_collections"
    galaxy_file = "${var.ANSIBLE_REPO_PATH}/requirements.yml"
    inventory_directory = "${var.ANSIBLE_INVENTORY_PATH}"
    ansible_env_vars = [
      "ANSIBLE_CONFIG=${var.ANSIBLE_REPO_PATH}/ansible.cfg"
    ]
    ansible_ssh_extra_args = [
      "-o UserKnownHostsFile=/dev/null",
      "-o StrictHostKeyChecking=no",
      "-o ControlPersist=60s"
    ]
    extra_arguments = [
      "-e",
      "ansible_user=${var.SSH_USERNAME}",
      "-e",
      "ansible_ssh_pass=${var.SSH_PASSWORD}",
      "-vvv"
    ]
    use_proxy = false
    user = "${var.SSH_USERNAME}"
```

Let's break down some of the parts here.

- `playbook_file`: self-explanatory, this is the playbook that's going to be run against the target. It's probably best to have `hosts: all` in it so Ansible will properly recognize regardless of what the host is called
- `collections_path`: if you're  using Ansible Collections (and you probably are) this is going to be the path to pick them up. Remember that Ansible was originally designed to run this from a dedicated host with an installation for orchestration so the directory path will be different from your roles. By default this is `[ansible path]/ansible_collections` and typically **needs** to be called `ansible_collections` to be recognized
- `galaxy_file`: One neat thing is that under normal circumstances you'd have to run `ansible-galaxy` twice to pick up roles and collections in the `requirements.yml` file but the provisioner here already understand that, so you only need the only file here
- `inventory_directory`: also self-explanatory, I recommend mixing a directory in from elsewhere
- `ansible_env_vars`: to set up dedicated overrides for execution you'll be able to put them here
- `ansible_ssh_extra_args`: this is probably the most important part to get things running smoothly so we'll examine each of these options in depth
  - `UserKnownHostsFile`: this ensures no collisions with an existing host file. You can change if you wish, but usually when using Packer there's no existing hosts you wish to collide with
  - `StrictHostKeyChecking`: like with SSH directly this prevents warnings about a host being untrusted which, again, is quite likely given you're spinning up a new host
  - `ControlPersist`: this is an important option since it helps Ansible maintain the control channel through SSH blips. We don't think about it most of the time but it DOES happen and given the sort of wait times involved with some Packer runs it's important for making sure Ansible doesn't error out
- `extra_arguments`: like with Ansible itself, this lets you use additional Ansible execution arguments and we should examine a few of them
  - `-e`: there's quite a few of these and they're passing environment variables in to Ansible, typically connection information
  - `ansible_user`: self-explanatory, the user you wish to connect via SSH with to the target
  - `ansible_ssh_pass`: also self-explanatory, the user's password to connect via SSH with to the target
  - `-vvv`: this **CAN** be removed if you wish but it's the only way to get debugging information out of the provider. If Ansible fails for any reason it will simply bubble up to a generic error in Packer, swallowing the output. When you set it to three (or four) levels of verbosity it captures all of that output from Ansible for easier debugging
- `use_proxy`: this is set to `false` by default now but it's not a bad idea to ensure it's set. This is an SSH proxy server inside of Packer which is being done away with
- `user`: the user to set up and connect with. Why is this required if `ansible_user` is already being set? Because the two do not always talk to each other and the Packer Ansible provisioner doesn't always pass it down like it's supposed to, so we explicitly set both

By following this setup, you should have a reliable and flawless way to execute Ansible from Packer. Happy provisioning!
