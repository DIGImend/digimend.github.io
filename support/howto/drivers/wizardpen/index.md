If your kernel does not support your tablet the recommendation is to use
the DIGI*mend* supplied [kernel patches](Kernel patches "wikilink") or
[kernel](Kernel packages "wikilink"). For supported kernel versions and
tablets see the latest release's
[COMPATIBILITY](http://digimend.git.sourceforge.net/git/gitweb.cgi?p=digimend/kernel-patches.git;a=blob_plain;f=COMPATIBILITY;hb=v6)
file.

However there are tablets that do not yet have a kernel driver, mostly
because they are new or diagnostics for the tablet have not been
submitted. While waiting for a kernel driver to be developed for a
currently [unsupported tablet](Tablet support status "wikilink") you
might want to try the WizardPen driver as a work around. Both UC-logic
and Waltop tablets have reported success with it on recent kernels and X
Servers even though it has been deprecated. Ubuntu Precise 12.04 (kernel
3.2 and X server 1.11/1.12) and Quantal 12.10 (kernel 3.5 and X Server
1.13) for example. You might be able to enable your pen and maybe your
pen buttons. The tablet's frame buttons and mouse (if it has them) will
require the kernel driver.

Obtain the WizardPen driver
===========================

Download the latest WizardPen driver tar (wizardpen\_0.8.1) from Martin
Owen's Launchpad PPA onto your Desktop. Open a terminal and run (copy
and paste) the following commands: \<pre\> cd Desktop

wget
<https://launchpad.net/~doctormo/+archive/xorg-wizardpen/+files/xserver-xorg-input-wizardpen_0.8.1-0ubuntu3.tar.gz>
\</pre\> Extract it onto your Desktop by running the following in the
terminal: \<pre\> tar xvzf
xserver-xorg-input-wizardpen\_0.8.1-0ubuntu3.tar.gz \</pre\>

Compile the WizardPen driver
============================

These instructions are Ubuntu centric but you should be able to adapt
them to other Distros. With a little detective work you can find the
names your Distro uses for the libraries. And you would use your
Distro's package manager and equivalent of sudo naturally.

Preliminaries
-------------

The dependencies listed in the WizardPen README are: \<blockquote\>
build-essential xutils-dev xutils libx11-dev libxext-dev xautomation
xinput xserver-xorg-dev \</blockquote\> In Ubuntu build-essential
contains the gcc and C development library along with the Debian package
development tools and make utility.

Additionally in Ubuntu the following were added to cover "all the
bases": \<pre\> autoconf libtool pkg-config \</pre\> But not all of them
may be necessary.

Compiling
---------

Then to compile run the following commands: \<pre\> sudo apt-get install
build-essential xutils-dev xutils libx11-dev libxext-dev xautomation
xinput xserver-xorg-dev autoconf libtool pkg-config

cd xserver-xorg-input-wizardpen\_0.8.1

./autogen.sh --prefix=/usr

make

sudo make install \</pre\>

Installed files
---------------

When it is compiled and installed you will end up with following:

1.  the WizardPen X driver at
    /usr/lib/xorg/modules/input/wizardpen\_drv.so (and
    wizardpen\_drv.la)
2.  the WizardPen udev rules at
    /etc/udev/rules.d/67-xorg-wizardpen.rules
3.  the wizardpen .conf at /usr/share/X11/xorg.conf.d/70-wizardpen.conf
4.  calibration executable (alternative to xinput\_calibrator) at
    /usr/bin/wizardpen-calibrate
5.  the WizardPen manual (i.e. *man wizardpen* in a terminal) at
    /usr/share/man/man4/wizardpen.4

Tablets in 67-xorg-wizardpen.rules
==================================

Tablet OEMs and models in the Wizardpen udev rules are supported by the
WizardPen driver through its 70-wizardpen.conf. Tablets not included
will need a custom .conf file, see **X.org configuration** below. To
determine your tablet's Vendor ID and Product ID enter *lsusb* in a
terminal and look at the tablet's line in the output, e.g. 5543:6001.
\<blockquote\>

