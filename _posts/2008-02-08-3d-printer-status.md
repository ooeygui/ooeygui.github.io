---
layout: post
title: "3D Printer Status"
date: 2008-02-08 23:39:01
categories: [Fabr]
wordpress_id: 58
redirect_from: /?p=58
---

### Motor Control Board
The motor control board is currently "panelized", and being fabricated. I had expected it to be done last week, and started populating it this weekend. After I submitted it, I realized I could make a simple modification to the design which would enable the board to be switched into "high res" (microstep) or "high speed" (full step) mode. This is done by routing two control lines on each of the drivers to an output pin on the controller; a high signal on both lines is microstep, low is full step.I'm starting to look into building a 2.5 amp version of the board using the 44pin PLCC 3977 from [Allegro Micro](http://www.allegromicro.com/en/Products/Part_Numbers/3977/index.asp). They have been fantastic about supplying samples for development.### Extruder
Where, oh where, do I find plastic? Finding small quantities of HDPE or ABS in 1/8" cord or granules is difficult. I can get granules in 55 pound bags, for $2 per pound or ABS cord in 25 pound rolls at $7 per pound. The cheapest I've found is Village Plastic will offer 5 pounds of HDPE cord for $6 per pound.I've been looking into ways of simplifying the heating barrel and extruder screw. Instead of using a metal pitch screw, I think a concrete screw will be able to grip either the granules or the cord with equal efficiency.### Software
I've been playing with the Sketchup API. Still not sure how I'm going to slice the printing object. I've been looking at some code which generates STL files, so I should garner what I need.### Structure
I'll be ordering structure parts this weekend, and start assembling it next week.
