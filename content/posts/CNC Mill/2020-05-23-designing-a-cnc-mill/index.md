---
title: Designing a CNC Mill
author: andrelimzs
date: 2020-05-23T13:03:54+00:00
showToc: true
draft: false
---
## Overview {#overview-438ad672-d0bb-438c-9385-532688c7dddb}

I&#8217;ve been looking at getting a CNC mill to machine small, strong, metal parts. Initially I looked at the chinese mills such as the 3018 (&#36;400). But they have a reputation of not lasting very long. Then I moved on to the Shapeoko 3 (&#36;1550) and OpenBuilds C-Beam (&#36;1200), but they&#8217;re too big to fit comfortably in an apartment. The Carbide3D Nomad (&#36;2750) would be perfect. It is rigid, compact and easily enclosed.

{{< figure src="V5_5_front_thumbnail.png" width=360  align="center" >}}

I decided that this would be a great chance to learn and design my own CNC mill.

### Design Objectives
- Rigid, to cut aluminium
- Enclosed and quiet
- Minimal fabrication, using as many off-the-shelf parts as possible
- Compact, to fit in an apartment



### Key Parameters

- Work area: 200mm x 200mm x 100mm
- HGR20 Linear Rails
- 2x HGR20 linear rails for each axis
- SFU1204 Ballscrews with NEMA 23
- 1500W air-cooled Spindle



### Summary

  1. Aluminum profiles bolted together
  2. Linear rails and ball screws for all axes



## Finite Element Analysis

I used FEA simulation to get an idea of the strain and deformation that I will see in the frame. The fact that the extrusions are bolted together complicates things, because it cannot be modeled as one large, &#8216;bonded&#8217; component.

To approximate the fasteners, I set

1. The extrusions to not interact with each other
2. The gussets and connectors to &#8216;bonded&#8217; with every other component



### X-Axis

{{< figure src="X_stress.jpg" width=360  align="center" >}}



### Y-Axis

  {{< figure src="Y_stress.jpg" width=360  align="center" >}}



Simulations were done along the x and y axes with a force of 10N.
