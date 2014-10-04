---
title: Tablet setup with xf86-input-evdev
summary: How to make your tablet work with the generic input device driver for X.org
---
In this HOWTO the basics of graphics tablet configuration and set up
with the evdev X driver is covered. For many the tablet will work out of
the box and not require anything further. However depending on your
system set up and work flow you may want to make a few tweaks and this
page shows you how to do that. If your tablet isn't supported or you are
having problems proceed to
[Collecting\_tablet\_diagnostics](Collecting_tablet_diagnostics "wikilink")
and contact
[spbnick@gmail.com](mailto:spbnick@gmail.com?subject=Tablet%20issue).

Determining your tablet OEM
===========================

Because of re-branding it is not always obvious what tablet you actually
have. You can determine your tablet's OEM Vendor ID and Product ID by
entering the following command in a terminal: \<pre\> lsusb \</pre\> and
looking for the tablet line in the output.

:   **Aiptek Int.**
    :   Vendor ID = 08ca

:   **KYE Systems**
    :   Vendor ID = 0458

:   **UC-Logic**
    :   Vendor ID = 5543

:   **Waltop**
    :   Vendor ID = 172F

The Product ID (essentially the model \#) immediately follows the Vendor
ID, separated from it by a colon.

-   Aiptek tablets use the Aiptek X driver.

X.org configuration
===================

Tablets are often re-branded and will carry the re-brander's name and
model name. To see how your tablet/stylus is identified by the kernel
enter in a terminal the following: \<pre\> xinput list \</pre\> You
should see the \<device name\> in the tablet/stylus line. Using the
\<device name\> a keyword can be selected from it for a match in a
xorg.conf.d .conf file.

10-evdev.conf
-------------

The evdev X driver will automatically match a tablet with a HID
compliant kernel driver. This is done with 3 snippets in the
10-evdev.conf located at /usr/share/X11/xorg.conf.d. The 3 snippets that
are relevant: \<pre\> Section "InputClass"

`       Identifier "evdev pointer catchall"`\
`       MatchIsPointer "on"`\
`       MatchDevicePath "/dev/input/event*"`\
`       Driver "evdev"`

EndSection

Section "InputClass"

`       Identifier "evdev keyboard catchall"`\
`       MatchIsKeyboard "on"`\
`       MatchDevicePath "/dev/input/event*"`\
`       Driver "evdev"`

EndSection

Section "InputClass"

`       Identifier "evdev tablet catchall"`\
`       MatchIsTablet "on"`\
`       MatchDevicePath "/dev/input/event*"`\
`       Driver "evdev"`

EndSection \</pre\>

-   **"evdev pointer catchall"** matches the tablet mouse
-   **"evdev keyboard catchall"** matches the frame buttons
-   **"evdev tablet catchall"** matches the tablet pen

With an evdev X driver of version 2.5.99 or later your tablet should
work out of the box. This is of course also dependent on which kernel
version provides support for your tablet, see [Tablet support
status](Tablet support status "wikilink").

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
The snippets should look similar to the following. \<pre\> Section
"InputClass"

`       Identifier "Graphics tablet mouse"`\
`       MatchIsPointer "on"`\
`       MatchProduct "keyword"`\
`       MatchDevicePath "/dev/input/event*"`\
`       Driver "evdev"`\
`       # Apply custom Options below.`

EndSection

Section "InputClass"

`       Identifier "Graphics tablet pen"`\
`       MatchIsTablet "on"`\
`       MatchProduct "keyword"`\
`       MatchDevicePath "/dev/input/event*"`\
`       Driver "evdev"`\
`       # Apply custom Options below.`

EndSection \</pre\>

-   there are no applicable options for the **"evdev keyboard
    catchall"** snippet so one isn't included
-   you do not need the mouse snippet if a mouse was not included with
    your tablet
-   there is no snippet yet for a scroll wheel because the protocol
    isn't HID-compatible and a driver to create synthetic devices hasn't
    been developed

Since it is a system file you will need root privileges. If your system
has GNOME and sudo installed you can use: \<pre\> gksudo gedit
/etc/X11/xorg.conf.d/52-tablet.conf \</pre\> You would use whatever text
editor and root privilege access your distribution supplies of course.
After a reboot your tablet should now be applying any options you've
added using the evdev X driver, provided the match was successful. This
is because whatever runs last in xorg.conf.d controls and by using the
52 as the file's number it should run after any other .conf file likely
to be matching the tablet. Additionally the xorg.conf.d in /etc/X11 runs
after the one at /usr/share/X11.

For X Server version 1.10 or later the preferred option snippets in
/etc/X11/xorg.conf.d, provided your tablet is already on the evdev X
driver, are: \<pre\> Section "InputClass"

`       Identifier "Graphics tablet mouse"`\
`       MatchDriver "evdev"`\
`       MatchIsPointer "on"`\
`       MatchProduct "keyword"`\
`       MatchDevicePath "/dev/input/event*"`\
`       # Apply custom Options below.`

EndSection

Section "InputClass"

`       Identifier "Graphics tablet pen"`\
`       MatchDriver "evdev"`\
`       MatchIsTablet "on"`\
`       MatchProduct "keyword"`\
`       MatchDevicePath "/dev/input/event*"`\
`       # Apply custom Options below.`

EndSection \</pre\>

-   there are no applicable options for the **"evdev keyboard
    catchall"** snippet so one isn't included
-   you do not need the mouse snippet if a mouse was not included with
    your tablet

And since the tablet is already matched to the driver you might want to
use a different keyword from the "device name" such as *EasyPen* for the
match. The keyword must be unique to the specific tablet and not used by
another device on the evdev driver. This permits you to hot plug two
different tablets with different options by making two unique snippets.

Further tablet set up information
=================================

To view the tablet properties potentially available for modification use
the \<device name\> in quotes from *xinput list* in the following
terminal command: \<pre\> xinput list-props "device name" \</pre\> For
further information see the evdev and xinput manuals; *man evdev* and
*man xinput* entered in a terminal.

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
coordinates generated in the following terminal command: \<pre\> xinput
set-prop "device name" "Evdev Axis Calibration" min-x max-x min-y max-y
\</pre\> This is a run-time command and is lost with a hot-plug,
log-off, or re-start. You can add it to a custom xorg.conf.d (static)
configuration file as an option: \<pre\> Section "InputClass"

`       Identifier "Custom evdev tablet options"`\
`       MatchIsTablet "on"`\
`       MatchProduct "keyword"`\
`       MatchDevicePath "/dev/input/event*"`\
`       Driver "evdev"`\
`       # Apply custom Options below.`\
`       Option "Calibration" "min-x max-x min-y max-y"`

EndSection \</pre\> This will survive a hot-plug.

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

***Left monitor*** \<pre\> xinput set-prop "device name" --type=float
"Coordinate Transformation Matrix" 0.5 0 0 0 1 0 0 0 1 \</pre\> ***Right
monitor*** \<pre\> xinput set-prop "device name" --type=float
"Coordinate Transformation Matrix" 0.5 0 0.5 0 1 0 0 0 1 \</pre\> For
more information see the [Coordinate Transformation
Matrix](http://linuxwacom.sourceforge.net/wiki/index.php/Dual_and_Multi-Monitor_Set_Up#Coordinate_Transformation_Matrix)
on the Linux Wacom Project's mediawiki.

Left handed tablet orientation
------------------------------

If you are left handed you may want to flip the tablet. \<pre\> xinput
set-prop "device name" "Coordinate Transformation Matrix" -1, 0, 1, 0,
-1, 1, 0, 0, 1 \</pre\> Or you could use: \<pre\> xinput set-prop
"device name" "Evdev Axis Inversion" 1 \</pre\> And unlike the
Coordinate Transformation Matrix you can use Inversion as paired options
in your custom xorg.conf.d .conf file: \<pre\> Option "InvertX" "on"
Option "InvertY" "on" \</pre\>

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
\<pre\>

1.  !/bin/bash

\</pre\> or \<pre\>

1.  !/bin/sh

\</pre\> Name the text/script file ***xinput.sh*** (or whatever you
prefer).

Place the script in a folder called bin in your home directory. You may
need to create the bin directory in /home/yourusername first. Many
distributions recommend placing user binaries and scripts in a user /bin
directory rather than just in /home/yourusername. Make the script
executable by right clicking on it and choosing Properties \>
Permissions tab and checking the "Make executable" box or with: \<pre\>
chmod +x \$HOME/bin/xinput.sh \</pre\> You could place a . in front of
xinput.sh to make it a hidden file to prevent directory clutter, but
that isn't needed if you place it the /home/yourusername/bin directory.
After a hot plug you can re-run the script so the settings are
re-applied.

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
static Option or a runtime xinput command. \<pre\> Option
"ButtonMapping" "1 3 2" \</pre\> \<pre\> xinput set-button-map "device
name" 1 3 2 \</pre\>

-   Buttons 4,5,6,7 are reserved by X for vertical and horizontal
    scroll.

