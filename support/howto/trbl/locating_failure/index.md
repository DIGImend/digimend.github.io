---
title: How to locate a failure
summary: How to find the reason your tablet doesn't work
---
This guide will help you find the reason your tablet doesn't work under Linux
and who can help you fix it. You'll need to know how to minimally use Linux
command line. This guide won't hold your hand and will expect you to find some
things out by yourself. We'll start from the bottom.

* TOC
{:toc}

Computer hardware
-----------------
As unlikely as it sounds, it sometimes happens, and might be good to rule out.
Sometimes the USB host hardware in computers is faulty and your tablet might
experience intermittent disconnects or similar transient issues. To make sure
that is not the case, try the tablet on another machine, preferably a
different model or make, but with the same operating system. If it works
there, then the computer hardware is likely at fault and you need to repair or
replace it. Proceed to the next section if it still doesn't work.

Don't be worried if you can't do this test. Such failures are rare and I think
I only saw them twice through all the history of the project.

Tablet hardware
---------------
Next thing you should do is check if your tablet works under Windows. If you
installed the required Windows drivers correctly and it still doesn't work,
then that's likely a hardware issue. To be extra sure try it on yet another
Windows machine, preferably with another Windows version.

If you don't have a Windows machine handy, but have an install disc or image,
and an appropriate license, you can run it in a virtual machine under
[VirtualBox], [Virt-Manager] or similar, install the drivers, connect the
tablet from the host system using VM configuration, and test it.

If you have none of the above, then you can use your Linux system to talk to
the tablet directly and try to figure out if what it says makes sense.

First, find out the vendor and product IDs of your tablet, commonly
abbreviated to VID and PID, and represented as a pair of 4-digit hexadecimal
numbers with a colon in between. You can see each tablet having them on our
[tablets][tablets] page. For example, [KYE EasyPen i405X][KYE_EasyPen_i405X]
has VID:PID of 0458:5010, but e.g. all Huion tablets share the same VID:PID
of 256c:006e (in abuse of the USB standard).

To find the VID:PID of your tablet, simply disconnect it from your computer,
run `lsusb`, connect it again, run `lsusb` again, and then compare the outputs
and find the line which appeared in the second output - that's your tablet.

    $ lsusb
    Bus 004 Device 006: ID 046d:c52b Logitech, Inc. Unifying Receiver
    Bus 004 Device 005: ID 05e3:0608 Genesys Logic, Inc. Hub
    Bus 004 Device 003: ID 17ef:100a Lenovo ThinkPad Mini Dock Plus Series 3
    Bus 004 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
    Bus 003 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
    $ lsusb
    Bus 004 Device 006: ID 046d:c52b Logitech, Inc. Unifying Receiver
    Bus 004 Device 005: ID 05e3:0608 Genesys Logic, Inc. Hub
    Bus 004 Device 003: ID 17ef:100a Lenovo ThinkPad Mini Dock Plus Series 3
    Bus 004 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
    Bus 003 Device 006: ID 256c:006e  
    Bus 003 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub

In the example above, the new line corresponding to the tablet is:

    Bus 003 Device 006: ID 256c:006e  

It describes a Huion tablet. Note how there's nothing after the VID:PID
(256c:006e). This is an extreme, but possible case, where the tablet doesn't
report actual vendor or product name, which would have helped to identify it.
That's why the ultimate method of finding your tablet in `lsusb` output is
comparing two outputs before and after plugging the tablet in.

Once you found your tablet VID:PID pair, you can run `usbhid-dump` to see what
the tablet says. Run `sudo usbhid-dump -es -m <VID:PID>`, where `<VID:PID>` is
your tablet's VID:PID pair. E.g. for the tablet VID:PID we found above the
command will be:

    sudo usbhid-dump -es -m 256c:006e

