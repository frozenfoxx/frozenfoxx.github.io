---
layout: post
title: No Usable libssl Found
author: FrozenFOXX
tags:
 - linux
 - OpenSSL
---

Ubuntu 22.04 and other more recent distributions have dropped OpenSSL 1 and include only OpenSSL 3. Unfortunately that means some applications that depend on OpenSSL 1.0 or 1.1 will no longer work and report `no usable libssl found` which isn't exactly the most helpful message. Fortunately you can still install the older OpenSSL libraries alongside the current ones, they're available to retrieve [here](http://security.ubuntu.com/ubuntu/pool/main/o/openssl1.0/). You'll want to perform the following as of this writing:

```
wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl1.0/libssl1.0.0_1.0.2n-1ubuntu5.10_i386.deb
wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl1.0/libssl1.0.0_1.0.2n-1ubuntu5.10_amd64.deb
sudo dpkg -i ./libssl*.deb
```
