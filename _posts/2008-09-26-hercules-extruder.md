---
layout: post
title: "Hercules Extruder"
date: 2008-09-26 21:53:40
categories: [Random, 3D Printing]
wordpress_id: 136
redirect_from: /?p=136
---

When I began working on an extruder, I spent quite a bit of time working on a screw based drive, attempting to engage the plastic in order to push it into the heater barrel. Turns out - plastic is slippery. I've seen lots of problems with screw based extruders, so I looked into using a pinch pulley. It works beautifully!


There are a few nice side effects of the pulley - The extruder is very simple, doesn't require much fabrication, and can be reversed instantly to stop plastic extrusion (there are no plastic fuzzies). Unfortunately, since I use a stepper, this extruder cannot be directly used by the reprap firmware without modification. I'm currently refactoring the reprap firmware to enable device abstraction which will allow either a stepper or dc motor.
 



[caption id="attachment_129" align="alignnone" width="180" caption="Hercules Extruder"][![Hercules Extruder](/assets/images/2008/09/herk-180x300.png)](/assets/images/2008/09/herk.png)[/caption]



[Download Sketchup](/assets/downloads/Herk.skp)






The **heating element** is 8 inches of insulated nichrome wire, purchased from [RepRap Research Foundation](http://rrrf.org) wrapped around an 1/8 inch brass plug. The brass plug was turned to a point (or filed to a pyramid). A .01" hole was drilled in the tip (very carefully!). The brass plug is threaded using a 1/4"-28 pitch tap. (The brass plugs from home depot are hollow, so it is easy to tap; otherwise it needs to be drilled out).

[![](/assets/images/2008/09/picture-11.png)](/assets/images/2008/09/picture-11.png)



This is a previous implementation with a brass coupler. The extra brass was acting as a heat sink which was causing problems keeping the extruder up to temperature.

[![](/assets/images/2008/09/img_6142-300x215.jpg)](/assets/images/2008/09/img_6142.jpg)




The **extension barrel** is a [0.375" OD x 0.12" WALL x 0.135" ID T-316 stainless steel tube](http://www.onlinemetals.com/merchant.cfm?pid=14839&step=4&showunits=inches&id=902&top_cat=1) from Online Metals. Half inch of the tube was turned down and threaded to mate with the heater element. I've been asked what the resulting heating characteristics are of the new barrel - I'm happy to say that the brass element holds temperature well, and the stainless does not conduct much heat (the top barely tops out at 115F) 




The Sketchup presented differs slightly from the implemented extruder. This was simply due to resource limitations (material on hand). The Sketchup includes a better mounting bracket for the heater barrel and uses a single piece of aluminum for the mounting wall.

[![](/assets/images/2008/09/img_6240-282x300.jpg)](/assets/images/2008/09/img_6240.jpg)





**Herk?** Before my father retired from the Air force, he flew a C-130 transport nicknamed Hercules. He currently uses the name "Herk" as his handle in various games. I named the extruder for him as a small form of thanks.