AceCad Corp\<BR\>

Flair II GT-504\<BR\>

:   VENDOR\_ID="0460", Product ID="0004"\<BR\>

\<BR\>

KYE Systems Corp\<BR\>

KYE Systems Corp Wide Screen Design Tablet TB-7300\<BR\>

:   VENDOR ID="0458", Product ID="5003"\<BR\>

KYE Systems Corp Wide Screen Design Tablet TB-7300\<BR\>

:   VENDOR ID="0458", Product ID="5004"\<BR\>

\<BR\>

UC-Logic Technology Corp\<BR\>

SuperPen WP3325U Tablet\<BR\>

:   VENDOR\_ID="5543", Product ID="0002"\<BR\>

WP4030, Genius MousePen 4x3 Tablet/Aquila L1 Tablet\<BR\>

:   VENDOR\_ID="5543", Product ID="0003"\<BR\>

WP5540, Genius MousePen 5x4 Tablet\<BR\>

:   VENDOR\_ID="5543", Product ID="0004"\<BR\>

WP8060, Genius MousePen 8x6 Tablet, Trust TB-6300\<BR\>

:   VENDOR\_ID="5543", Product ID="0005"\<BR\>

Genius PenSketch 6x8 Tablet\<BR\>

:   VENDOR\_ID="5543", Product ID="0041"\<BR\>

Genius PenSketch 12x9 Tablet\<BR\>

:   VENDOR\_ID="5543", Product ID="0042"\<BR\>

Digital Organizer (may not exist)\<BR\>

:   VENDOR\_ID="5543", Product ID="6000"\<BR\>

Genius G-Note 5000\<BR\>

:   VENDOR\_ID="5543", Product ID="6001"\<BR\>

\</blockquote\>

-   adapted from /etc/udev/rules.d/67-xorg-wizardpen.rules
-   Product ID is Model ID in udev rules.

X.org configuration
===================

70-wizardpen.conf
-----------------

This .conf file is installed in /usr/share/X11/xorg.conf.d when the
WizardPen driver is compiled. \<pre\> Section "InputClass"

`  Identifier "wizardpen"`\
`  MatchIsTablet "on"`\
`  MatchDevicePath "/dev/input/event*"`\
`  MatchTag "wizardpen"`\
`  Driver "wizardpen"`\
`  Option       "TopX"      "1506"`\
`  Option       "TopY"      "2705"`\
`  Option       "BottomX"   "31225"`\
`  Option       "BottomY"   "30892"`

EndSection \</pre\> The wizardpen.rules ensure MatchIsTablet matches and
sets the "wizardpen" tag for MatchTag. You may want to remove it (or
comment the lines out) if your tablet is not included in the udev rules,
unless you prefer to create a rule for your tablet.

Custom tablet .conf
-------------------

