---
layout: post
title: "Stepper Motor Driver v2a"
date: 2008-07-23 22:03:32
categories: [Fabr, 3D Printing]
wordpress_id: 108
redirect_from: /?p=108
---

One of the side effects of using 80/20 for the structure of a 3d printer is that it weighs significantly more than the reprap design using rods and plastic joints. I became concerned that a 600ma Stepper may not drive that much weight or overcome the friction of the linear motion bearings. I started designing the 'super driver' which was capable of driving up to a 2.5 amp Stepper which would easily handle those issues.



After Fabr's appearance on [Hackaday](http://www.hackaday.com/2008/07/13/fabr-another-3d-printing-project/), Zach from the [RepRap research foundation](http://rrrf.org) contacted me about collaborating; we were going down parallel paths. I deeply admire the foundation's work, and Zach's contributions seriously rock. Needless to say I was very enthusiastic about this collaboration.



Zach was interested to see if we could use the 'super driver' as the next generation stepper driver board for the RepRap, and offered suggestions for unifying the projects.



The first iteration of this collaboration has been checked into the [RepRap source forge project](http://sourceforge.net/projects/reprap/) (and the fabr subversion project will be retired).  It is looking pretty sweet!



Here's the start of the new board:

[![](/assets/images/2008/07/smd2a-237x300.png)](/assets/images/2008/07/smd2a.png)



Notable Differences:



	- License changed to GPL


	- Name change (no more super driver)


	- Uses the RepRap standard IDC header for communication


	- Uses the RepRap standard power supply connector (vertical instead of horizontal)


	- Min/Max end stops route through the motor controller using Headphone Jacks (to be implemented)


	- Uses the new RepRap .156" headers for the motor connector (similar to the ones I was using, but non-removable and less expensive)


	- Stepper mode (full/eighth) is a jumper instead of software settable


	- Opto-isolated (to be implemented)


	- SR is jumper configurable (needs the diodes to be completed)
