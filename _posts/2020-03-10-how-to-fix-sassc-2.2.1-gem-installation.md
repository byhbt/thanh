---
layout: post
title: "How to fix sassc-2.2.1 Gem installtion"
categories:
  - Ruby
tags:
  - gem
  - bundle
---

When I tried to run **```bundle```** then I got this issue.

The message is clear that **```make: g++: Command not found```**.

```
current directory: /home/mypc/.rvm/gems/ruby-2.6.3/gems/sassc-2.2.1/ext
make "DESTDIR="
compiling ./libsass/src/cssize.cpp
make: g++: Command not found
Makefile:235: recipe for target 'cssize.o' failed
make: *** [cssize.o] Error 127

make failed, exit code 2

Gem files will remain installed in /home/mypc/.rvm/gems/ruby-2.6.3/gems/sassc-2.2.1 for inspection.
Results logged to /home/mypc/.rvm/gems/ruby-2.6.3/extensions/x86_64-linux/2.6.0/sassc-2.2.1/gem_make.out
```


So we need to install the **```build-essential```**

But when try to install **```build-essential```** I have this issue:


```
sudo apt-get install build-essential
Reading package lists... Done
Building dependency tree
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 build-essential : Depends: dpkg-dev (>= 1.17.11) but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
```

Try to install **```dpkg-dev```**, then got this issue:

```
sudo apt-get install dpkg-dev
Reading package lists... Done
Building dependency tree
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 dpkg-dev : Depends: libdpkg-perl (= 1.19.0.5ubuntu2) but 1.19.0.5ubuntu2.1 is to be installed
            Recommends: build-essential but it is not going to be installed
            Recommends: fakeroot
            Recommends: libalgorithm-merge-perl but it is not going to be installed
E: Unable to correct problems, you have held broken packages.

```

Here is the root cause, resolve it by remove **```dpkg-dev```**,
```
sudo apt-get remove libdpkg-perl
```

Then reinstall the **```build-essential```**

```
sudo apt-get install build-essential
```

And now, I can run bundle again.

```
bundle
```

Done. 2h to solved :((