Once you start the command, it will begin listening to your tablet and
outputting the data it receives:

    Starting dumping interrupt transfer stream
    with 1 minute timeout.

    003:006:000:STREAM             1474196897.024195
     07 80 5A 60 5D 21 00 00

    003:006:000:STREAM             1474196897.028067
     07 80 5D 60 61 21 00 00

    003:006:000:STREAM             1474196897.030137
     07 80 5D 60 66 21 00 00

    003:006:000:STREAM             1474196897.034089
     07 80 5D 60 6C 21 00 00

    003:006:000:STREAM             1474196897.036189
     07 80 57 60 72 21 00 00

    003:006:000:STREAM             1474196897.040095
     07 80 52 60 79 21 00 00

The above is a very short excerpt of reports sent by a Huion Tablet in
response to its pen hovering above the drawing surface. Once you're done
looking at the reports, press Ctrl-C to terminate usbhid-dump.

Deciphering everything the tablet sends can be hard and it isn't really
necessary to find out if the tablet hardware works in general. Usually it is
sufficient to simply see if the tablet reports **change** in response to the
kind of input that you see not working. Remember also that numbers in the
reports are in little-endian format, e.g. the least significant byte comes
first (leftmost), followed by more significant bytes. So, a 32-bit hexadecimal
number 1234ABCD will be output as `CD AB 34 12`.

For the most frequent complaint of pressure detection not working, try
holding your pen still, increase and reduce pressure on the tip repeatedly,
and see if the reports change in sync with that. You should see some bytes in
the reports go up as you increase the pressure:

    003:006:000:STREAM             1474197596.184651
     07 81 F7 85 C7 1F F4 05

    003:006:000:STREAM             1474197596.188573
     07 81 F7 85 C7 1F 3F 06

    003:006:000:STREAM             1474197596.194530
     07 81 F7 85 C7 1F 8E 06

    003:006:000:STREAM             1474197596.198662
     07 81 F7 85 C7 1F E6 06

    003:006:000:STREAM             1474197596.202603
     07 81 F7 85 C7 1F 3A 07

    003:006:000:STREAM             1474197596.206529
     07 81 F7 85 C7 1F 95 07

And go down as you decrease it:

    003:006:000:STREAM             1474197596.822554
     07 81 DD 85 83 1F FF 07

    003:006:000:STREAM             1474197596.828535
     07 81 DD 85 87 1F C8 06

    003:006:000:STREAM             1474197596.832432
     07 81 DB 85 8D 1F 9C 03

    003:006:000:STREAM             1474197596.836574
     07 80 DB 85 98 1F 00 00

Note also that with any pen input it is very hard to keep the pen completely
still and so reported pen coordinates will almost always change with anything
else you're trying to do, such as varying the pressure. Can you see the
pressure changing in the reports above? Try finding it.

Yes, it is the last two bytes. In most cases this is where the pressure is.
In some very rare cases, where the tablet actually supports reporting tilt
angles (I only ever saw one such non-Wacom tablet), they will follow pressure.

Don't be discouraged if you don't see the tablet responding to the input
you're trying to make. It can still be the case, that Linux simply doesn't
initialize tablet properly and it doesn't e.g. report pressure, or clicks on
the buttons around the drawing area. To rule **that** out you will have to
test with Windows.

If you're **sure** Linux supports your tablet and you still don't see the
reaction to the input, or it doesn't work on Windows despite your best
efforts, then your tablet hardware is likely at fault and you'll need to send
it for repair or replacement. Otherwise, proceed to the next section.

Kernel drivers
--------------

### Debugfs interface

The kernel drivers can be buggy, or simply don't know about your tablet. To
see if that's the case, we can look at the kernel debug interfaces under
/sys/kernel/debug/hid.

First, become root:

    sudo su -

Then go to /sys/kernel/debug/hid directory:

    cd /sys/kernel/debug/hid

List all files there:

    ls -l

You will likely see a couple directories. Each HID interface on a USB device
will have a separate directory:

    root@gimli:/sys/kernel/debug/hid# ls -l
    total 0
    drwxr-xr-x 2 root root 0 Oct 29 19:42 0003:046D:C016.0001
    drwxr-xr-x 2 root root 0 Oct 30 14:39 0003:256C:006E.000E
    drwxr-xr-x 2 root root 0 Oct 30 14:39 0003:256C:006E.000F
    drwxr-xr-x 2 root root 0 Oct 30 14:39 0003:256C:006E.0010

