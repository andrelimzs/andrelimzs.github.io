---
title: CNC Mill V3
author: andrelimzs
date: 2020-06-15T07:05:57+00:00
showToc: true
math: true
draft: false
---

{{< figure src="V9_1_sidebyside_1.png" width=720 align="center" >}}

The parts are going to take a while to arrive, so in the meantime I'm refining my design again. I went from a XY-Z configuration to Y-XZ. This frees up space in both the x and z axes and should make it easier to put additional reinforcement as well as a future 4th axis.

The y-axis table also extends out of the frame, which will make it easier to load/unload the machine.

## Reinforcing the X-axis

{{< figure src="x_torsion_1.jpg" width=480 align="center" >}}
For the x-axis, a force on the spindle (red arrow)

{{< figure src="x_torsion_2.jpg" width=480 align="center" >}}
Results in a moment about the z-axis (blue arrows)

{{< figure src="x_torsion_3.jpg" width=480 align="center" >}}
Which depends on the polar moment of area on the XY-plane (green shaded)

## Reinforcing the Y-axis

{{< figure src="y_torsion_1.jpg" width=480 align="center" >}}
A force on the y-axis (red arrow)

{{< figure src="y_torsion_2.jpg" width=480 align="center" >}}
Results in a moment (blue arrow)

{{< figure src="y_torsion_3.jpg" width=480 align="center" >}}
Which depends on the polar moment of area in the YZ-plane (green shaded)

## Front Column

{{< figure src="Front_spine.jpg" width=480 align="center" >}}
Both x- and y-axis benefit from adding a second column in the front.

It more than triples the polar moment of area, if some assumptions are met (joints, rails & ballscrews are significantly stronger than the extrusion etc).

## FEA Simulation

I started using the "trend tracker" feature in solidworks. It's convenient and a real time saver.

{{< figure src="image-4.png" width=480 align="center" >}}

Adding the front column led to the most significant improvement, from 150 μm to 65 μm.

Adding the 5mm triangle/L-shaped plates dropped it further to 20 μm. The difference between aluminum (6061) and stainless steel (304) was negligible.

For the second study I changed the fixtures because the model was becoming numerically unstable. The table is still _fixed_ and the force is still a _remote load_ acting on the spindle mount but I added an _elastic support_ on the base to simulate the effects of gravity/the ground.
Changing the fixture immediately dropped deflection from 20 μm to 8 μm. It makes sense that the weight of the machine will help anchor it to the ground.

{{< figure src="image-5.png" width=480 align="center" >}}

Adding a second horizontal beam between front and back columns halved the deflection, to 4 μm.