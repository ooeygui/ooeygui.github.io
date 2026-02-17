---
layout: post
title: "35 year old ROS Robot?"
date: 2020-07-12 21:09:33
categories: [Robotics]
tags: [Hero 1, ROS, Robot]
author: Lou
wordpress_id: 359
redirect_from: /?p=359
---

Meet Marty. Marty is a HeathKit ET-18 Educational HERO 1. ROS - the Robot Operating System - is an open source "system" for operating Robots. Can I ROS enable a 35 year old Robot?

(Full disclosure - I'm a Development Lead at Microsoft in the Edge Robotics team - working on ROS within the Core Operating Systems group. This project is not endorsed by or funded by Microsoft)

ROS enforces a "separation of concerns" - Sensors, Compute and Actuators are separated from each other using a "pub/sub" messaging model. 

The HERO 1 uses a Motorola 6808 Microprocessor with 4kb of RAM. This obviously won't run ROS or a modern operating system; but is it enough to run the motors and sensors?

Let's Try it!

But first, I need to get it up and running. 

![](/assets/images/2020/07/20200713_002314699_iOS-2-1024x590.jpg)Marty was built in October 1984 - when I was 9 years old.  Marty will be cheaper to restore than a Delorean...

![](/assets/images/2020/07/20200713_000358532_iOS-768x1024.jpg)Are those Eyes or Ears?

![](/assets/images/2020/07/20200713_001525484_iOS-1024x768.jpg)Something missing...

![](/assets/images/2020/07/20200713_002214880_iOS-768x1024.jpg)Check out this wheel encoder!

![](/assets/images/2020/07/20200713_002252983_iOS-1024x768.jpg)I think this is the end stop

![](/assets/images/2020/07/20200713_002341566_iOS-768x1024.jpg)Hmmm... I'm sure that shouldn't be like this...

![](/assets/images/2020/07/20200713_001445474_iOS-768x1024.jpg)Safety Third - That's some good [macgyvering ](https://www.urbandictionary.com/define.php?term=macgyvering)there. 

![](/assets/images/2020/07/20200713_002125285_iOS-1024x768.jpg)I'm pretty sure they aren't supposed to be this way...

![](/assets/images/2020/07/20200713_002000219_iOS-1024x768.jpg)1980s Ground Bar...

![](/assets/images/2020/07/20200713_001751781_iOS-768x1024.jpg)Yes, the wire is jammed into test points to change modes...

![](/assets/images/2020/07/20200713_002049330_iOS-1024x768.jpg)This looks aftermarket. Good plan...

For a 35 year old Education robot, I think it is in relatively good shape. I've ordered manuals and some upgrades - including a USB interface...

My plan: 

-  Clean all the contacts 

-  Fixup some of the solder blobs

-  The open fuses scare me. I'm going to replace them with closed fuses... 

- I'll do subsystem bringup, then work on the Arm...

Until Next time....