The directory names consist of four hexadecimal numbers separated by colons.
First number is the bus ID (3 for USB), second number is device VID, third
number is device PID, and the fourth and last number is the HID interface
number, which is incremented with each connected device. If you disconnect
your tablet and connect it again it will get different HID interface numbers.

In the example above the first directory belongs to a USB mouse, and the other
three belong to a [Huion H610][Huion_H610] tablet. Each of the directories
contains two files, `rdesc` and `events`:

    root@gimli:/sys/kernel/debug/hid/0003:256C:006E.000E# ls -l
    total 0
    -r-------- 1 root root 0 Oct 30 14:39 events
    -r-------- 1 root root 0 Oct 30 14:39 rdesc

These are not regular files, but instead contain data generated by the kernel
as you read them. The `rdesc` file will contain the interface HID report
descriptor and its interpretation by the kernel, which is useful, but is
hard to interpret, and we'd better focus on the `events` file.

When you read the `events` file, the kernel will be outputting the stream of
input reports it reads from the device and how it interpreted them, without
end. However, this is done only if something actually reads from the
corresponding device. If you have no user-space drivers handling this device,
you might as well see nothing in `events` file. In that case, running `evtest`
on the corresponding event device (see below), will fix it.

As before, you might need to try reading the `events` file for each device and
do some input to find the device directory you need. That, or look at and
interpret the contents of the corresponding `rdesc` file. Here is an example
output for the pen interface of the same Huion H610:

    root@gimli:/sys/kernel/debug/hid/0003:256C:006E.000E# cat events 

    report (size 8) (numbered) =  07 80 a3 62 c9 3b 00 00
    Digitizers.TipSwitch = 0
    Digitizers.BarrelSwitch = 0
    Digitizers.TabletPick = 0
    Digitizers.InRange = 1
    GenericDesktop.X = 25251
    GenericDesktop.Y = 15305
    Digitizers.TipPressure = 0

    report (size 8) (numbered) =  07 80 9c 62 b6 3b 00 00
    Digitizers.TipSwitch = 0
    Digitizers.BarrelSwitch = 0
    Digitizers.TabletPick = 0
    Digitizers.InRange = 1
    GenericDesktop.X = 25244
    GenericDesktop.Y = 15286
    Digitizers.TipPressure = 0

    report (size 8) (numbered) =  07 80 99 62 bb 3b 00 00
    Digitizers.TipSwitch = 0
    Digitizers.BarrelSwitch = 0
    Digitizers.TabletPick = 0
    Digitizers.InRange = 1
    GenericDesktop.X = 25241
    GenericDesktop.Y = 15291
    Digitizers.TipPressure = 0

The above is truncated to just three reports, you will get much more than that
with a pen interface of your tablet.

### Evtest tool

Another approach is to see what the tablet looks like through the evdev
interface, which userspace drivers use. Make sure you have the `evtest`
package installed, and then run the `evtest` tool:

    sudo evtest

It will present you with the choice of devices to test. Something like this:

    nick@gimli:~$ sudo evtest
    No device specified, trying to scan all of /dev/input/event*
    Available devices:
    /dev/input/event0:      AT Translated Set 2 keyboard
    /dev/input/event1:      TPPS/2 IBM TrackPoint
    /dev/input/event2:      Lid Switch
    /dev/input/event3:      Sleep Button
    /dev/input/event4:      Power Button
    /dev/input/event5:      PC Speaker
    /dev/input/event6:      ThinkPad Extra Buttons
    /dev/input/event7:      Integrated Camera
    /dev/input/event8:      Video Bus
    /dev/input/event9:      HDA Digital PCBeep
    /dev/input/event10:     HDA Intel PCH Mic
    /dev/input/event11:     HDA Intel PCH Dock Mic
    /dev/input/event12:     HDA Intel PCH Headphone
    /dev/input/event13:     HDA Intel PCH Dock Headphone
    /dev/input/event14:     HDA Intel PCH HDMI/DP,pcm=3
    /dev/input/event15:     HDA Intel PCH HDMI/DP,pcm=7
    /dev/input/event16:     HDA Intel PCH HDMI/DP,pcm=8
    /dev/input/event17:     Logitech Optical USB Mouse
    /dev/input/event18:     10594 Pen
    /dev/input/event19:     10594 Mouse
    /dev/input/event20:     10594 Keyboard
    /dev/input/event21:     10594 Consumer Control
    /dev/input/event22:     10594 System Control
    Select the device event number [0-22]: 

