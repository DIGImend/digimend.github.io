---
title: Development
weight: 40
---

Tools
-----

The project has created and is using a number of tools to help with debugging
tablet issues and developing new drivers.

### hidrd

A [library and a tool][hidrd-home] for reading, writing and converting HID
report descriptors in/between various formats. We use it when fixing tablet
report descriptors for consumption by the Linux kernel. Can also be used for
authoring report descriptors and converting them to binary as part of a device
firmware build. Present in Debian and derived distros.

Latest release
: [hidrd v0.2.0][hidrd-latest] - May 27, 2010

[hidrd-home]: https://github.com/DIGImend/hidrd
[hidrd-latest]: https://github.com/DIGImend/hidrd/releases/tag/0.2.0

### huion-tools

A [collection of programs][huion-tools-home] for collecting and analyzing
diagnostic information from Huion graphics tablets.

Latest release
: [huion-tools v3][huion-tools-latest] - Oct 14, 2013

[huion-tools-home]: https://github.com/DIGImend/huion-tools
[huion-tools-latest]: https://github.com/DIGImend/huion-tools/releases/tag/v3

### usbhid-dump

A [USB HID dumping utility][usbhid-dump-home] based on libusb 1.0. It dumps
USB HID device report descriptors and reports themselves as they are being
sent, for all or specific device interfaces. Use it to find out how an input
device reports input events.  Included in
[usbutils](https://github.com/gregkh/usbutils), which is present in most
distributions.

Latest release
: [usbhid-dump v1.4][usbhid-dump-latest] - Nov 11, 2014

[usbhid-dump-home]: https://github.com/DIGImend/usbhid-dump
[usbhid-dump-latest]: https://github.com/DIGImend/usbhid-dump/releases/tag/1.4

### evdev-dump

A [very simple utility][evdev-dump-home] for dumping input event device
streams.  [Evtest](http://cgit.freedesktop.org/~whot/evtest/) is present in
most distributions and is recommended in most cases instead, except when it
is necessary to dump several devices at once with one tool.

Latest release
: [evdev-dump v1.0][evdev-dump-latest] - Jul 27, 2010

[evdev-dump-home]: https://github.com/DIGImend/evdev-dump
[evdev-dump-latest]: https://github.com/DIGImend/evdev-dump/releases/tag/1.0

Live streams
------------
Nikolai Kondrashov streams some of his work on the project to YouTube.
View his past recordings on
[the DIGImend project channel][digimend-youtube-channel].

[digimend-youtube-channel]: https://www.youtube.com/channel/UCioG-vCm2WB6nSz0Y0JTXoA
