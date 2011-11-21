---
layout: main.html
title: Fbsplash Internals
---

Fbsplash Internals
------------------

### Starting Fbsplash

There are three main points during the boot process when it makes sense to
start fbsplash. Not surprisingly, the one that takes the most effort to set up
also yields the best results (in terms of how early the silent splash image is
displayed). The three ways of starting fbsplash, in order of increasing
configuration difficulty are the following:

1. Start the fbsplash daemon using the initscript system (**start-from-rc**)
2. Start fbsplash from an initrd after the kernel is fully loaded, but before
the initscript system kicks in (**start-from-initrd**)
3. Start fbsplash from an initramfs image right after the framebuffer is
activated, during kernel bootup (**start-from-kernel**)

#### start-from-rc

Starting fbsplash daemon from the initscript system is relatively easy. It
doesn't require touching the kernel in any way. The two steps necessary to get
it working are usually:

* installing the core fbsplash utilities and scripts, patching the
* initscript system or installing appropriate plugins/addons that add 
  support for fbsplash.

Note that these two steps are also necessary for the other ways of starting
fbsplash (start-from-initrd, start-from-kernel). Thus, start-from-rc is a good
first step when porting fbsplash to a previously unsupported distro.

###### When is fbsplash started?

In start-from-rc, the fbsplash daemon (fbsplashd) is normally started before
all initscripts, but after the initial system setup is done (/proc and /sys are
mounted, udev started and /dev is fully populated). The details might differ
from distro to distro, but it is essential to start the daemon as early as
possible to ensure the best user experience.

##### Advantages 

* easy to setup, no kernel modifications required
* necessary to get fbsplash working anyway

##### Disadvantages

* some kernel messages or messages from the early system setup might be
  visible on the screen before the silent splash screen is brought up
* it might take up to 10-15 sec before the splash is screen is displayed,
  depending on system configuration

#### start-from-initrd

###### When is fbsplash started?

In start-from-initrd, fbsplash is started after the kernel has finished
booting, but before `/sbin/init` from the live system is executed. The splash
screen will thus appear several seconds faster than in the case of
start-from-rc.

##### Advantages

* no kernel modifications required
* no messages coming from the initscript system will be visible on the screen

##### Disadvantages

* requires integration with whatever initrd setup your distro is using

#### start-from-kernel

![](/img/fixme.gif)
