---
title: How to locate a failure
summary: How to find the reason your tablet doesn't work
---
This guide will help you find the reason your tablet doesn't work under Linux
and who can help you fix it. You'll need to know how to minimally use Linux
command line. We'll start from the bottom.

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
To be done.

Userspace drivers
-----------------
To be done.

Applications
------------
To be done.

[virtualbox]: https://www.virtualbox.org/
[virt-manager]: https://virt-manager.org/
[tablets]: http://digimend.github.io/tablets/
[KYE_EasyPen_i405X]: http://digimend.github.io/tablets/KYE_EasyPen_i405X/
