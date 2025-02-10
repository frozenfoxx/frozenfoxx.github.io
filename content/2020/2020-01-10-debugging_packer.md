---
layout: post
title: Debugging Packer
author: FrozenFOXX
tags:
 - packer
 - devops
 - troubleshooting
 - vsphere
---

HashiCorp's [Packer](https://packer.io/) is a tremendously useful if annoying to use tool. While it's very flexible and extendable you can sometimes run into errors that are difficult to debug. My personal favorite is this one:

```
➜  packer git:(master) docker run -it --rm [lots of variables] --name=packer [custom registry]/packer:latest build -force [path to a template]
Failed to initialize build 'vsphere-iso': error initializing builder 'vsphere-iso': fork/exec /bin: permission denied
```

Has this happened to you? Is that annoying? Trying to track down whether it's a plugin (in this case the excellent [JetBrains Plugin](https://github.com/jetbrains-infra/packer-builder-vsphere), your Dockerfile, or something else? Yeah, me, too. Wouldn't it be nice if there was a verbose option?

Well, there sort of is. Kinda.

In the `packer` command you can supply `-debug` which will instruct the builders to give debugging info. That's great in theory but what if you're trying to debug why the builder won't initialize, such as the above instance? Instead what you're going to want is an environment variable, `PACKER_LOG=1`. Supply this and all the magical info you're looking for from Packer itself will dump:

```
➜  packer git:(master) docker run -it --rm [lots of variables] -e PACKER_LOG=1 --name=packer [custom registry]/packer:latest build -force [path to a template]
2020/01/11 02:03:13 [INFO] Packer version: 1.5.0 [go1.13.5 linux amd64]
2020/01/11 02:03:13 [DEBUG] Discovered plugin: vsphere-clone = /bin/packer-builder-vsphere-clone.linux
2020/01/11 02:03:13 [DEBUG] Discovered plugin: vsphere-iso = /bin/packer-builder-vsphere-iso.linux
2020/01/11 02:03:13 using external builders [vsphere-clone vsphere-iso]
2020/01/11 02:03:13 Attempting to open config file: /root/.packerconfig
2020/01/11 02:03:13 [WARN] Config file doesn't exist: /root/.packerconfig
2020/01/11 02:03:13 Setting cache directory: [path to a cache dir]
2020/01/11 02:03:13 Plugin could not be found at  (exec: "": executable file not found in $PATH). Checking same directory as executable.
2020/01/11 02:03:13 Current exe path: /bin/packer
2020/01/11 02:03:13 Creating plugin client for path: /bin
2020/01/11 02:03:13 Starting plugin: /bin []string{"/bin"}
Failed to initialize build 'vsphere-iso': error initializing builder 'vsphere-iso': fork/exec /bin: permission denied
2020/01/11 02:03:13 Build debug mode: false
2020/01/11 02:03:13 Force build: true
2020/01/11 02:03:13 On error:
2020/01/11 02:03:13 Waiting on builds to complete...
==> Builds finished but no artifacts were created.
2020/01/11 02:03:13 [INFO] (telemetry) Finalizing.
```

Now THAT is a lot more useful. Turns out the Packer interface in this case had changed from 1.4.5 to 1.5.0+ and they introduced some bugs with the new interface. Plugins that were retooled to the new interface still wouldn't work and as of today they're still trying to patch it, meaning I have to downgrade to 1.4.5 until they fix it. I hope this helps you if you stumble across this problem!
