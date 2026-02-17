---
layout: post
title: "Retrofitting an EasyDriver"
date: 2009-03-05 23:51:43
categories: [Random, 3D Printing]
wordpress_id: 285
redirect_from: /?p=285
---

Brian Schmalz's easy driver, offered by SparkFun is a nice little stepper driver. There are two major deficiencies however: It is hard wired to eighth step mode, and the ground pin is opposite the signal pins. I decided to fix these as I reassemble the extruder.


The step mode on the A3967 is selected by pin 12 & pin 13. By bringing these pins low, we can select full step. This is achieved by clipping the pins from the pads and soldering a wire to ground - the same ground I wanted to route over to the signal pins. In order to seat the polarized header, I filed a pin slot and soldered the ground jumper.
[![Rev EasyDriver](/assets/images/2009/03/img_7058-300x169.jpg)](/assets/images/2009/03/img_7058.jpg)


