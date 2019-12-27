---
layout: post
title: Perl is Still a Requirement for open-vm-tools
author: FrozenFOXX
tags:
 - systems
 - linux
 - debian
 - ubuntu
 - packer
 - terraform
 - vsphere
---

I've recently been configuring up CICD pipelines with [Packer](https://www.packer.io) and [TerraForm](https://www.terraform.io) with vSphere and ran into an unexpected issue. I've been building the VM templates with Packer and then setting them to deploy with TerraForm. Problem is that while the templates can be cloned and boot just fine TerraForm would toss me an error about failing during VM customization. Even cranking up the debugging with `TF_LOG=TRACE` didn't seem to yield anything but that got me thinking, maybe the `open-vm-tools` package wasn't being installed correctly on my new PXE/netinstall Ubuntu hosts. Took a look at my preseed, turned out that yes, `open-vm-tools` was being installed and vSphere shows the host as running VMware Tools. So what gives?

Simple, `perl` is a package requirement for full functionality but isn't a dependency.

I'm sure there's probably a good explanation for this but since it's been several years and still hasn't happened I don't think it's likely to be fixed any time soon. Had the preseed install `perl` as well as `open-vm-tools` and now TerraForm is able to provision and customize the hosts without complaint.
