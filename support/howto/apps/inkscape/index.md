---
title: Tablet setup in Inkscape
summary: How to setup a tablet to work in Inkscape
---
An Open Source vector graphics editor, with capabilities similar to
Illustrator, CorelDraw, or Xara X, using the W3C standard Scalable
Vector Graphics (SVG) file format.

Inkscape supports many advanced SVG features (markers, clones, alpha
blending, etc.) and great care is taken in designing a streamlined
interface. It is very easy to edit nodes, perform complex path
operations, trace bitmaps and much more. \<sup\>[1]\</sup\>

In this tutorial you can learn how to setup your tablet, which uses the
evdev driver, working in this software.

Inkscape 0.48
=============

Inkscape 0.48.3 has been released at the begining of 2012. This is the
latest version of the program and this version is what you can find in
many Linux distributions.

Tablet setup
------------

In order to setup your tablet choose *File\>Input devices* from the
menu. The **Input Devices** dialogue appears as a docked dialogue. On
that dialogue select your device and set it's mode to *Screen*. The
following image illustrate this step.

![Inkscape 0.48 Input Devices dialogue to set up your
tablet](devicesetup.png "Inkscape 0.48 Input Devices dialogue to set up your tablet")

If you have a multiple input device (like a pen with a mouse), it a bit
hard to choose which one is the pen, since they both got identical
device names. In this case on this dialogue click on the *Hardware* tab.
You can see how many axes a device has. A mouse has two axes, a pen has
more than two axes, so with this information you can select which device
you want to configure.

![Inkscape 0.48 Input Devices dialogue hardware information for checking
the axes
count](axescount.png "Inkscape 0.48 Input Devices dialogue hardware information for checking the axes count")

Then go back to the *Configuration* tab and set up your device as
described above.

Testing the device
------------------

After you have configured the device you can test it too. In order to do
that click on the *Hardware* tab of the **Input Devices** dialogue.
Start drawing on the *Test Area* and you can see the how the data from
the pen changes.

![Inkscape 0.48 Input Devices dialogue hardware
test](devicetest.png "Inkscape 0.48 Input Devices dialogue hardware test")

(On the dialogue the axes represents the following data (from top to
bottom): X coordinate, Y coordinate, Pressure, X tilt, Y tilt, Wheel.)

More information
================

You can find more information about Inkscape on the [official website of
the software](http://inkscape.org/).

If you have any questions or suggestions according to this article
please send an email to the [DIGI*mend* users mailing
list](mailto:digimend-users@lists.sourceforge.net).

References
==========

1\. <http://inkscape.org/>
