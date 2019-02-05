---
title: Drivers
weight: 20
---
We strive to submit all drivers and fixes to upstream projects, including the
Linux kernel, xf86-input-evdev and xf86-input-wacom. However, to support
people running older software we maintain an out-of-tree kernel driver
package.

digimend-kernel-drivers
-----------------------

To make it easier to develop and test new kernel drivers, and to let people
running older kernels use their new tablets, we're maintainig an [out-of-tree
driver package][11]. It works on v3.5 and later kernels and [supports a
variety of tablets](/drivers/digimend/tablets/).

Latest release
: [digimend-kernel-drivers v9][19] - Dec 15, 2018

[11]: https://github.com/DIGImend/digimend-kernel-drivers
[12]: https://github.com/DIGImend/digimend-kernel-drivers/blob/master/README.md
[19]: https://github.com/DIGImend/digimend-kernel-drivers/releases/tag/v9

digimend-kernel-patches
----------------------
A [set of patches][21] adding support for a few tablets to some older Linux
kernel versions (see [README.md][22]). The Linux kernel started supporting
out-of-tree HID drivers in v3.5 and patching the kernel to add support for new
tablets became a rare necessity, so work mostly moved to
[digimend-kernel-drivers][11].  Most people don't need these patches nowadays.

Latest release
: [digimend-kernel-patches v6][29] - Sep 8, 2012

[21]: https://github.com/DIGImend/digimend-kernel-patches
[22]: https://github.com/DIGImend/digimend-kernel-patches/blob/master/README.md
[29]: https://github.com/DIGImend/digimend-kernel-patches/releases/tag/v6

digimend-evdev-patches
----------------------
A [set of patches][31] for very old xf86-input-evdev versions. Most people
don't need them nowadays.

Latest release
: [digimend-evdev-patches v0.1][39] - Sep 9, 2010

[31]: https://github.com/DIGImend/digimend-evdev-patches
[39]: https://github.com/DIGImend/digimend-evdev-patches/releases/tag/v0.1
