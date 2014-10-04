---
title: Tablet setup in MyPaint
summary: How to setup a tablet to work in MyPaint
---
> MyPaint is a fast and easy open-source graphics application for digital
> painters. It lets you focus on the art instead of the program. You work on
> your canvas with minimum distractions, bringing up the interface only when
> you need it.

— [About MyPaint](http://mypaint.intilinux.com/)

In this tutorial you can learn how to setup your tablet, which uses the
evdev driver, working in this software.

MyPaint 1.0
===========

This version has been released on the 22nd of November 2011. This is the
default version in most Linux distributions.

Tablet setup
------------

In order to set up your device choose *MyPaint\>Help\>Debug\>GTK input
device dialog* from the menu. On the **Input** dialogue select your
device and set the *Mode* to *Screen*. The following image illustrates
the settings.

![MyPaint 1.0 GTK input device
dialogue](setup.png "MyPaint 1.0 GTK input device dialogue")

\<!----\>

Pressure curve
--------------

In order to change the pressure curve used by your devices choose
*MyPaint\>Edit\>Preferences* from the menu. On the **Preferences**
dialogue change the curve as you wish.

![MyPaint 1.0 Preferences
dialogue](curve.png "MyPaint 1.0 Preferences dialogue")

Device testing
--------------

After you have set up your device, you can test if it is works or not.
In order to do that select *MyPaint\>Help\>Debug\>Test input devices*
from the menu. Start to draw something on the canvas and you can see on
the Input Device Test dialogue how the pressure and other data of the
pen changes.

![MyPaint 1.0 Input Device Test
dialogue](devicetest.png "MyPaint 1.0 Input Device Test dialogue")

MyPaint 0.9
===========

This version has been released on the 2nd of November 2010. This was the
default version of MyPaint in earlier Linux distributions, like Ubuntu
11.10. This version of the program do not have the GTK input dialogue
menu item, so you cannot setup your *(non wacom)* tablet.

More information
================

You can find more information about MyPaint on the [official website of
the software](http://mypaint.intilinux.com/).

If you have any questions or suggestions according to this article
please send an email to the [DIGI*mend* users mailing
list](mailto:digimend-users@lists.sourceforge.net).
