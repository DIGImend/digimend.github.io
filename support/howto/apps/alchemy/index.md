---
title: Tablet setup in Alchemy
summary: How to setup a tablet to work with Alchemy
---
> Alchemy is an open drawing project aimed at exploring how we can sketch,
> draw, and create on computers in new ways. Alchemy isn’t software for
> creating finished artwork, but rather a sketching environment that focuses
> on the absolute initial stage of the creation process.  Experimental in
> nature, Alchemy lets you brainstorm visually to explore an expanded range of
> ideas and possibilities in a serendipitous way.

— [Alchemy website](http://al.chemy.org/)

In this tutorial you can learn how to setup a tablet, which uses the
evdev driver, to work with this application.

Alchemy BETA 008
================

This is the latest version of this program. You can download it
[here](http://al.chemy.org/download/).

Tablet setup
------------

This software automatically recognises your tablet, there is no need to
configure anything in Alchemy.

If your tablet is not recognised, there might be a problem with loading
the JPen library.

To correct the problem do the following. Download the JPen library
(jpen-2-100101-lib.zip) from
[here](http://sourceforge.net/projects/jpen/files/jpen/2-100101/jpen-2-100101-lib.zip/download)
to \~/Downloads. \<ol\> \<li\> Open a terminal and change to the Alchemy
directory. \<br\> \<pre\>\$ cd Alchemy/\</pre\> \</li\> \<li\> Rename
the Alchemy's lib directory to libold.\<br\> \<pre\>\$ mv lib
libold\</pre\> \</li\> \<li\> Extract the jpen-2-100101-lib.zip into the
Alchemy directory.\<br\> \<pre\>\$ unzip
\~/Downloads/jpen-2-100101-lib.zip\</pre\> \</li\> \<li\> Create a new
lib directory under the Alchemy directory.\<br\> \<pre\>\$ mkdir
lib\</pre\> \</li\> \<li\> Copy libjpen-2-2-ia64.so, libjpen-2-2.so,
libjpen-2-2-x86\_64.so, libjpen-2-3.jnilib from the zip file into the
lib directory.\<br\> \<pre\>\$ cp -t ./lib
./jpen-2-100101/libjpen-2-2-ia64.so ./jpen-2-100101/libjpen-2-2.so \\
./jpen-2-100101/libjpen-2-2-x86\_64.so
./jpen-2-100101/libjpen-2-3.jnilib\</pre\> \</li\> \<li\> Copy
jpen-2.jar from the zip into the Alchemy installation directory.\<br\>
\<pre\>\$ cp -t . ./jpen-2-100101/jpen-2.jar\</pre\> \</li\> \<li\>
Start Alchemy with the following command.\<br\> \<pre\>\$ java
-Djava.library.path=lib -jar Alchemy.jar\</pre\> \</li\> \</ol\>

If you have successfully set up everything, the following message should
appear in the terminal: \<pre\>08-Jun-2012 15:27:03
jpen.provider.NativeLibraryLoader\$4 run INFO: loading JNI library:
jpen-2-2-x86\_64 ... 08-Jun-2012 15:27:03
jpen.provider.NativeLibraryLoader\$4 run INFO: jpen-2-2-x86\_64
loaded\</pre\> Which means the JPen library loaded correctly and your
tablet should be recognised.

Device testing
--------------

If you choose *Pressure Shapes* in the *Create* menu, you can see how
pressure is affecting the shape of what you are drawing.

![Alchemy Beta 008 Selecting pressure shapes for
drawing](pressureshapes.png "Alchemy Beta 008 Selecting pressure shapes for drawing")

More information
================

You can find more information about Alchemy on the [official
website](http://al.chemy.org/).

If you have any questions or suggestions according to this article
please send an email to the [DIGI*mend* users mailing
list](mailto:digimend-users@lists.sourceforge.net).
