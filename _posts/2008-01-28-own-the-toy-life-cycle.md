---
layout: post
title: "Own the toy life-cycle..."
date: 2008-01-28 00:20:28
categories: [Fabr]
wordpress_id: 56
redirect_from: /?p=56
---

Just before my son's 4th birthday, my wife and I purged some of his toys. We looked for things he didn't play with any more, stuff our younger son wasn't interested in, broken toys, and toys with missing parts. The amount of stuff we found was staggering. What we could - we donated; what we couldn't donate we removed non-recyclable parts and recycled the rest - but we still ended up with lots of stuff that went right into the landfill. There has got to be a better way. With the recent spat of recalls related to lead or dangerous products, I wanted to be more aware of the life cycle of the toys my kids play with.	- What if you could make your own toys?
	- And have the ability to directly recycle them into new toys when the children get bored or break them?
	- What if you could share your toy designs with a community?

These thoughts have driven me to start designing and building my own 3D printer. I've been following the RepRap dev team with enthusiasm, but wanted to take a slightly different approach to the problem. The RepRap's goal is to achieve self replication - but in order to build your own RepRap, at the moment you need access to a real 3d printer.I designed Fabr as a 3D printer which uses commonly available parts, requires minimal part fabrication, is highly accurate, and has enough power to not only extrude plastic, but can be used to mill metal, wood or plastic.Here are some of Fabr's key components:	- 80/20 extruded aluminum Bars and Fasteners (purchased from [the 80/20 Garage Sale on eBay](http://stores.ebay.com/8020-Inc-Garage-Sale))
	- Anti-backlash nuts and drive Couplers from [DumpsterCNC](DumpsterCNC.com)
	- [](https://www.jameco.com/webapp/wcs/stores/servlet/ProductDisplay?storeId=10001&langId=-1&catalogId=10001&productId=162027) from Jameco
	- Screws, bearings, and aluminum bars from [McMaster-Carr](http://www.mcmaster.com/)
	- Timing belts, and Pulleys from [SmallParts](http://www.smallparts.com/)
	- A custom stepper motor controller board based on [EasyDriver3](http://greta.dhs.org/EasyDriver/)
	- Interfaces with [Sketchup](http://sketchup.google.com/)

Here's a Sketchup of the latest design:
[![Fabr](/assets/images/2008/01/fabr2.thumbnail.png)](/assets/images/2008/01/fabr2.png)  
I've been developing an arduino shield based on the EasyDriver which uses an [Allegro 3967 Microstepping stepper motor controller](http://www.allegromicro.com/en/Products/Part_Numbers/3967/index.asp). Essentially the board has 3 drivers, and connectors for end stops. The first board is currently being made at [Batch PCB - an offshoot of SparkFun](batchpcb.com); I expect it any day now. Once complete, I'll solder it up, and run it through its paces. I'm looking into using the 3977 which would allow a 2.5 amp motor - a project for another time.
![Arduino Shield](/assets/images/2008/01/controller.thumbnail.png)
The next part of the project is the extruder. I'm currently determining if I want to route a cord, or have a granule hopper directly on the extruder head. Stay tuned.  �And yes, it has a funky Web 2.0 name - domains are hard to come by.... Fabr.org currently redirects to ooeygui.com.
