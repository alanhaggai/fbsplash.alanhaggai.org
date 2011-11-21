---
layout: main.html
title: Splashutils on Debian
---

Splashutils on Debian
---------------------

### Current Packages Versions

* `splashutils`: latest i386, powerpc and amd64 version is 1.5.4.2-1 ([debian
  changelog](/distros/debian/changelog/splashutils.html))
* `miscsplashutils`: latest, powerpc i386 and amd64 version is 0.1.8-3 ([debian
  changelog](/distros/debian/changelog/miscsplashutils.html))

### Prologue

I would like to cite [spock's weblog](http://mjanusz.wordpress.com/):

    fbsplash = silent splash, fbcondecor = verbose splash.

        * Since the gensplash project was started, everyone referred to the userspace part as “fbsplash” anyway.
        * Many people don’t realize that fbsplash is fully functional and usable without any kernel patch.
        * fbcondecor (which stands for: Framebuffer Console Decorations), which was previously named “fbsplash”,
    has nothing to do with displaying the splash screen during boot. The only thing that it does is displaying
    pictures in the background of system consoles. This is not a splash screen, but a decoration.

**This means that you DO NOT NEED TO HAVE A PATCHED KERNEL to see the progress
bar.** You will need a kernel patched with fbcondecor if you want to have console
decoration.

### Repositories

End-users will need to add this single line in the file `/etc/apt/sources.list`:

    deb ftp://ftp.berlios.de/pub/fbsplash/debian/splashutils sid contrib

See at the end of this document on how you can rebuild the packages.

There are four packages:

    miscsplashutils             Miscellaneous tools for splashutils
    splashutils                 Splashutils main package
    splashutils-dev             Splashutils dev package
    splashutils-initramfs       Splashutils initramfs hook and script

### Installation

#### Required packages

Unless you want to see a background image at very early boot (i.e. almost
immediately after linux kernel has started, and before the userspace's INIT is
taking over), you will only need to install the splashutils package:

    apt-get install splashutils

Obviously you can do that with your preferred package installer, `dselect`,
`aptitude`, `kpackage`, etc… Installation of splashutils will trigger automatically
the installation miscsplashutils. So the minimum number of required packages is
two: splashutils and miscsplashutils.

#### Install a theme

Unless a theme is packaged, you have to install it by hand. You can find themes
on this wiki at the themes page, or on the web, for example at
[http://kde-look.org](http://kde-look.org). Themes always reside entirely under
`/etc/splash` directory, that has been created when you installed the
splashutils package. For example, taking the emergence theme:

    cd /tmp
    wget http://fbsplash.berlios.de/themes/repo/emergence.tar.bz2
    cd /etc/splash
    sudo tar -jxvf /tmp/emergence.tar.bz2

#### Configure the kernel command-line

Take a look at your boot configuration (whereas this is lilo, grub, etc…) and add the followings to the kernel command-line:

    splash=silent,fadein,theme:THEME quiet CONSOLE=/dev/tty1

where you put a real theme name instead of THEME! For example, still with the emergence theme that we previously installed:

    splash=silent,fadein,theme:emergence quiet CONSOLE=/dev/tty1

### Troubleshooting

First, always check the [FAQ](/faq.html) page.

#### Extra Repositories if you Want to Rebuild the Packages

Users that want to rebuild the packages will need the following lines:

    deb-src ftp://ftp.berlios.de/pub/fbsplash/debian/splashutils sid contrib
    deb ftp://ftp.berlios.de/pub/fbsplash/debian/klibc sid contrib

The first line is the source of splashutils, the second line is a dependency to
a fbsplash-aware klibc. The klibc itself can also be rebuilt using:

    deb-src ftp://ftp.berlios.de/pub/fbsplash/debian/klibc sid contrib

### Ackowledgements

Big thanks to [ekl](mailto:eklektist@gmail.com) for providing the powerpc
debian packages.

Here is the HOWTO he wrote when doing it on this architecture:

    REQUIRED (for console decorations): a fbcondecor patched kernel and the related source directory

    splashutils howto:

    1) add deb-src lines to sources.list
        echo "# fbsplash / fbcondecor" >> /etc/apt/sources.list
        echo "deb-src ftp://ftp.berlios.de/pub/fbsplash/debian/klibc sid contrib" >> /etc/apt/sources.list
        echo "deb-src ftp://ftp.berlios.de/pub/fbsplash/debian/splashutils sid contrib" >> /etc/apt/sources.list


    2) if you're not using unstable (sid), set apt preferences and whatever else is needed to manage a mixed system
    (in my system, apt pinning is set for using unstable packages only when the package i'm going to install is not available in testing)
    NOTE: this may or may not be needed, read documentation about managing mixed systems 

    my example:
        echo "Package: *" >> /etc/apt/preferences
        echo "Pin: release a=unstable" >> /etc/apt/preferences
        echo "Pin-Priority: 900" >> /etc/apt/preferences

    3) update apt sources launching
        aptitude update

    4) install dependencies to build klibc
        aptitude install cdbs debhelper bison gcc-4.1

    5) download and build klibc
        apt-get -b source klibc

        *** error ***

    don't worry, this is normal.

    fix:
    edit klibc's debian/control and remove "linux-headers-2.6.25-tuxonice-fbcon" from the dependencies line
    edit klibc's debian/rules and replace all the instances of "/usr/src/linux-headers-2.6.25-tuxonice-fbcon" with the real path to fbcon-patched kernel, usually "/usr/src/linux"

    launch again
        apt-get -b source klibc


    6) install all .deb packages in klibc building directory
    dpkg -i *klibc*.deb


    7) install dependencies to build splashutils
        aptitude install libpng12-dev libjpeg62-dev libfreetype6-dev libmng-dev libgpmg1-dev dpatch
    NOTE: dpatch is not listed in splashutils dependencies, but if you don't install it the build will fail for missing dpatch command.
        JD (mantainer of splashutils for debian - thank him for most of this stuff) has been notified of this little problem and he's going to fix it

    8) build splashutils
        apt-get -b source splashutils


    9) build miscsplashutils
        apt-get -b source miscsplashutils


    10) Install splashutils and miscsplashutils
        dpkg -i *.deb


    11) Download a theme and unpack it in /etc/splash/


    12) there are 2 ways now:
        12a) use only the "default" theme
            link /etc/splash/default to /etc/splash/selected_theme_name
                ln -s /etc/splash/selected_theme_name /etc/splash/default
            rebuild initramfs to add the default theme
                mkinitramfs -o initrd-image-name kernel_version

        12b) add multiple themes to initramfs
            download and unpack all the themes you want in /etc/splash and rebuild initramfs
                mkinitramfs -o initrd-image-name kernel_version
            add all the themes to initramfs:
                splash_geninitramfs -a initrd-image-name --all

    13) edit yaboot.conf and add splash options:
            append="video=<your_fb_driver>:<resolution-depth> quiet splash=silent,fadein,fadeout,theme=<default> console=tty1"

        example (for my iBook G3 clamshell)
            append="video=atyfb:800x600-32 quiet splash=silent,fadein,fadeout,theme=default console=tty1"

    the end.

    if something goes wrong, you can try to mail me at eklektist@gmail.com

    usually i'm very busy, but if you're not hurry you can wait untill i have some free time for a reply

    ekl
