---
layout: post
title: TerraForm UUID Not Found
author: FrozenFOXX
tags:
 - terraform
 - devops
 - troubleshooting
 - vsphere
---

HashiCorp's [TerraForm](https://terraform.io/) is useful but can be very frustrating with its errors. One of the errors I've run into recently is `UUID Not Found` on a `terraform plan`. This can be especially frustrating since at least at the time of this writing TerraForm doesn't provide anything resembling a stack trace so there's no way to know _which_ resource is failing, only that one of them is having a problem.

Let's have a closer look at the error I was running into:

```
Error: disk.0: virtual machine with UUID "422d4584-53fb-f86c-4e10-05785bfd1e05" not found
```

Naturally that's not very helpful, there's nothing to say which module or machine is causing the problem, only that there's a disk issue of some kind. To find the offending host object in the state file I did this:

```
terraform state show module.[module name].vsphere_virtual_machine.main' | grep [UUID]
```

This will spit out if that module is the offender. If you HAVE found the offender, this error means that the base image that was used to deploy that host has changed UUIDs (probably from a rebuild of it) and that TerraForm has not reconciled the updated UUID. The only solution I've found that reliably works in my case is to remove the deployed host from the infrastructure manually and then rerun TerraForm to deploy it again.
