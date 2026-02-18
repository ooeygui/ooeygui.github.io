---
layout: post
title: "Sources and Specs Posted"
date: 2008-02-25 00:12:54
categories: [Fabr]
wordpress_id: 63
redirect_from: /?p=63
---

I posted the Sketchups for the 3D printer, the motor controller board and what I have of the decomposer plugin. You can fetch the sources here:```
svn co https://ooeygui.com/Arduino .
```
The Decomposer doesn't work yet. I had been working under the assumption that Sketchup performed true volume intersections. Well, it doesn't - it only performs edge intersections. If you attempt to intersect a voxel in the middle of the model, there's nothing generated. This requires a minor change in plans. My original discussion mentioned a slice, then a strip then a voxel, but I tried to short circuit it in code; my mistake.Something for another night.
