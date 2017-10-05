---
title: Tablet setup in GIMP
summary: How to setup a tablet to work with The GIMP
---
> GIMP is an acronym for GNU Image Manipulation Program. It is a freely
> distributed program for such tasks as photo retouching, image
> composition and image authoring.
>
> It has many capabilities. It can be used as a simple paint program, an
> expert quality photo retouching program, an online batch processing
> system, a mass production image renderer, an image format converter,
> etc.
>
> GIMP is expandable and extensible. It is designed to be augmented with
> plug-ins and extensions to do just about anything. The advanced
> scripting interface allows everything from the simplest task to the most
> complex image manipulation procedures to be easily
> scripted.
>
> — [GIMP Introduction](http://www.gimp.org/about/introduction.html)

In this tutorial you can learn how to setup a tablet, which uses the
evdev driver, to work with this application.

GIMP 2.8
========

This version has been released on the 3rd of May 2012. This is the
latest stable version of GIMP which is the default version in many new
Linux distributions.

> This version of GIMP is equipped with a wealth of new features,
> including some highly requested ones. Keep reading to find out exactly
> what GIMP 2.8 has to offer you in areas such as the user interface,
> tools, and plug-ins.
>
> — [GIMP 2.8 Release Notes](http://www.gimp.org/release-notes/gimp-2.8.html)

Tablet setup
------------

In order to make your tablet work in this application you should
configure it first. Select *Edit\>Input devices* or *Edit\>Preferences*
from the menu, on the appearing dialogue select *Input Devices* and then
click on *Configure Extended Input Devices*. On the **Configure Input
Devices** dialogue select your device and set the *Mode* to *Screen*.

![GIMP 2.8 Configure Input Devices dialogue to set up your
pen](28lucky.png "GIMP 2.8 Configure Input Devices dialogue to set up your pen")

Device status
-------------

You can see the status of your devices on the **Device Status**
dialogue. To see it select *Windows\>Dockable Dialogues\>Device Status*
from the menu. In this window you can see the tool, foreground colour,
background colour, brush, pattern and gradient assigned to the enabled
devices.

![GIMP 2.8 Device Status dialogue to keep track of the settings for your
devices](28devstatus.png "GIMP 2.8 Device Status dialogue to keep track of the settings for your devices")

Paint dynamics
--------------

In 2.8 the brush dynamics engine has been expanded considerably, making
almost all aspects of the brush engine drivable by a multitude of
inputs, all of them configurable with their own response
curve.\<sup\>[2]\</sup\> The built in pressure curves are not editable,
but you can create new pressure curves which fit you more. Select
*Windows\>Dockable Dialogues\>Paint Dynamics* from the menu. On the
dialogue click on the new dynamics icon. On the **Paint Dynamics
Editor** you can select what attributes will be affected by the pressure
of your pen. For example, you can set the opacity curve to be different
than linear (see the image bellow).

![GIMP 2.8 Pressure curve
editing](28devcurve.png "GIMP 2.8 Pressure curve editing")

Configuring multiple input devices
----------------------------------

I have used Genius Mousepen i608X in the examples. This tablet comes
with a mouse. You may notice that on the **Configure Input Devices**
dialogue there is only one device, and that is the pen. Well, that was a
bit of luck, actually. It could happen that, as you plug and unplug your
device, only the mouse appears on this dialogue.

![GIMP 2.8 Configure Input Devices dialogue with only the
mouse](28wrong.png "GIMP 2.8 Configure Input Devices dialogue with only the mouse")

Actually this is a bug in GIMP. You can find the bug report
[here](https://bugzilla.gnome.org/show_bug.cgi?id=674253). At the
moment, the only solution is using a custom-built GIMP. You can download
the source [here](http://www.gimp.org/downloads/).

If you comment out the first "if" branch in
*gimp\_device\_manager\_device\_added* function in
*app/widgets/gimpdevicemanager.c*, like this:

`static void`\
`gimp_device_manager_device_added (GdkDisplay        *gdk_display,`\
`                                 GdkDevice         *device,`\
`                                 GIMPDeviceManager *manager)`\
`{`\
` GIMPDeviceManagerPrivate *private = GET_PRIVATE (manager);`\
` GIMPDeviceInfo           *device_info;`\
\
` device_info =`\
`   GIMP_DEVICE_INFO (gimp_container_get_child_by_name (GIMP_CONTAINER (manager),`\
`                                                       device->name));`\
\
` /*if (device_info)`\
`   {`\
`     gimp_device_info_set_device (device_info, device, gdk_display);`\
`   }`\
` else*/`\
`   {`\
`     device_info = gimp_device_info_new (private->gimp, device, gdk_display);`\
\
`     gimp_container_add (GIMP_CONTAINER (manager), GIMP_OBJECT (device_info));`\
`     g_object_unref (device_info);`\
`   }`\
`}`

For Ubuntu 12.04 you can find the building instruction
[here](http://www.gimpusers.com/tutorials/compiling-gimp-for-ubuntu).

Then, after building the modified GIMP, you can configure both of your
devices:

![GIMP 2.8 Configure Input Devices dialogue both
devices](28good.png "GIMP 2.8 Configure Input Devices dialogue both devices")

**NOTE**: This modification could make GIMP unstable. Use this advice at your
own risk.

GIMP 2.6.12
===========

This version has been released on the 1st of February 2012. This version
of GIMP is the default in many older distributions, like Ubuntu 12.04.

Tablet setup
------------

In order to set up your tablet, select *Edit\>Preferences* from the
menu. On the appearing dialogue select *Input Devices* and on the right
side click on *Configure Extended Input Devices*. On the **Configure
Input Devices** dialogue select your device and set *Mode* to *Screen*.
The following image illustrates these steps.

![GIMP 2.6.12 Configure Input Devices dialogue to set up your
pen](2612inputdev.png "GIMP 2.6.12 Configure Input Devices dialogue to set up your pen")

For more information please visit the GIMP User Manual ([Chapter 11.
Pimp my
GIMP](http://docs.gimp.org/2.6/en/gimp-pimping.html#gimp-prefs-input-devices)).

Device status
-------------

You can check the tool, foreground colour, background colour, brush,
pattern and gradient assigned to the enabled devices on the **Devices**
dialogue. In order to display it select *Windows\>Dockable
Dialogues\>Device status* from the menu.

![GIMP 2.6.12 Device status
dialogue](2612devicestatus.png "GIMP 2.6.12 Device status dialogue")

For more information please visit the GIMP User Manual ([5.2. Device
Status
Dialog](http://docs.gimp.org/2.6/en/gimp-device-status-dialog.html)).

Brush dynamics
--------------

In this version of GIMP the brush dynamics configuration is very
limited, compared to version 2.8. You can only set what kind of brush
property is affected by your pen's pressure. In order to do this, select
a tool, for example the pencil tool, in the **Tool Options**. Open the
*Brush Dynamics* area by clicking that little triangle icon next to the
text. Within the *Brush Dynamics* area you can set the pressure to
affect the opacity, size or the colour of the brush by clicking the
appropriate check boxes.

![GIMP 2.6.12 Setting up brush
dynamics](2612brushdynamics.png "GIMP 2.6.12 Setting up brush dynamics")

For more information please visit the GIMP User Manual ([3. Paint
Tools](http://docs.gimp.org/2.6/en/gimp-tools-paint.html#gimp-tools-paint-options)).

More information
================

You can find more information about GIMP on the [official
website](http://www.gimp.org/).
