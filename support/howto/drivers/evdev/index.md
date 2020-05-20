---
title: Tablet setup with xf86-input-evdev
summary: How to make your tablet work with the generic input device driver for X.org
weight: 10
---
In this HOWTO the basics of graphics tablet configuration and set up
with the evdev X driver is covered. For many the tablet will work out of
the box and not require anything further. However depending on your
system set up and work flow you may want to make a few tweaks and this
page shows you how to do that.

Determining your tablet OEM
===========================

Because of re-branding it is not always obvious what tablet you actually
have. You can determine your tablet's OEM Vendor ID and Product ID by
entering the following command in a terminal:

    lsusb

and looking for the tablet line in the output.

Aiptek Int.
: Vendor ID = 08ca

KYE Systems
: Vendor ID = 0458

UC-Logic
: Vendor ID = 5543

Waltop
: Vendor ID = 172F

The Product ID (essentially the model \#) immediately follows the Vendor
ID, separated from it by a colon.

- Aiptek tablets use the Aiptek X driver.

X.org configuration
===================

Tablets are often re-branded and will carry the re-brander's name and
model name. To see how your tablet/stylus is identified by the kernel
enter in a terminal the following:

    xinput list

You should see the \<device name\> in the tablet/stylus line. Using the
\<device name\> a keyword can be selected from it for a match in a xorg.conf.d
.conf file.

10-evdev.conf
-------------

The evdev X driver will automatically match a tablet with a HID
compliant kernel driver. This is done with 3 snippets in the
10-evdev.conf located at /usr/share/X11/xorg.conf.d. The 3 snippets that
are relevant:

    Section "InputClass"
           Identifier "evdev pointer catchall"
           MatchIsPointer "on"
           MatchDevicePath "/dev/input/event*"
           Driver "evdev"
    EndSection

    Section "InputClass"
           Identifier "evdev keyboard catchall"
           MatchIsKeyboard "on"
           MatchDevicePath "/dev/input/event*"
           Driver "evdev"
    EndSection

    Section "InputClass"
           Identifier "evdev tablet catchall"
           MatchIsTablet "on"
           MatchDevicePath "/dev/input/event*"
           Driver "evdev"
    EndSection

-   "evdev pointer catchall" matches the tablet mouse
-   "evdev keyboard catchall" matches the frame buttons
-   "evdev tablet catchall" matches the tablet pen

With an evdev X driver of version 2.5.99 or later your tablet should work out
of the box. To determine your version of evdev use your Distro's Software
Manager and search "evdev". This is of course also dependent on which kernel
version provides support for your tablet, see [Tablets](/tablets).

Custom tablet .conf
-------------------

A DIGI*mend* project goal is to have everything on the tablet work
without needing configuration by you. However a custom .conf does allow
the default options to be changed if you feel the need to. You can read
about the available options in *man evdev* entered in a terminal.

Because .conf files at /usr/share/X11/xorg.conf.d are shipped by the
distribution they may be overwritten by an update. For that reason it is
not recommended to put any user-specific configuration files or changes
in /usr/share/X11/xorg.conf.d. Instead user custom .conf files
technically belong in /etc/X11/xorg.conf.d.

For X Servers less than 1.10 to create a custom evdev options .conf or
to place the tablet on the evdev X driver identify an appropriate
keyword from the \<device name\> in the output of *xinput list*. This is
usually the OEM's name, such as "KYE" or "UC-Logic". Then use the key
word in a MatchProduct line in the snippet. Place the snippet in a new
file you'll create in /etc/X11/xorg.conf.d called ***52-tablet.conf***.
The snippets should look similar to the following.

    Section "InputClass"
           Identifier "Graphics tablet mouse"
           MatchIsPointer "on"
           MatchProduct "keyword"
           MatchDevicePath "/dev/input/event*"
           Driver "evdev"
           # Apply custom Options below.
    EndSection

    Section "InputClass"
           Identifier "Graphics tablet pen"
           MatchIsTablet "on"
           MatchProduct "keyword"
           MatchDevicePath "/dev/input/event*"
           Driver "evdev"
           # Apply custom Options below.
    EndSection

-   there are no applicable options for the **"evdev keyboard
    catchall"** snippet so one isn't included
-   you do not need the mouse snippet if a mouse was not included with
    your tablet
-   there is no snippet yet for a scroll wheel because the protocol
    isn't HID-compatible and a driver to create synthetic devices hasn't
    been developed

Since it is a system file you will need root privileges. If your system
has GNOME and sudo installed you can use:

    gksudo gedit /etc/X11/xorg.conf.d/52-tablet.conf

You would use whatever text editor and root privilege access your distribution
supplies of course.  After a reboot your tablet should now be applying any
options you've added using the evdev X driver, provided the match was
successful. This is because whatever runs last in xorg.conf.d controls and by
using the 52 as the file's number it should run after any other .conf file
likely to be matching the tablet. Additionally the xorg.conf.d in /etc/X11
runs after the one at /usr/share/X11.

For X Server version 1.10 or later the preferred option snippets in
/etc/X11/xorg.conf.d, provided your tablet is already on the evdev X
driver, are:

    Section "InputClass"
           Identifier "Graphics tablet mouse"
           MatchDriver "evdev"
           MatchIsPointer "on"
           MatchProduct "keyword"
           MatchDevicePath "/dev/input/event*"
           # Apply custom Options below.
    EndSection

    Section "InputClass"
           Identifier "Graphics tablet pen"
           MatchDriver "evdev"
           MatchIsTablet "on"
           MatchProduct "keyword"
           MatchDevicePath "/dev/input/event*"
           # Apply custom Options below.
    EndSection

-   there are no applicable options for the **"evdev keyboard
    catchall"** snippet so one isn't included
-   you do not need the mouse snippet if a mouse was not included with
    your tablet

To determine both your kernel and X Server versions run the command "Xorg
-version".

And since the tablet is already matched to the driver you might want to
use a different keyword from the "device name" such as *EasyPen* for the
match. The keyword must be unique to the specific tablet and not used by
another device on the evdev driver. This permits you to hot plug two
different tablets with different options by making two unique snippets.

Further tablet set up information
=================================

To view the tablet properties potentially available for modification use
the \<device name\> in quotes from *xinput list* in the following
terminal command:

    xinput list-props "device name"

For further information see the evdev and xinput manuals; *man evdev* and *man
xinput* entered in a terminal.

Calibration
-----------

Calibration can be necessary when the pointer on the screen does not
coincide with where the stylus tool is located on the tablet. This
usually made obvious when the pointer does not reach a monitor's edge or
edges. To fix this you can use a calibration tool such as
**xinput\_calibrator**. It may be in your Distro's repositories as
xinput-calibrator. Follow the instructions and run the calibration
routine being sure you are in your normal working postition and holding
the stylus as you normally do. Use the min-x max-x min-y max-y
coordinates generated in the following terminal command:

    xinput set-prop "device name" "Evdev Axis Calibration" min-x max-x min-y max-y

This is a run-time command and is lost with a hot-plug,
log-off, or re-start. You can add it to a custom xorg.conf.d (static)
configuration file as an option:

    Section "InputClass"
           Identifier "Custom evdev tablet options"
           MatchIsTablet "on"
           MatchProduct "keyword"
           MatchDevicePath "/dev/input/event*"
           Driver "evdev"
           # Apply custom Options below.
           Option "Calibration" "min-x max-x min-y max-y"
    EndSection

This will survive a hot-plug.

### Tablet proportions

You can also use the Calibration xinput command or static Option setting
to correct an problematic screen aspect ratio (tablet to monitor),
especially with a wide screen monitor. Determine the proportion of the
tablet equivalent to the screen dimensions and use Calibration to set
the tablet to the corresponding area in order to get a one to one ratio.
You will lose some of the active area of the tablet doing this.

Dual and multiMonitor set up
----------------------------

If you have two or more monitors and want to confine the stylus to one
screen you can use the Coordinate Transformation Matrix to accomplish
that. For example with two monitors of the same size side by side you
would use:

***Left monitor***

    xinput set-prop "device name" --type=float "Coordinate Transformation Matrix" 0.5 0 0 0 1 0 0 0 1

***Right monitor***

    xinput set-prop "device name" --type=float "Coordinate Transformation Matrix" 0.5 0 0.5 0 1 0 0 0 1

For more information see the [Coordinate Transformation
Matrix](http://linuxwacom.sourceforge.net/wiki/index.php/Dual_and_Multi-Monitor_Set_Up#Coordinate_Transformation_Matrix)
on the Linux Wacom Project's mediawiki.

***xrestrict Utility***

The [`xrestrict`](https://github.com/Ademan/xrestrict) utility is in early
development, and may contain bugs. It automatically calculates the
appropriate "Coordinate Transformation Matrix" for a given monitor and device,
and applies it. Its [github
page](https://github.com/ademan/xrestrict#building) includes instructions for
building from source.  Its basic invocation is `xrestrict -d DEVICEID -c
CRTCINDEX`. `DEVICEID` corresponds to the XID of the tablet device.
`CRTCINDEX` is the index of the CRTC to use. (For most multi-monitor setups
without mirrored monitors, a CRTC generally corresponds directly to monitors.
So if you have two monitors, you will have two CRTCs: 0 and 1.) You can use
the `xinput` command to discover the XID of your tablet. For instance this is
what my xinput looks like

    $ xinput
    ⎡ Virtual core pointer                    	id=2	[master pointer  (3)]
    ⎜   ↳ Virtual core XTEST pointer              	id=4	[slave  pointer  (2)]
    ⎜   ...
    ⎜   ↳ HV 10594                                	id=15	[slave  pointer  (2)]
    ⎣ Virtual core keyboard                   	id=3	[master keyboard (2)]
        ↳ Virtual core XTEST keyboard             	id=5	[slave  keyboard (3)]
        ...
        
My tablet is named "HV 10594" so in this case 15 is the correct `DEVICEID`.
On my system, CRTC 0 corresponds to my left monitor, but this numbering is
arbitrary, you will have to experiment to see what number your monitors are.
Given these things, if I want to restrict my tablet to the left screen I issue
`xrestrict -d 15 -c 0` and xrestrict does the rest for me.

Left handed tablet orientation
------------------------------

If you are left handed you may want to flip the tablet.

    xinput set-prop "device name" "Coordinate Transformation Matrix" -1, 0, 1, 0, -1, 1, 0, 0, 1

Or you could use:

    xinput set-prop "device name" "Evdev Axis Inversion" 1

And unlike the Coordinate Transformation Matrix you can use Inversion as
paired options in your custom xorg.conf.d .conf file:

    Option "InvertX" "on"
    Option "InvertY" "on"

### Left handed tablet orientation with dual monitor set up

If you are left handed and have a dual monitor set up you may want to flip
the tablet and confine the stylus to one screen.

***Left monitor***

    xinput set-prop "device name" --type=float "Coordinate Transformation Matrix" -0.5 0 0.5 0 -1 1 0 0 1

***Right monitor***

    xinput set-prop "device name" --type=float "Coordinate Transformation Matrix" -0.5 0 1 0 -1 1 0 0 1

Runtime script
--------------

If you plan on using one or more xinput commands you will likely want to
place them in a script due to the fact that they are runtime commands.
While you can change your settings on the fly using the xinput commands
in a terminal, runtime commands only apply during the current session.
To create a script file open a text editor and enter the commands you
want to use into it, each command on its own line. The script should
start with a shebang line followed by a blank line and then the
commands. The shebang line specifies which shell interpreter to use.

    #!/bin/bash

or

    #!/bin/sh

Name the text/script file ***xinput.sh*** (or whatever you prefer).

Place the script in a folder called bin in your home directory. You may
need to create the bin directory in /home/yourusername first. Many
distributions recommend placing user binaries and scripts in a user /bin
directory rather than just in /home/yourusername. Make the script
executable by right clicking on it and choosing Properties \>
Permissions tab and checking the "Make executable" box or with:

    chmod +x $HOME/bin/xinput.sh

You could place a . in front of xinput.sh to make it a hidden file to prevent
directory clutter, but that isn't needed if you place it the
/home/yourusername/bin directory.  After a hot plug you can re-run the script
so the settings are re-applied.

### Running the script at startup

You will likely want to set the script to auto-start so the settings are
applied automatically. In GNOME go to System \> Preferences \> Startup
Applications (or your distribution's equivalent) and click on Add and
for the command write "sh /home/yourusername/bin/xinput.sh" (without the
quotes).

Stylus button assignment
------------------------

Many prefer to reverse middle click (2) and right click (3) on their
stylus button assignments if they have two stylus side buttons. Middle
click is often the "grab" button in graphics programs such as Gimp and
Inkscape. The stylus tip is by default set to button 1 (left click) and
that assignment shouldn't be changed. Again you have the choice of a
static Option or a runtime xinput command.

    Option "ButtonMapping" "1 3 2"

    xinput set-button-map "device name" 1 3 2

- Buttons 4,5,6,7 are reserved by X for vertical and horizontal scroll.
