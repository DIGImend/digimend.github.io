---
title: Collecting tablet diagnostics
summary: How to collect diagnostics to help make a driver
---
Disclaimer: the DIGImend project currently lacks capacity to process
diagnostics and make tablet drivers although you still can send
diagnostics, we cannot promise that any drivers will be made for the
tablets in question.

Please follow these steps to collect the information required to make a
driver for your tablet. Please send the resulting files to the
[DIGImend-devel](mailto:DIGImend-devel@lists.sourceforge.net?subject=Tablet%20diagnostics)
maillist.

All the steps need to be executed in a terminal with the tablet plugged
in. You could use either a text mode console or a terminal emulator
running under X. However, please don't rely on your tablet to control
the terminal, as some steps will temporarily detach it from the kernel
driver, which will stop your terminal from receiving the input.

You will need "lsusb" and "usbhid-dump" utilities which are both
included into "usbutils" package, available in most distributions.

Identify original model
-----------------------

Use lsusb to find the tablet's USB vendor and product IDs (USB VID/PID).
Run "lsusb" and find the line containing your tablet model or
manufacturer's name in the output. The VID/PID pair will be displayed
next to "ID" as two hexadecimal numbers separated by a colon.

Example:

    $ lsusb
    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 003 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 004 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 005 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 001 Device 004: ID 2232:1001
    Bus 003 Device 002: ID 046d:c52b Logitech, Inc. Unifying Receiver
    Bus 003 Device 003: ID 0a5c:219c Broadcom Corp.
    Bus 005 Device 002: ID 172f:0501 Waltop International Corp. Media Tablet 10.6"

