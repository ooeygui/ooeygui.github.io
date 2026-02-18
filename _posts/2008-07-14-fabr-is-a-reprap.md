---
layout: post
title: "Fabr is a RepRap"
date: 2008-07-14 00:23:49
categories: [Electronics, Fabr, 3D Printing]
wordpress_id: 97
redirect_from: /?p=97
---

I became  �intrigued  �by the concept of 'renewable manufacturing' - owning the life-cycle of everyday things. This idealism was captured by the RepRap project; who's tag line is 'Wealth without money'.



In the early phases of the construction of Fabr, I ran into the classic RepStrapping problem - how do you build a device intended to be built using a 3D Printer without a 3D printer? I had attempted to use the printing services at the TechShop in Menlo Park, but was unsuccessful. After that failure, I decided to  �designed and build Fabr using commonly available materials and few custom parts.



Over the last year, the RepRap organization has made changes independently which  �amusingly coincide with some Fabr design decisions, and Fabr has changed to be more like the RepRap in order to better leverage the software and firmware from the RepRap team.



In  �essence, Fabr is a RepRap.



Since my last post, I've been working on the following parts of the project:



	- The TextMate plugin was nearly rewritten in order to remove the Processing toolchain, and require the AVR MacPack from Objective Development.


	- Using the new Mill & Lathe, I've improved some of the articulation points


	- Implemented a multi-screw Y axis to compensate for unacceptable racking. I wish I could say that 80/20 is an asset, but the linear motion bearings are woefully  �inadequate and  �excruciatingly  �expensive.  �


	- I smoked my motor shield while debugging a stepper problem, and switched to using 4 EasyDrivers from SparkFun.


	- Started building a 'Super Driver' which can drive steppers to 2.5A, as well as software configurable Full Step or Microstepping.


	- Ported the RepRap gcode interpreter to the TextMate toolchain and adapted it for the EasyDrivers (Need to unify this work with the trunk, and submit my updates to the RepRap team).


	- Implemented a Ruby gcode uploader; hopefully to be used in the Sketchup exporter.


	- Acquired materials and Building RepRap Opto Endstops for home positioning


	- Acquired materials and Building the Temperature controller for the extruder
