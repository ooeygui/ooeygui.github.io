---
layout: post
title: "Arduino for TextMate 2.0 (coming soon)"
date: 2008-11-22 13:41:17
categories: [Random, 3D Printing]
wordpress_id: 178
redirect_from: /?p=178
---

I've been working on an update to the 'microcontroller' plugin for TextMate. I'm putting the finishing touches on the release, and want it to bake. So far, it has the following changes:

-  Dropped the Make Controller and renamed to Arduino. This was done because I use Arduino exclusively, The Make controller doesn't work on Leopard (without some nasty hacks) and it complicated the code-base.

-  The Serial monitor was rewritten. It is now a cocoa console app, uses cocoa remoting to allow the TextMate plugin to control the serial monitor - such as releasing the device during a compile and upload or changing the port speed

- Support for multiple port speeds

- Support for multiple Arduino (Arduinos? Arduinii? more than 1 Arduino) 

- [Sanguino support](http://sanguino.cc/)

- Stability improvements and bug fixes

- Independence from Arduino releases

- Exclusive support for the Objective Development AVRMacPack




Further, I'll be creating a top level page for the plugin, and maybe releasing an ANSI graphics update to the arduino sources - allowing the arduino to provide rich graphics via the console.