Your tablet can be represented by several devices: one for pen input, another
for frame buttons, a third one for the mouse, if it has one, and so on. In the
output above the tablet interfaces are listed last and are clearly labeled,
thanks to the work of Benjamin Tissoires on the UC-Logic driver. Other tablets
might not be so well catered-for, so you might need to figure out which one is
which and select appropriately. Otherwise you can simply try them one-by-one
until you find the one that produces output when you do the kind of input
you're trying to troubleshoot.

Enter the device number and you will be presented with a list of events and
value ranges the corresponding interface can report, followed by endless
output of events the kernel reports. Example for Huion H610 pen interface:

    Select the device event number [0-22]: 18
    Input driver version is 1.0.1
    Input device ID: bus 0x3 vendor 0x256c product 0x6e version 0x111
    Input device name: "10594 Pen"
    Supported events:
      Event type 0 (EV_SYN)
      Event type 1 (EV_KEY)
        Event code 320 (BTN_TOOL_PEN)
        Event code 330 (BTN_TOUCH)
        Event code 331 (BTN_STYLUS)
        Event code 332 (BTN_STYLUS2)
      Event type 3 (EV_ABS)
        Event code 0 (ABS_X)
          Value  24748
          Min        0
          Max    40000
          Resolution     157
        Event code 1 (ABS_Y)
          Value  15294
          Min        0
          Max    25000
          Resolution     157
        Event code 24 (ABS_PRESSURE)
          Value      0
          Min        0
          Max     2047
      Event type 4 (EV_MSC)
        Event code 4 (MSC_SCAN)
    Properties:
    Testing ... (interrupt to exit)
    Event: time 1477836527.658645, type 1 (EV_KEY), code 320 (BTN_TOOL_PEN), value 1
    Event: time 1477836527.658645, type 3 (EV_ABS), code 0 (ABS_X), value 21147
    Event: time 1477836527.658645, type 3 (EV_ABS), code 1 (ABS_Y), value 14660
    Event: time 1477836527.658645, -------------- SYN_REPORT ------------
    Event: time 1477836527.662601, type 3 (EV_ABS), code 0 (ABS_X), value 21134
    Event: time 1477836527.662601, type 3 (EV_ABS), code 1 (ABS_Y), value 14682
    Event: time 1477836527.662601, -------------- SYN_REPORT ------------
    Event: time 1477836527.664558, type 3 (EV_ABS), code 0 (ABS_X), value 21113
    Event: time 1477836527.664558, type 3 (EV_ABS), code 1 (ABS_Y), value 14711
    Event: time 1477836527.664558, -------------- SYN_REPORT ------------
    Event: time 1477836527.668588, type 3 (EV_ABS), code 0 (ABS_X), value 21105
    Event: time 1477836527.668588, -------------- SYN_REPORT ------------

As mention before, pen interfaces produce a lot of events, and the above is
just a short snippet.

### In any case

Look at the produced output and try to see if it makes sense, e.g. if the pen
movement affects appropriate axes fully, button clicks make sense, etc.

If you think the output you see is wrong, report your problem to the bug
tracker of your Linux distribution. If that fails, report it to the kernel
bugtracker at https://bugzilla.kernel.org/.

If the output looks right to you, move on to the next section.

Userspace drivers
-----------------
If you use the still most popular X.org display server, you're likely using
either xf86-input-evdev or the newer xf86-input-libinput drivers. You can also
be using the xf86-input-wacom driver, but only if you set it up yourself, in
which case you would know. Unfortunately, I cannot help you if you use Mir or
Wayland, as I haven't used them myself yet. Refer to their documentation and
support forum.