"172f:0501" above is the [Waltop Media Tablet
10.6"](/tablets/Waltop_Media_Tablet_10.6_inch/) USB VID and PID.

If you can't identify your tablet just by looking at vendor/product
names, you could unplug it, run lsusb, plug it back, run lsusb again and
notice which line has appeared - it will represent your tablet.

For convenience executing the following steps, you could put the found
VID/PID pair in a shell variable, like this:

    $ T=172f:0501

The following examples will refer to this shell variable as "\$T" and
will assume it contains the tablet VID/PID pair. So, if you assign it,
you could just copy-paste and execute the commands from the examples in
your terminal.

Retrieve USB descriptors
------------------------

Retrieve USB device, configuration, interface and endpoint descriptors,
using lsusb. Run "sudo lsusb -v -d VID:PID" and attach the output to the
diagnostics message.

Example:

    $ sudo lsusb -v -d $T > descriptors.txt

The above stores the lsusb output in "descriptors.txt" file.

Retrieve USB HID report descriptor(s) using usbhid-dump. Run "sudo usbhid-dump
-ed -m VID:PID" and attach the output to the diagnostics message.

Example:

    $ sudo usbhid-dump -ed -m $T > hid_report_descriptors.txt

The above stores the usbhid-dump output in
"hid\_report\_descriptors.txt" file.

Collect raw input samples
-------------------------

Raw input samples could be collected either with usbhid-dump or with a USB
traffic sniffer, such as [Wireshark](http://www.wireshark.org/) or
[tcpdump](http://www.tcpdump.org/). In most cases ubshid-dump is sufficient.

There are several input samples to be collected. Please put them into
separate attachments, if possible. To collect an input sample with
usbhid-dump, execute "sudo usbhid-dump -es -m VID:PID". It is best to
redirect sample output to a file, because there is often quite a lot of
it.

Example:

    $ sudo usbhid-dump -es -m $T | tee frame_wheel_srolling.txt
    Starting dumping interrupt transfer stream
    with 1 minute timeout.

    005:002:000:STREAM             1331485997.649152
     01 00 00 00 FF 00 00 00

    005:002:000:STREAM             1331485999.213131
     01 00 00 00 01 00 00 00

    005:002:000:STREAM             1331486014.777136
     01 00 00 00 00 FF 00 00

    005:002:000:STREAM             1331486016.477138
     01 00 00 00 00 01 00 00

    ^C

Here, the displayed stream output will also be stored in
"frame\_wheel\_srolling.txt" file.

Please follow the (applicable) steps below to collect all the sample
types. Each step has an example command you could copy-paste into your
terminal to have the output stored in separate and appropriately-named
files. When capturing each sample type, it is better to repeat the
described input 3 to 5 times for better precision.

### Pen

All graphics tablets have a pen, so these samples are required.

#### Coordinates

Make four strokes with the pen, starting from inside the tablet working
area, slowly moving outside and crossing each border in the following
(clockwise) order: top, right, bottom, left.

    $ sudo usbhid-dump -es -m $T | tee pen_coords.txt

[Video](http://youtu.be/ysYk8KLY98U)

#### Tilt angles

If you're sure your tablet doesn't support pen tilt angle reporting,
please skip this step. Otherwise, please collect a sample. Most
inexpensive tablets don't support it, though.

Put the pen vertically, tip touching the center of the working area, and
slowly tilt the opposing end towards each working area border in turn,
as far as the pen shape allows, without lifting the pen tip, then tilt
it back. It is OK if the pen tip is lifted during tilting, it is just
unnecessary to tilt further than that. Please tilt towards the borders
in the following (clockwise) order: top, right, bottom, left.

    $ sudo usbhid-dump -es -m $T | tee pen_tilt.txt

[Video](http://youtu.be/h11zK-p_h4w)

#### Pressure

Hold your pen as you do for drawing and press its tip on the working
tablet surface with the strength appropriate for the full pressure -
press it until it stops sliding into the pen and then press a little
more. Please don't press hard enough to break the tip.

    $ sudo usbhid-dump -es -m $T | tee pen_pressure.txt

[Video](http://youtu.be/cukrOS5MKyA)

#### Buttons

Bring the pen tip close to the tablet surface (within, say, 5 mm / 0.2
inches), but not touching it or anything else. Then, holding the pen
with one hand, press the pen side buttons with the other, first - the
lower button, then the upper. It doesn't matter if the pen tip touches
anything in between presses, it is only important it doesn't touch
anything at the same time as pressing the buttons.

    $ sudo usbhid-dump -es -m $T | tee pen_buttons.txt

[Video](http://youtu.be/2NC4c2x33ZE)

### Frame controls

If your tablet doesn't have additional controls on the frame, such as
buttons or dials, please skip these steps.

#### Dials

When generic tablets have dials, they often have buttons, which control
their function. Please capture input samples for every dial function
mode. In each mode, please do several rotations of the dials, first
clockwise, then counter-clockwise.

There are usually two dials on either side of the tablet, which produce
the same input and are intended for equally convenient right/left-handed
use, so there is no need to rotate both of them.

Touch-based sensor dials could be acting as directional buttons in some
modes. In such cases, please press every direction in this (clockwise)
order: top, right, bottom, left.

    $ sudo usbhid-dump -es -m $T | tee frame_dials.txt

[Video](http://youtu.be/jXBZH7D3_7k)

#### Buttons

If your tablet has no buttons on the frame, aside from those controlling
dials' input mode, please skip this step.

Please press every button on the frame, in left-to-right, top-to-bottom
order. If the order still wasn't obvious, please describe it in the
message.

If there are two identical (mirrored) sets of buttons on either side of
the working area, please press first the buttons on the left, then those
on the right. Although these buttons are intended to be equally used by
left and right hand, they may produce different key codes.

    $ sudo usbhid-dump -es -m $T | tee frame_buttons.txt

[Video](http://youtu.be/z8i17sFSUW8)

### Mouse

If your tablet didn't have a mouse bundled, please skip these steps.

#### Coordinates

Place the mouse within the working area and move it outside, over each
border in turn, in this (clockwise) order: top, right, bottom, left. It
may be more convenient to hold the mouse by its sides so it is easier to
lift a little over the bezel edge.

    $ sudo usbhid-dump -es -m $T | tee mouse_coords.txt

[Video](http://youtu.be/iooZjBENDqc)

#### Buttons

Place the mouse within the working area and then click each button in
turn in this order: left, right, middle.

    $ sudo usbhid-dump -es -m $T | tee mouse_buttons.txt

[Video](http://youtu.be/z7KO2SGkPRA)

#### Wheel

Some older tablet mice have rocking wheel, which only rocks (clicks) in
two directions, while newer models have traditional, fully rotating
wheels.

Place the mouse within the working area and rotate (rock) the wheel,
first away from you, then towards you.

    $ sudo usbhid-dump -es -m $T | tee mouse_wheel.txt

[Video](http://youtu.be/kfWpgHXqKyg)

Send the results
----------------

Please attach the resulting files to a message, either separately or in
an archive, and send to the
[DIGImend-devel](mailto:DIGImend-devel@lists.sourceforge.net?subject=Tablet%20diagnostics)
maillist. Also please be sure to mention the tablet manufacturer and
model name.

