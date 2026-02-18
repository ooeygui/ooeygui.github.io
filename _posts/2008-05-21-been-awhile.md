---
layout: post
title: "Been awhile"
date: 2008-05-21 00:18:30
categories: [Electronics, Fabr]
wordpress_id: 95
redirect_from: /?p=95
---

It has been awhile since I've posted an update. I've been working on numerous projects, all at various stages. I figured I'd post an update across the board with no real conclusions.



### Sources


Sources have been posted on:

```
svn co https://ooeygui.com/Arduino .
```




But now I have a [Trac server](http://trac.edgewall.org/) installed at [https://ooeygui.com/trac](https://ooeygui.com/trac).




This server allows people to post bugs, read design docs, and see changes live. Enjoy!



### TextMate Microcontroller plugin


While working on the gcode interpreter, I discovered some issues with the plugin. I'm in the process of rewriting portions of the plugin. The plugin now uses [AVR MacPack](http://www.obdev.at/products/avrmacpack/index.html) instead of the Arduino packaged compilers. This isolates the plugin from breaking changes by the Arduino developers. I'll release new packages as new Arduino packages are released.



### Fabr Hardware


I had completed the Axes, but noticed a nasty binding problem. After disassembling, I discovered part of the drive assembly wasn't square, which caused a bind when spinning. A second problem that was bugging me was related to an early cut on the X axis not being square to the Y axis...  Both are being corrected as I build the extruder mount.




The extruder is completely built, but untested. I leveraged the extruder head that was featured in a previous post, added a cooling pipe and an insulated feed pipe in order to prevent premature melting of the extrusion cord. I'm currently constructing a mounting plate which will allow me to swap print heads relatively quickly (as well as allow me to mount the current one).



### xstrudr


I have 25lbs of bioplastic granules; not terribly useful for a printer. I had considered building a hopper on the printer and extruding the granules directly to the print job, but couldn't make it practical. Instead, I'm going to have a separate unit which processes granulated plastic into a cord, and yet another unit for grinding plastic into granules ('chipr'?)



### Motor Controller


After the experience with the triple axis controller, I'm starting to design a single purpose motor controller; I find that I need 4 controllers - one for each axis and one for the extruder.  Here's a preview: [![](/assets/images/2008/05/picture-2-150x150.png)](/assets/images/2008/05/picture-2.png).


I still plan on having an Arduino Shield to tie 3 or 4 axes together.


