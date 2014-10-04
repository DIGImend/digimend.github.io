---
title: Tablet setup in Krita
summary: How to setup a tablet to work in Krita
---
Krita is a flexible painting application that makes creating art from
scratch or existing resources a fun and productive experience. With many
powerful brush engines and unique features as multi- hand and mirrored
painting, Krita explicitly supports creating concept art, storyboards,
textures, matte paintings and illustrations.\<sup\>[1]\</sup\>

In this tutorial you can learn how to setup your tablet, which uses the
evdev driver, working in this software.

Krita 2.4
=========

Krita 2.4 is the second release of Krita that is ready for end users,
and the first that is ready for professional digital
artists.\<sup\>[1]\</sup\>\<br\> This version has been released on the
12th of April 2012. This is the latest stable version of this program.

Tablet setup
------------

Krita is based on the QT library. The current QT version is 4.8.
Unfortunately QT 4.8 is not compatible with the evdev driver, so the
pen's pressure will not affect anything.

If you were brave enough to have made QT evdev compatible by patching
and compiling the source code yourself, you will not need to do any
device configuration. Your pen should just work in Krita.

Brush dynamics
--------------

You can modify how the pressure affects what you are drawing in two
ways. You can modify the global pressure curve of the application or you
can set up different pressure curves for different brushes.

### Global pressure curve

You can set up a global pressure curve in Krita. In order to do that
select *Settings\>Configure Krita* and on the **Preferences** dialogue
select *Tablet settings*. Then you can modify the curve as you wish.

![Krita 2.4 Setting up the global pressure
curve](krita24globalpressure.png "Krita 2.4 Setting up the global pressure curve")

### Brush pressure curves

Click on the brush icon and the select for example *Basic\_paint\_25*
brush. If you click on *Opacity* then you can set up your opacity curve
for the brush. If the *Use curve* is not checked the pressure is
ignored. If *Use the same curve* checked, all inputs sensors will use
the same curve. If not checked, you will be able to assign different
curves to all active input sensors.\<sup\>[2]\</sup\>

![Krita 2.4 Setting up a pressure curve for a
brush](krita24pressureopacity.png "Krita 2.4 Setting up a pressure curve for a brush")

The same way you can set up different pressure curves for *Size*,
*Spacing*, *Mirror*, *Softness*, *Sharpness*, *Rotation*, *Scatter*,
*Darken*, *Mix*, *Hue*, *Saturation* and *Value*.

More information
================

You can find more information about Krita on the [official website of
the software](http://krita.org/).

If you have any questions or suggestions according to this article
please send an email to the [DIGI*mend* users mailing
list](mailto:digimend-users@lists.sourceforge.net).

References
==========

1\. [Krita 2.4 – Creative
Freedom](http://www.krita.org/about-krita-2-4.pdf)\<br\> 2.
<http://permalink.gmane.org/gmane.comp.kde.cvs/1138910>

[Category:Tablet setup in
applications](Category:Tablet setup in applications "wikilink")

