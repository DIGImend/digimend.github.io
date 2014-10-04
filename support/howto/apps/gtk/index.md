---
title: Tablet setup in GTK-based applications (Gimp, Mypaint)
---
GTK
---

GTK (or gimp toolkit) is a toolkit for creating graphical user
interfaces. It’s homepage is here: <http://www.gtk.org/>. The two of the
most popular drawing applications, Gimp and MyPaint, based on this
library. In this tutorial you can learn how to setup your tablet, which
uses the evdev driver, working in these two programs.

Gimp
----

The Gimp is the GNU image manipulation program. It’s homepage is here:
<http://www.gimp.org/>.

### Gimp 2.8

This is latest version of Gimp which is the default version in many new
Linux distributions. In order to make your tablet work in this
application you should configure it first. Select *Edit\>Input devices*
from the menu or *Edit\>Preferences* from the menu on the appearing
dialogue select Input Devices and then click on Configure Extended Input
Devices. On the Configure Input Devices dialogue select your device and
set the 'Mode' to 'Screen'.

![Gimp 2.8 Configure Input Devices dialogue to set up your
pen](../gimp28lucky.png "Gimp 2.8 Configure Input Devices dialogue to set up your pen")

#### Device status

You can see the status of your devices on the Device Status dialogue. To
see this select *Windows\>Dockable Dialogues\>Device Status* from the
menu. On this window you can see the tool, foreground color, background
color, brush, pattern and gradient assigned to the enabled devices.

![Gimp 2.8 Device Status dialogue to keep track of the settings for your
devices](../gimp28devstatus.png "Gimp 2.8 Device Status dialogue to keep track of the settings for your devices")

#### Pressure curves

The built in pressure curves are not editable, but you can set create
new pressure curves which fit you more. Select *Windows\>Dockable
Dialogues\>Paint Dynamics* from the menu. On the dialogue click on the
new dynamics icon. On the Paint dynamics editor you can select what
attributes will be affected by the pressure of your pen and for example
you can set up the opacity curve to be different than linear. (See the
image bellow.)

![Gimp 2.8 Pressure curve
editing](../gimp28devcurve.png "Gimp 2.8 Pressure curve editing")

#### Configuring multiple input devices

On the examples I have used Genius Mousepen i608X. This pen comes with a
mouse too. You may notice that on the Configure Input Devices dialogue
there is only one device, and that is the pen. Well, that was a bit
lucky actually. It could happen that, as you plug and uplug your device,
on this dialogue only the mouse appears.

![Gimp 2.8 Configure Input Devices dialogue with only the
mouse](../gimp28wrong.png "Gimp 2.8 Configure Input Devices dialogue with only the mouse")

Actually this is a bug in gimp you can find the bug report here:
<https://bugzilla.gnome.org/show_bug.cgi?id=674253>. At the moment the
only solution is using a custom built gimp. You can download the source
here: <http://www.gimp.org/downloads/>

If you modify the gimp-2.8.0 source/app/widgets/gimpdevicemanager.c file
according to this: in line 312 starts the
gimp\_device\_manager\_device\_added function, comment out the first
part of the if statement like this:

`static void`\
`gimp_device_manager_device_added (GdkDisplay        *gdk_display,`\
`                                 GdkDevice         *device,`\
`                                 GimpDeviceManager *manager)`\
`{`\
` GimpDeviceManagerPrivate *private = GET_PRIVATE (manager);`\
` GimpDeviceInfo           *device_info;`\
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

And after you have compiled gimp you can configure both of your devices:

![Gimp 2.8 Configure Input Devices dialogue both
devices](../gimp28good.png "Gimp 2.8 Configure Input Devices dialogue both devices")

Gimp 2.6.12
-----------

In order to set up your pen select *Edit\>Preferences* from the menu. On
the appeared dialogue select Input Devices and then on the right click
on configure extended input devices. On the Configure Input Devices
select your device and set mode to 'Screen'. The following image
illustrates these steps.

![Gimp 2.6.12 Configure Input Devices dialogue to set up your
pen](../gimp2612inputdev.png "Gimp 2.6.12 Configure Input Devices dialogue to set up your pen")

#### Device status

You can check the tool, foreground color, background color, brush,
pattern and gradient assigned to the enabled devices on the Devices
dialogue. In order to display it select *Windows\>Dockable
Dialogues\>Device status* from the menu.

![Gimp 2.6.12 Device status
dialogue](../gimp2612devicestatus.png "Gimp 2.6.12 Device status dialogue")

#### Brush dynamics

You can set up what kind of brush property is affect by your pen's
pressure. In order to do this select a tool, for example the pencil
tool, on the Tool Options open the Brush Dynamics area, by clicking that
little triangle icon next to the text. Within the Brush Dynamics area
you can set the pressure affect the opacity, size or the color of the
brush by clicking the appropriate checkboxes.

![Gimp 2.6.12 Setting up brush
dynamics](../gimp2612brushdynamics.png "Gimp 2.6.12 Setting up brush dynamics")

MyPaint
-------

My paint is a simple and easy painter program. It's homepage is here:
<http://mypaint.intilinux.com/>.

In order to set up your device choose *MyPaint\>Help\>Debug\>GTK input
device dialog* from the menu. On the Input dialog select your device and
set the mode to 'Screen'. The following image illustrates the settings.

![MyPaint 1.0 GTK input device
dialogue](../mypaint_setup.png "MyPaint 1.0 GTK input device dialogue")

#### Pressure curve

In order to change the pressure curve used by your devices choose
*MyPaint\>Edit\>Preferences* from the menu. On the Preferences dialogue
change the curve as you wish.

![MyPaint 1.0 Preferences
dialogue](../mypaint_curve.png "MyPaint 1.0 Preferences dialogue")

#### Device testing

After you have set up your device, you can test if it is works or not.
In order to do that select *MyPaint\>Help\>Debug\>Test input devices*
from the menu. Start to draw something on the canvas and you can see on
the Input Device Test dialogue how the pressure and other data of the
pen changes.

![MyPaint 1.0 Input Device Test
dialogue](../mypaintdevicetest.png "MyPaint 1.0 Input Device Test dialogue")

