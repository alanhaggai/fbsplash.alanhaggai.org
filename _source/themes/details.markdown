---
layout: main.html
title: Fbsplash Themes
---

Fbsplash Themes
---------------

Fbsplash themes are formed by a set of files and directories, placed in a
specific location in the main filesystem (usually `/etc/splash`). Each theme is
entirely contained in a single directory, e.g. `/etc/splash/linux`. The
directory name should not contain spaces.

The theme directory contains a theme description file called `metadata.xml`
(see fbsplash `metadata.xml` example) and at least one config file. The config
files are named `<res>.cfg`, where `<res>` is a framebuffer resolution for
which the given config file was written (e.g. 800×600, 1280×800). It is
advisable to provide at least one config file for a low resolution such as
640×480. In the case the current resolution is not supported by a theme, the
fbsplash daemon can use a config file for a lower resolution. Having one for
640×480 provides a safe fallback. The config files can contain a number of
different directives, which are documented in the the `theme_format` document
distributed in the splashutils package.

### The Event System

Fbsplash themes can take advantage of an event system. This makes it possible
to make the themes more dynamic by allowing them to react to what happens
during boot. To be notified about an event, the theme should provide an event
handler in the form of an executable script located at
`scripts/<event>-(post|pre)`. `pre` and `post` specify whether the handler should be
executed respectively before or after the event is handled by the fbsplash
framework. For example, if the linux theme would like to be notified about the
`rc_init` event, before it is handled by fbsplash, it should provide the handler
in `/etc/splash/linux/scripts/rc_init-pre`.

When an event takes place, the event handler is executed with a set of command
line arguments (specified below). The current version of fbsplash supports the
following events:

<table>
    <tr>
        <th>Event Name</th>
        <th>Arguments</th>
        <th>Description<th>
    </tr>
    <tr>
        <td>rc_init</td>
        <td>internal_runlevel sysvinit_runlevel</td>
        <td>start of a new runlevel</td>
    </tr>
    <tr>
        <td>rc_exit</td>
        <td>internal_runlevel sysvinit_runlevel</td>
        <td>end of the current runlevel</td>
    </tr>
    <tr>
        <td>svc_start</td>
        <td>service_name</td>
        <td>a service is starting</td>
    </tr>
    <tr>
        <td>svc_started</td>
        <td>service_name</td>
        <td>a service has started sucessfully</td>
    </tr>
    <tr>
        <td>svc_start_failed</td>
        <td>service_name</td>
        <td>a service has failed to start</td>
    </tr>
    <tr>
        <td>svc_stop</td>
        <td>service_name</td>
        <td>a service is stopping</td>
    </tr>
    <tr>
        <td>svc_stopped</td>
        <td>service_name</td>
        <td>a service has stopped</td>
    </tr>
    <tr>
        <td>svc_stop_failed</td>
        <td>service_name</td>
        <td>a service has failed to stop</td>
    </tr>
    <tr>
        <td>svc_input_begin</td>
        <td>service_name</td>
        <td>a service is requesting user input</td>
    </tr>
    <tr>
        <td>svc_input_end</td>
        <td>service_name</td>
        <td>a service has finished receiving user input</td>
    </tr>
    <tr>
        <td>critical</td>
        <td></td>
        <td>a critical condition has occurred and the screen has to be switched to verbose mode</td>
    </tr>
</table>

A handler script can expect the following about its environment:

`SPLASH_XRES` and `SPLASH_YRES` environment variables are defined and together
they specify which config file is currently being used by fbsplash. For
instance, if the fbsplash daemon is using 640×480.cfg, `SPLASH_XRES` would be
set to 640 and `SPLASH_YRES` to 480.  `splash-functions.sh` has been sourced
and all its functions are available.  `splash_setup` from `splash-functions.sh`
has been executed and all standard splash environment variables are defined.
