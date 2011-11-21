---
layout: main.html
title: Splashutils on Gentoo
---

Splashutils on Gentoo
---------------------

### Installing Splashutils

First, decide which features you want to enable. Available USE flags are:

* png: support for PNG pictures
* mng: support for MNG animations
* truetype: support for TrueType fonts
* fbcondecor: support for the FBConDecor kernel patch

To enable a specific USE flag for splashutils, add an appropriate line to the
`packages.use` file, e.g. to enable `fbcondecor`, execute `echo
"media-gfx/splashutils fbcondecor" » /etc/portage/packages.use`

After you've decided on the features, install splashutils: `emerge splashutils`.

The splashutils package doesn't contain any themes, so you'll probably want to
install some to test your fbsplash installation. A good theme to try is the
Gentoo LiveCD splash theme. To install it, run: `emerge splash-themes-livecd`.

### Configuring Splashutils

To enable fbsplash, you have to edit the config file of your bootloader. Find
the kernel command line for your current kernel and extend it with:
`splash=silent,theme:SampleTheme quiet console=tty1` (replace SampleTheme with a
theme you wish to use).

If you're using a kernel patched with fbcondecor (`gentoo-sources` is always
patched automatically), you might want to create an initramfs image that the
kernel will be able to use display the splash screen at an early stage of the
boot process. To create this image, use `splash_geninitramfs -g
/boot/fbsplash-initrd -r 1024×768 -v SampleTheme` (replace 1024×786 with your
framebuffer resolution, use `fbset -i` if you don't know what it is) and then
configure your bootloader to use `/boot/fbsplash-initrd` as an initrd.

### More Info

Unofficial documentation about fbsplash on Gentoo is available in the [Gentoo
Wiki](http://gentoo-wiki.com/HOWTO_gensplash).