If your tablet is not one of the ones already included in the WizardPen
udev rules above you will need to create a custom .conf file as
described on the
[evdev](Tablet setup with xf86-input-evdev#X.org_configuration "wikilink")
or
[wacom](Tablet setup with xf86-input-wacom#X.org_configuration "wikilink")
driver pages. Add your custom .conf file to /etc/X11/xorg.conf.d. You
may need to create the xorg.conf.d directory if it is not already there.
\<pre\> sudo mkdir /etc/X11/xorg.conf.d \</pre\> Now using a text editor
create the 52-tablet.conf: \<pre\> gksudo gedit
/etc/X11/xorg.conf.d/52-tablet.conf \</pre\> and add the following
snippet to it. Determine the keyword used below from the \<device name\>
in the output of xinput list. Pick one as unique to your tablet as
possible. \<pre\> Section "InputClass"

`       Identifier "Tablet on WizardPen"`

1.  MatchIsTablet "on"

`       MatchProduct "keyword"`\
`       MatchDevicePath "/dev/input/event*"`\
`       Driver "wizardpen"`\
`       # Apply custom Options below.`

EndSection \</pre\> Save, Close, and restart. Provided you have done the
match correctly with luck your pen should be working. You can check if
you succesfully placed the pen on the WizardPen driver by looking in
Xorg.0.log at /var/log.

MatchIsTablet is commented out because the kernel might not be flagging
the tablet's pen as a tablet. If it does go ahead and use MatchIsTablet
by removing the comment (\#).

The WizardPen driver does not use the same options as the evdev or Wacom
drivers so be sure to look at *man wizardpen* entered in a terminal.

### Example custom .confs

For a H85 Kanvus tablet. The *lsusb* ouput is: \<pre\> Bus 003 Device
016: ID 5543:0782 UC-Logic Technology Corp. \</pre\> The *xinput list*
output is: \<pre\> ⎜ ↳ H850S id=10 [slave pointer (2)] ⎜ ↳ H850S id=11
[slave pointer (2)] \</pre\> Since the \<device name\> is "H850S" there
is no choice for a keyword, but "H850S" seems plenty unique. \<pre\>
Section "InputClass"

`       Identifier "UC-Logic tablet"`\
`       MatchIsTablet "on"`\
`       MatchProduct "H850S"`\
`       MatchDevicePath "/dev/input/event*"`\
`       Driver "wizardpen"`\
`       # Apply custom Options below.`

EndSection \</pre\>

-   thank you Muy-buro at Ubuntu forums for testing on Precise.

For a Kanvus Note A4. The *lsusb* ouput is: \<pre\> Bus 003 Device 053:
ID 5543:6003 UC-Logic Technology Corp. \</pre\> The *xinput list* output
is: \<pre\> ⎜ ↳ UC-LOGIC DIGITAL-Organizer id=11 [slave pointer (2)]
\</pre\> Given the \<device name\>, "UC-LOGIC" is a reasonable keyword.
And it could be combined with "H850S" creating a match to the Kanvus H85
also. \<pre\> Section "InputClass"

`       Identifier "UC-Logic tablet"`\
`       MatchIsTablet "on"`\
`       MatchProduct "UC-LOGIC|H850S"`\
`       MatchDevicePath "/dev/input/event*"`\
`       Driver "wizardpen"`\
`       # Apply custom Options below.`\
`   Option      "TopX"      "-10"`\
`   Option      "TopY"      "45"`\
`   Option      "BottomX"   "4020"`\
`   Option      "BottomY"   "2842"`

EndSection \</pre\>

-   coordinates arrived at empirically to account for the Note's
    mismatched aspect ratio to monitor. Active area is lost along the
    bottom of the tablet.
-   thank you fillemazendacus at Ubuntu forums for testing on Quantal.

For an ACCU branded Waltop tablet. The *lsusb* ouput is: \<pre\> Bus 004
Device 003: ID 172f:0051 Waltop International Corp. \</pre\> The *xinput
list* output is: \<pre\> ⎜ ↳ WALTOP International Corp. PEN-INPUT DEVICE
id=12 [slave pointer (2)] \</pre\> Note: the output actually contained 3
lines with stylus, eraser, and pad appended because the tablet was
originally setup on the Wacom driver, which did not work for it.

Here the \<device name\> is "WALTOP International Corp. PEN-INPUT
DEVICE" making "WALTOP" a good choice for the keyword. \<pre\> Section
"InputClass"

`   Identifier "Waltop pen"`\
`   MatchIsTablet "on"`\
`   MatchProduct "WALTOP"`\
`   MatchDevicePath "/dev/input/event*"`\
`   Driver "wizardpen"`\
`       # Apply custom Options below.`\
`   Option      "TopZ"      "50"  # pressure threshold`\
`   Option      "TopX"      "-10"`\
`   Option      "TopY"      "-10"`\
`   Option      "BottomX"   "7210"`\
`   Option      "BottomY"   "5410"`

EndSection \</pre\>

-   thank you artspace at Ubuntu forums for testing on Precise.

