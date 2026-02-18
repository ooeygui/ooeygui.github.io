---
layout: post
title: "Drive Components"
date: 2008-08-10 14:22:57
categories: [Random, 3D Printing]
wordpress_id: 117
redirect_from: /?p=117
---

There are 4 stepper motors on the 3d printer: one for each axis and another for the extruder. I wanted to user a motor that could be powered with a computer power supply, was under 750ma (A3967 compatible), had a small step angle and maximized the torque. I evaluated many (too many) options, but settled on the 162027 from Jameco:



[![](/assets/images/2008/07/162027.jpg)](http://www.jameco.com/webapp/wcs/stores/servlet/ProductDisplay?langId=-1&storeId=10001&catalogId=10001&productId=162027)

This motor is 600ma, is compatible with the A3967, has a .25" spindle which fits without modification the sprockets from electric goldmine, and has plenty of torque to handle the





[![](/assets/images/2008/07/g13605t.jpg)](http://www.goldmine-elec-products.com/prodinfo.asp?number=G13605)  �  �  �  �  �  �  �  �  �  �  �  �  �  �  �  �  �  �

[![](/assets/images/2008/07/g13608t.jpg)](http://www.goldmine-elec-products.com/prodinfo.asp?number=G13608)

RepRap uses a toothed belt which engages plastic pulleys. The toothed belt and pulleys are fairly expensive and require precision gluing. I was intrigued by the idea of using ['ball chain'](http://blog.reprap.org/2008/04/ball-chain-update.html) and am considering milling custom sprockets eventually. In the mean time, Electronics Goldmine has rollerchain and sprockets for very cheap - $1.25 per sprocket, and $2.50 for 23" of mini-chain. I used 9 Sprockets and 5 sets of roller chain. They are fairly quiet and you can't really beat the price.







[![](/assets/images/2008/08/easydriverv3-01-l-150x150.jpg)](/assets/images/2008/08/easydriverv3-01-l.jpg)

[SparkFun.com](http://www.sparkfun.com/commerce/product_info.php?products_id=8368) sells a stepper driver based on the Allegro A3967. This chip is a microstepping stepper driver capable of driving a stepper up to 750ma. It requires lots of tuning to drive the stepper correctly (varying parameters such as step frequency, current limiting via a pot, etc). It is also subject to motor noise.


I've been working with Zach on from the [RepRap research foundation](rrrf.org) on a higher power stepper driver based on the A3977 which will drive 2.5a and is dip switch configurable for full step or microstep. You can follow the [project on SorceForge](http://reprap.svn.sourceforge.net/viewvc/reprap/trunk/users/lamadio/Stepper%20Driver/). Until that project is completed and available, this combo seems suitable for light duty FDM extrusion.


