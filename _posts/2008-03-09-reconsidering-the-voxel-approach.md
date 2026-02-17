---
layout: post
title: "Reconsidering the voxel approach"
date: 2008-03-09 09:44:41
categories: [Fabr]
wordpress_id: 74
redirect_from: /?p=74
---

My first approach at decomposing an object to voxels, then to generateÂ primitivesÂ from this "voxel space" may not be the best algorithm. I'm finding that I use edges to discover if a voxel is set, then duringÂ reconstitutionÂ need to find edges. I'm worried about curve detection - Sketchup has context if an edge is part of a curve. Not to mention that it takes an hour to "voxelize" a small simple object...I still believe that locating sub-volumes is the correct approach for the path generator, but moving forward I'm going to look into using the edges themselves not voxels. Â Stay Tuned.
