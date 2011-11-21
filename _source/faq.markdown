---
layout: main.html
title: FAQ
---

Frequently Asked Questions
--------------------------

### Why can't I use the <kbd>F2</kbd> key to switch back to the splash screen?

To be able to use the <kbd>F2</kbd> key to switch to the splash screen you need
the kernel evdev (Event Device) module (switching from the splash screen to the
first console should work regardless of whether you use this module or not). To
make sure the event device is enabled, build your kernel with
`CONFIG_INPUT_EVDEV=y`. Building `evdev` as a module might or might not work,
depending on how your Linux distribution handles the loading of kernel modules.


### The screen flickers during the boot

This is often due to a distribution-specific initscript, which is modifying the
settings of the system consoles. For instance, on Debian the problem is caused
by `/etc/init.d/console-screen.sh`, and you can easily fix it by commenting out
all lines starting with `SCREEN_FONT` in `/etc/console-tools/config`.


### Can fbsplash be used with KMS (Kernel Mode Setting)?

Absolutely! KMS provides a standard framebuffer device which allows fbsplash to
operate in the usual fashion. In the case of Intel hardware, the framebuffer
driver is called `inteldrmfb`, and is provided by the Intel DRM module, not by
`intelfb`. In fact, you don't even need to have `intelfb` enabled if you're using
KMS. This also means that in case you want to take advantage of the early
silent splash feature (this requires the fbcondecor kernel patch), you need to
have the Intel DRM module built into the kernel.
