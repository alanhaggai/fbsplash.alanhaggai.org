---
layout: main.html
title: Splashutils Overview
---

Splashutils Overview
--------------------

Splashutils is a collection of fbsplash-related utilities and scripts.

### Standard Programs

* `fbsplashd` – the fbsplash daemon, responsible for displaying the splash
    screen during boot
* `splash_util` – a simple utility for various splash-related tasks not
    suitable for fbsplashd
* `splash_manager` – a wrapper script useful for testing themes, taking
    screenshots, etc.
* `splash_geninitramfs` – a helper script to include fbcondecor_helper in
    initramfs images

### Optional Programs

splashutils has optional support for the fbcondecor kernel patch. If compiled
with this functionality enabled, the following two additional programs will be
installed:

* `fbcondecor_ctl` – a control application for the fbcondecor kernel patch
* `fbcondecor_helper` – a kernel helper for the fbcondecor kernel patch

### Libraries

* `libfbsplash.so` – a library to facilitate the integration of fbsplash with the
    initscript system
* `libfbsplashrender.so` – a library to facilitate supporting fbsplash theme
    files in different applications (such as userspace parts of
Linux hibernation systems)
