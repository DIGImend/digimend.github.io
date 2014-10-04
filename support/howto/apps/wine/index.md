---
title: Tablet setup in Wine
summary: How to setup a tablet to work with Windows applications running under Wine
---
> Wine (originally an acronym for "Wine Is Not an Emulator") is a
> compatibility layer capable of running Windows applications on several
> POSIX-compliant operating systems, such as Linux, Mac OSX, & BSD.
>
> — [About Wine](https://www.winehq.org/about/)

In this tutorial you can learn how to setup your tablet, which uses the
evdev driver, working in this software.

Wine 1.5
========

The Wine team started to work on the 1.5 develpment release in March
2012. This version is the latest development version wich is currently
in active development.

Tablet setup
------------

This sofware is not compatible with the evdev driver, so the pen's
pressure will not affect anything.

This bug has been reported to the Wine bugtracker
[here](http://bugs.winehq.org/show_bug.cgi?id=30893). In the comments of that
bug you can find some tips how to make the software evdev-compatible. You can
build a corrected Wine for yourself, but that modification can make Wine
unstable, so use those tips at your own risk.

If you were brave enough to have made Wine evdev compatible by patching
and compiling the source code yourself, you will not need to do any
device configuration. Your pen should just work in Wine.

More information
================

You can find more information about Wine on the [official website of the
software](http://www.winehq.org/).

If you have any questions or suggestions according to this article
please send an email to the [DIGI*mend* users mailing
list](mailto:digimend-users@lists.sourceforge.net).