To test the X.org drivers you will need the `xinput` tool, usually available
in the package by the same name. First, find out your device name. Run `xinput
list` to see all devices. They will look similar to what you get from the
evdev output and you can use the same approach to find the one you need: read
the device names, and if several look like what you need, try them one by one
by supplying their name or IDs to `xinput test` command, and then doing the
appropriate input until it produces output.

Here is an example for Huion H610:

    nick@gimli:~$ xinput list
    ⎡ Virtual core pointer                          id=2    [master pointer  (3)]
    ⎜   ↳ Virtual core XTEST pointer                id=4    [slave  pointer  (2)]
    ⎜   ↳ Logitech Optical USB Mouse                id=9    [slave  pointer  (2)]
    ⎜   ↳ TPPS/2 IBM TrackPoint                     id=14   [slave  pointer  (2)]
    ⎜   ↳ 10594 Pen                                 id=10   [slave  pointer  (2)]
    ⎣ Virtual core keyboard                         id=3    [master keyboard (2)]
        ↳ Virtual core XTEST keyboard               id=5    [slave  keyboard (3)]
        ↳ Power Button                              id=6    [slave  keyboard (3)]
        ↳ Video Bus                                 id=7    [slave  keyboard (3)]
        ↳ Sleep Button                              id=8    [slave  keyboard (3)]
        ↳ Integrated Camera                         id=12   [slave  keyboard (3)]
        ↳ AT Translated Set 2 keyboard              id=13   [slave  keyboard (3)]
        ↳ ThinkPad Extra Buttons                    id=15   [slave  keyboard (3)]

    nick@gimli:~$ xinput test 10
    motion a[0]=25003 a[1]=15786 
    motion a[0]=25007 a[1]=15774 
    motion a[0]=25012 a[1]=15746 
    motion a[0]=25020 a[1]=15708 
    motion a[0]=25029 a[1]=15665 
    motion a[0]=25036 a[1]=15622 
    motion a[1]=15581 
    motion a[0]=25035 a[1]=15547 
    motion a[0]=25028 a[1]=15521 
    motion a[0]=25015 a[1]=15506 
    motion a[0]=24994 
    motion a[0]=24964 a[1]=15521 
    motion a[0]=24927 a[1]=15551 
    motion a[0]=24879 a[1]=15597 
    motion a[0]=24824 a[1]=15662 
    motion a[0]=24763 a[1]=15748 
    motion a[0]=24695 a[1]=15853 
    motion a[0]=24627 a[1]=15964 
    ^C

Look at the output and try to figure out if it makes sense. If something looks
not right, like wrong axes reported for pen movement, or wrong scan codes
reported for button presses, then report your problem at bugtracker of your
Linux distribution (you know where to find it, right?). If that fails, report
your problem at https://bugs.freedesktop.org/

If everything looks right, move on to the next section.

Applications
------------
If you see problems with your tablet in all applications you try, then you
need to check previous sections. Otherwise, make sure your application is
configured correctly. Although not strictly necessary, many applications
provide interface for configuring tablet usage and sometimes that needs
adjusting. Look at the corresponding application configuration and
documentation and check our (now rather outdated, but still useful)
[application setup HOWTOs][app_howtos].

![GIMP input setup dialog](gimp_input_setup.png)

Some applications even provide a way to test the way they understand your
tablet, such as MyPaint. That can help you.

![MyPaint menu option for input device testing](mypaint_input_test.png)

If you still can't figure it out, check if you only get problems in
applications using the same toolkit, i.e. GTK or Qt.  Both of them had issues
with tablet support in the past and that can happen again, especially since
most people test with Wacom tablets only. If you can confirm that, report the
issue with the corresponding toolkit to your Linux distribution bug tracker,
or if that fails, to the bug tracker of the specific toolkit.

Otherwise, report an issue with the specific application to your
distribution's bug tracker, or to the bug tracker of the application itself.

[virtualbox]: https://www.virtualbox.org/
[virt-manager]: https://virt-manager.org/
[tablets]: /tablets/
[KYE_EasyPen_i405X]: /tablets/KYE_EasyPen_i405X/
[Huion_H610]: /tablets/Huion_H610/
[app_howtos]: /support/#application-setup
