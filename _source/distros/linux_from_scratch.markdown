---
layout: main.html
title: Splashutils on Linux from Scratch
---

Splashutils on Linux from Scratch
---------------------------------

The following is to explain what I had to do to get Splashutils compiled on my Linux From Scratch installation:

### Overview:

* You will need to install KLIBC
* If GPM is not already installed you will need to install this.
* Compile and install Splashutils
* Patch kernel 2.6.24
* Configure GRUB

### Details:

#### KLIBC

1. Download klibc:

        wget ftp://ftp.berlios.de/pub/fbsplash/debian/klibc/pool/contrib/k/klibc/klibc_1.5.7.orig.tar.gz

2. Extract this:

        tar zxvf klibc_1.5.7.orig.tar.gz

3. In the klibc-1.5.7 directory create a symbolic link to your Linux headers directory:

        ln -s /usr/src/linux linux

4. Compile the klibc: `make; make install`

        If you get any errors then check linux source directory.
        Else download the Kernel source(http://www.kernel.org/pub/linux/kernel/v2.6/linux-2.6.24.tar.bz2 )
        Extract the kernel: tar jxvf linux-2.6.24.tar.bz2
        In linux-2.6.24 build the API headers:
            make mrproper
            make headers_check
            make INSTALL_HDR_PATH=dest headers_install
        In the klibc directory create a symbolic link to the linux-2.6.24/dest directory: 
        ln -s ../linux-2.6.24/dest linux
        Try and make again: make; make install

5. You should now have klcc at `/usr/bin/klcc` and `libs` for klibc in `/usr/lib/klibc`

6. Check that `/usr/lib/klibc/include/asm` has `posix_types.h` file, if it does
not then copy all files from `/usr/src/linux/include/asm` into this directory:

        cp /usr/src/linux/include/asm/* /usr/lib/klibc/include/asm

#### GPM

7. Use the LFS guide for GPM:

    [http://www.linuxfromscratch.org/blfs/view/stable/general/sysutils.html](http://www.linuxfromscratch.org/blfs/view/stable/general/sysutils.html)

NOTE: I can't see a reason why you would need to install GPM to get fbsplash to work. It works on my system without GPM. - Andre

#### SPLASHUTILS

8. Download the latest version of splashutils:

        wget http://download.berlios.de/fbsplash/splashutils-1.5.4.tar.bz2

9. Extract this:

        tar jxvf splashutils-1.5.4.tar.bz2

10. In the splashutils-1.5.4 directory run configure:

        ./configure

11. Compile splashutils:

         make; make install

#### PATCH THE KERNEL

12. If you are not already using Kernel 2.4.26 then download it:

        wget http://www.eu.kernel.org/pub/linux/kernel/v2.6/linux-2.6.24.tar.bz2

13. Extract the kernel source

        tar jxvf linux-2.6.24.tar.bz2

14. Download the fbcondecor patch

        wget http://dev.gentoo.org/~dsd/genpatches/trunk/2.6.24/4200_fbcondecor-0.9.4.patch

15. In the Kernel source directory(linux-2.6.24) patch the source

        patch -Np1 -i ../4200_fbcondecor-0.9.4.patch

16. Ensure that the kernel source is clean

        make mrproper

17. At this stage you should configure the kernel with all your required settings. I cheat by copying my previous kernel version config file to the `linux-2.6.24/.config`

        make mrproper

18. Ensure that the following is set in the kernel:

    `Device Drivers->Graphics Support->Support` for frame buffer devices, `Device drivers->Graphics support->Console display driver support->Framebuffer Console support`, `Device drivers->Graphics support->Console Display driver support->Support for the Framebuffer Console Decorations`.

19. Make the kernel:

        make

20. Copy the bzImage and config files to your boot directory

        cp -v arch/i386/boot/bzImage /boot/kernel-2.6.24
        cp -v System.map /boot/System.map-2.6.24
        cp -v .config /boot/config-2.6.24

#### Build an initramfs:

21. Create an /etc/splash directory

        mkdir /etc/splash

22. Download into this directory a theme

        wget http://fbsplash.berlios.de/themes/repo/emergence.tar.bz2

23. Extract this theme

        tar jxvf emergence.tar.bz2

24. Create an initramfs with this theme

        splash_geninitramfs -g /boot/initrd-fbsplash -r 1024x768 -v emergence

#### Configure GRUB:

25. Create a new entry in /boot/grub/menu.lst.

        title fbsplash
        kernel /boot/kernel-2.6.24 root=/dev/sda1 vga=791 splash=verbose,fadein,theme:emergence quiet CONSOLE=tty1
        initrd /boot/initrd-fbsplash

    Note: At this point in time I can not get silent splash to work. This must be something to do with the way LFS boot_mesg works.

    EDIT: This does not come as a surprise. You need to add `quiet CONSOLE=tty1` to your boot parameters (I edited the grub config shown above). - Andre

I hope this helps

Jason
