---
layout: post
title: RVM Cannot Compile on Ubuntu 22.04
author: FrozenFOXX
tags:
 - linux
 - ruby
---

Since upgrading to Ubuntu 22.04 the popular [RVM](https://rvm.io) tool has stopped being able to install or use Ruby:

```
➜  ~ rvm install 3.0
Searching for binary rubies, this might take some time.
No binary rubies available for: ubuntu/22.04/x86_64/ruby-3.0.0.
Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.
Checking requirements for ubuntu.
Requirements installation successful.
Installing Ruby from source to: /home/frozenfoxx/.rvm/rubies/ruby-3.0.0, this may take a while depending on your cpu(s)...
ruby-3.0.0 - #downloading ruby-3.0.0, this may take a while depending on your connection...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 18.6M  100 18.6M    0     0  14.7M      0  0:00:01  0:00:01 --:--:-- 14.7M
ruby-3.0.0 - #extracting ruby-3.0.0 to /home/frozenfoxx/.rvm/src/ruby-3.0.0 - please wait
ruby-3.0.0 - #configuring - please wait
ruby-3.0.0 - #post-configuration - please wait
ruby-3.0.0 - #compiling - please wait
Error running '__rvm_make -j12',
please read /home/frozenfoxx/.rvm/log/1653505265_ruby-3.0.0/make.log

There has been an error while running make. Halting the installation.
```

Looking in the `/home/frozenfoxx/.rvm/log/1653505265_ruby-3.0.0/make.log` it looks like it's having a problem with OpenSSL:

```
[...]
make[2]: Leaving directory '/home/frozenfoxx/.rvm/src/ruby-3.0.0/ext/openssl'
make[1]: *** [exts.mk:260: ext/openssl/all] Error 2
[...]
```

Turns out this is an incompatibility introduced by Ubuntu's switch to OpenSSL 3.0. Previously many Rubies shipped with OpenSSL 1.1 and many Gems relied on it. Supposing you don't want to downgrade the system OpenSSL to 3.0 which would have its own issues, what are you supposed to do?

While a little cumbersome there's a way to get everything you need still contained in the RVM installation. First, use the RVM `pkg` command to install a local build of a compatible OpenSSL:

```
rvm pkg install openssl
```

This will create a compatible OpenSSL build for those Rubies that need it. Now when installing Rubies that have a problem with OpenSSL (pretty much any that are older than 3.1) you can redirect them to use the locally supplied OpenSSL library:

```
rvm install [version] --with-openssl-dir=$HOME/.rvm/usr
```

Here's a sample involving Ruby 2.4:

```
➜  ~ rvm install 2.4 --with-openssl-dir=$HOME/.rvm/usr
Checking requirements for ubuntu.
Requirements installation successful.
Installing Ruby from source to: /home/frozenfoxx/.rvm/rubies/ruby-2.4.10, this may take a while depending on your cpu(s)...
ruby-2.4.10 - #downloading ruby-2.4.10, this may take a while depending on your connection...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 11.9M  100 11.9M    0     0  17.4M      0 --:--:-- --:--:-- --:--:-- 17.4M
ruby-2.4.10 - #extracting ruby-2.4.10 to /home/frozenfoxx/.rvm/src/ruby-2.4.10 - please wait
ruby-2.4.10 - #configuring - please wait
ruby-2.4.10 - #post-configuration - please wait
ruby-2.4.10 - #compiling - please wait
ruby-2.4.10 - #installing - please wait
ruby-2.4.10 - #making binaries executable - please wait
ruby-2.4.10 - #downloading rubygems-3.0.9
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  865k  100  865k    0     0  5337k      0 --:--:-- --:--:-- --:--:-- 5309k
No checksum for downloaded archive, recording checksum in user configuration.
ruby-2.4.10 - #extracting rubygems-3.0.9 - please wait
ruby-2.4.10 - #removing old rubygems - please wait
ruby-2.4.10 - #installing rubygems-3.0.9 - please wait
ruby-2.4.10 - #gemset created /home/frozenfoxx/.rvm/gems/ruby-2.4.10@global
ruby-2.4.10 - #importing gemset /home/frozenfoxx/.rvm/gemsets/global.gems - please wait
ruby-2.4.10 - #generating global wrappers - please wait
ruby-2.4.10 - #gemset created /home/frozenfoxx/.rvm/gems/ruby-2.4.10
ruby-2.4.10 - #importing gemsetfile /home/frozenfoxx/.rvm/gemsets/default.gems evaluated to empty gem list
ruby-2.4.10 - #generating default wrappers - please wait
ruby-2.4.10 - #adjusting #shebangs for (gem irb erb ri rdoc testrb rake).
Install of ruby-2.4.10 - #complete 
Ruby was built without documentation, to build it run: rvm docs generate-ri
```
