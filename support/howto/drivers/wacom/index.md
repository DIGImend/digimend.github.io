---
title: Tablet setup with xf86-input-wacom
---
It is intended that Waltop tablets be supported by the Wacom X driver.
However things have been in a state of flux both in the kernel and with
the Wacom and evdev X drivers. It is possible that your Waltop tablet
might be better supported by the evdev driver.

The advantage the Wacom driver offers is stylus pressure on a [Bezier
curve](http://en.wikipedia.org/wiki/B%C3%A9zier_curve) rather than the
simple linear curve of the evdev driver. This enables setting the stylus
to firmer or softer feel. See the [graphical javascript
demo](http://linuxwacom.sourceforge.net/misc/bezier.html) for viewing
the results of manipulating the two control points.

In addition, the Wacom driver has a pressure threshold setting, while
the evdev driver currently lacks it. With evdev, minimum pressure is
always set to 0 and there is no option to vary that. If the stylus is
too sensitive you can decrease the pressure sensitivity with the
"Threshold" parameter.

A third benefit is the availability of the MapToOutput parameter if you
are using your Waltop tablet in a multi-monitor setup.

Determining if you have a Waltop tablet
=======================================

Because of re-branding it is not always obvious what tablet you actually
have. You can determine your tablet's Vendor ID and Product ID by
entering the following command in a terminal: \<pre\> lsusb \</pre\> and
looking for the tablet line in the output. A Waltop tablet has the
following Vendor ID:

:   **Waltop Vendor ID = 172F (0x172F)**

The Product ID immediately follows the Vendor ID.

X.org configuration
===================

If you have at least the 2.6.35 kernel, particularly the more recent
versions, you should be able to get your tablet working on the Wacom X
driver. Waltop tablets are often re-branded and will carry another OEM's
model name. To verify that your tablet/stylus is indeed identified as a
WALTOP (which is also used as the keyword for a match) enter in a
terminal the following. \<pre\> xinput list \</pre\> You should see
WALTOP (capitalized) as part of the \<device name\> in the tablet/stylus
line. WALTOP is used as the keyword match by the 50-wacom.conf and the
custom 52-waltop.conf. If you see another tablet name in the line you do
not have a Waltop.

50-wacom.conf
-------------

The 50-wacom.conf is located at /usr/share/X11/xorg.conf.d. If you do
not know your xf86-input-wacom version you can determine it by running
the following command: \<pre\> xsetwacom -V \</pre\>

### xf86-input-wacom-0.15.0 or later

In a current
[xf86-input-wacom](https://sourceforge.net/projects/linuxwacom/files/xf86-input-wacom/)
50-wacom.conf file there is a separate Waltop snippet: \<pre\>

1.  Waltop tablets

Section "InputClass"

`   Identifier "Waltop class"`\
`   MatchProduct "WALTOP"`\
`   MatchIsTablet "on"`\
`   MatchDevicePath "/dev/input/event*"`\
`   Driver "wacom"`

EndSection \</pre\> This snippet only matches the Waltop event device
classified as a tablet (i.e. the digitizer/pen). That allows the
non-digitizer devices on separate event nodes (/dev/input) to be handled
by xf86-input-evdev. Those include the on-the-frame controls such as the
pad buttons, multifunction dials (controlling scroll, zoom, volume, and
keyboard navigation), and keyboard modifier buttons. The
xf86-input-wacom driver does not work with them.

### xf86-input-wacom-0.12.0 to 0.14.0

With the 2.6.35 kernel Waltop tablet users reported the Waltops were
again working with xf86-input-wacom. Because they were again working the
WALTOP keyword was added back into the 50-wacom.conf USB snippet match
with the release of xf86-input-wacom-0.12.0. \<pre\> Section
"InputClass"

`   Identifier "Wacom class"`\
`   MatchProduct "Wacom|WACOM|WALTOP|Hanwang|PTK-540WL"`\
`   MatchDevicePath "/dev/input/event*"`\
`   Driver "wacom"`

EndSection \</pre\> Which means the Waltop tablets are by default
automatically matched to the Wacom X driver.

Prior to xf86-input-wacom-0.12.0 Waltop tablets were commented out from
the USB snippet match in the 50-wacom.conf because the HID usb kernel
driver for Waltops did not support xf86-input-wacom. Waltop tablets had
worked with the linuxwacom X driver before the xf86-input-wacom fork
from linuxwacom.

52-waltop.conf
--------------

Because .conf files at /usr/share/X11/xorg.conf.d are shipped by the
distribution they may be overwritten by an update. For that reason it is
not recommended to put any user-specific configuration files or changes
in /usr/share/X11/xorg.conf.d. Instead user custom .conf files
technically belong in /etc/X11/xorg.conf.d.

The 52-waltop.conf enables multifunction dials on evdev if your tablet
has them. It also provides you with a way to apply static xorg.conf.d
Wacom Options to your pen. Provided the kernel supports Waltop on
xf86-input-evdev and xf86-input-wacom or has been patched for support.
The WALTOP key word is again used in the match line with each of the
following snippets. Place the snippets below into a new file you'll
create in /etc/X11/xorg.conf.d called 52-waltop.conf. \<pre\> Section
"InputClass"

`   Identifier "Waltop buttons"`\
`   MatchProduct "WALTOP"`\
`       MatchIsKeyboard "on"`\
`   MatchDevicePath "/dev/input/event*"`\
`   Driver "evdev"`

EndSection

Section "InputClass"

`   Identifier "Waltop scroll"`\
`   MatchProduct "WALTOP"`\
`   MatchIsPointer "off"`\
`   MatchIsKeyboard "off"`\
`   MatchIsTouchpad "off"`\
`   MatchIsTablet "off"`\
`   MatchIsTouchscreen "off"`\
`   MatchDevicePath "/dev/input/event*"`\
`   Driver "evdev"`

EndSection

Section "InputClass"

`   Identifier "Waltop pen"`\
`   MatchProduct "WALTOP"`\
`   MatchIsTablet "on"`\
`   MatchDevicePath "/dev/input/event*"`\
`   Driver "wacom"`\
`       # Apply custom Options below this line.`

EndSection \</pre\> Since it is a system file you need root privileges.
If your system has gnome and sudo installed you can use: \<pre\> gksudo
gedit /etc/X11/xorg.conf.d/52-waltop.conf \</pre\> You would use
whatever text editor and root privilege access your distribution
supplies of course.

### 52-waltop.conf explained

1.  **"Waltop buttons"** snippet
    :   The tablet's frame/bezel buttons are handled as key events by
        the 10-evdev.conf "evdev keyboard catchall" snippet. This
        snippet functionally duplicates it with MatchIsKeyboard. The
        "Waltop buttons" identifier is a bit of a misnomer as dial
        zooming and volume control functions are also implemented as key
        presses.

2.  **"Waltop scroll"** snippet
    :   This snippet is currently required for the scroll feature of the
        multifunction dials. The multifunction dial's scroll is exported
        from the kernel by hid-waltop.ko as a separate device input
        event but X.Org does not recognize its specific device type.
        This means it needs to be reclaimed from any 10-evdev.conf
        snippet that may have mistakenly claimed it.

3.  **"Waltop pen"** snippet
    :   This snippet places only the Waltop pen on the Wacom X driver by
        use of MatchIsTablet. It duplicates the new "Waltop class"
        snippet in xf86-input-wacom-0.15.0's 50-wacom.conf. Static Wacom
        options may be added below the *Driver* line.

Waltop models supported by the wacom X driver
=============================================

The Wacom X driver is
[xf86-input-wacom](https://sourceforge.net/apps/mediawiki/linuxwacom/index.php?title=Xf86-input-wacom).
It has support for the Waltop tablets built-in as can be seen at
xf86-input-wacom/src/wcmUSB.c in the source code.

  ------------------------------------------
  Product ID   Corresponding WacomModelPtr
               
  ------------ -----------------------------
  0x24         &usbGraphire
               

  0x25         &usbGraphire2
               

  0x26         &usbGraphire2
               

  0x27         &usbGraphire3
               

  0x28         &usbGraphire3
               

  0x30         &usbGraphire4
               

  0x31         &usbGraphire4
               

  0x32         &usbBambooFun
               

  0x33         &usbBambooFun
               

  0x34         &usbBamboo1
               

  0x35         &usbGraphire4
               

  0x36         &usbGraphire4
               

  0x37         &usbGraphire4
               

  0x38         &usbBambooFun
               

  0x39         &usbBambooFun
               

  0x51         &usbBamboo
               

  0x52         &usbBamboo
               

  0x53         &usbBamboo
               

  0x54         &usbBamboo
               

  0x55         &usbBamboo
               

  0x56         &usbBamboo
               

  0x57         &usbBamboo
               

  0x58         &usbBamboo
               

  0x500        &usbBamboo
               

  0x501        &usbBamboo
               

  0x502        &usbIntuos4
               

  0x503        &usbIntuos4
               
  ------------------------------------------

  : ***Waltop Product IDs in wcmUSB.c***

-   Product IDs from the Waltop Linux driver (release date 2009/08/11),
    which was a fork of linuxwacom 0.8.4
    <http://www.waltop.com.tw/download.asp?lv=0&id=2>.

Further wacom set up information
================================

If you are using the Wacom X driver for your Waltop tablet the Linux
Wacom Project's mediawiki [HOWTO
page](https://sourceforge.net/apps/mediawiki/linuxwacom/index.php?title=Category:HOWTO)
has information on Calibration, Dual and MultiMonitor Set Up, Tablet
Configuration, Xsetwacom, etc.

