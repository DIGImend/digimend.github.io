---
title: Tablet setup in Krita
summary: How to setup a tablet to work in Krita
---
> Krita is the full-featured free digital painting studio for artists who want
> to create professional work from start to end. Krita is used by comic book
> artists, illustrators, concept artists, matte and texture painters and in
> the digital VFX industry.Krita is free software, licensed under the GNU
> Public License, version 2 or later.
>
> — [About Krita](https://krita.org/about/press/)

In this tutorial you can learn how to setup your tablet, which uses the
evdev driver, working in this software.

Krita 2.4
=========

*"Krita 2.4 is the second release of Krita that is ready for end users, and
the first that is ready for professional digital artists."* — Krita 2.4
release notes. This version has been released on the 12th of April 2012. This
is the latest stable version of this program.

Tablet setup
------------

Krita is based on the QT library. The current QT version is 4.8.
Unfortunately QT 4.8 is not compatible with the evdev driver, so the
pen's pressure will not affect anything.

This bug has been reported to the QT bugtracker
[https://bugreports.qt-project.org/browse/QTBUG-25329 here]. You may sign up
to that site and vote for this bug.

In the comments of that bug you can find some tips how to make the library
evdev compatible. There is a lot of applications using this library, using a
modified library is very risky. So use those tips at your own risk.

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
curve](24globalpressure.png "Krita 2.4 Setting up the global pressure curve")

### Brush pressure curves

Click on the brush icon and the select for example *Basic\_paint\_25*
brush. If you click on *Opacity* then you can set up your opacity curve
for the brush. If the *Use curve* is not checked the pressure is
ignored. If *Use the same curve* checked, all inputs sensors will use
the same curve. If not checked, you will be able to assign different
curves to all active input sensors.

![Krita 2.4 Setting up a pressure curve for a
brush](24pressureopacity.png "Krita 2.4 Setting up a pressure curve for a brush")

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
