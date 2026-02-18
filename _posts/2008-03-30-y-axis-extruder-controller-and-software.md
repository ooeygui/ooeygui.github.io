---
layout: post
title: "Y Axis, Extruder, Controller and Software"
date: 2008-03-30 15:10:37
categories: [Fabr]
wordpress_id: 84
redirect_from: /?p=84
---

### Y Axis


Shortly after building the Z axis, I began working on the Y Axis. I was being cheap and wanted to attempt to use a single drive screw instead of two (as planned) thinking that the 80/20 would be true enough to prevent racking. Once I actually hooked up the cross beam I found the racking to be pretty extreme.   �To counter this, I needed another drive screw and a timing chain instead of direct linkage; This meant another order from electric gold mine.   �I had decided to use chain instead of a timing belt for cost reasons. I may revisit this in V2 as it adds quite a bit of weight and is really loud.   �I should be able to get the Y Axis built this week and the X axis shortly after that.

### Extruder
I had some problems attempting to use a screw to feed plastic into the extruder. I tested a feed mechanism which uses two  �pulleys and was impressed with the results. In order to get a screw to engage the plastic quite a bit of side pressure is needed, which in turn requires quite a bit of torque to drive the screw. With the dual pulleys, a smaller stepper motor can be used because very little pressure is needed, which means less torque is needed for the same extrusion feed pressure.I was attempting to work with  �[Clayborn Labs](http://www.claybornlab.com/) to design a heater barrel. I need to follow up with them because my account seems to have gone into the ether... In the mean time, I'm going to be working with the electric gold mine "NASA heat tape".  �Smoke test passes!   �No smoke is passing right? I ran some simple tests designed to test each motor controller. I used LEDs instead of a motors because I didn't want to risk destroying them. It was wild to see the coil animation represented by lights. Next up is testing the end stops and building an on-board gcode interpeter.

### Software
I ran into some bugs in the TextMate plugin, so corrected them. You can  �[download the new version](/assets/downloads/Microcontroller.zip) and unzip to  �~/Library/Application Support/TextMate/Plugins. The Sketchup plugin is successfully decomposing objects into voxels. I've begun work on a recomposer which attempts to use the voxels by using various 'solid handlers' to attempt to generate sub-volumes. During this process I noticed that I was trying to locate the same edges I used to generate the voxels - and lamented that it may not be the best solution. In the degenerate 'cuboid' case, using voxels is dumb. However, as the complexity of the object increases using voxels will deterministically reduce the original volume to sub-volumes without requiring edge walking.I had wondered if using octrees would be better for dividing the volume into cuboids, but I think it may loose some context when adding other shape composers (like finding holes or  �cylinders)

