---
title: Improving Rigidity
author: andrelimzs
date: 2020-06-10T07:21:37+00:00
showToc: true
math: true
draft: false
---

{{< figure src="V7_9_no_enclosure.png" width=480 align="center" >}}

When we last left off we managed to get 20 μm, I'm going to try to improve on it.

## Spindle Brace

{{< figure src="02_110N_X.jpg" width=480 align="center" >}}

Adding

1. Stainless steel sheet metal (5 mm) brace between the spindle and rear column
2. Aluminum sheet (4 mm) at the bottom

reduces deflection (at 110 N) from 20 μm to 16 μm.

## Column Brace

{{< figure src="03_spindle_column_brace_X.jpg" width=480 align="center" >}}

Adding a 60 x 200 x 5 mm stainless steel plate reduces deflection from 16 μm to 14.3 μm.

## Custom Column Plate

{{< figure src="01_X.png" width=480 align="center" >}}

Removing the spindle brace, the deflection goes back up to 17.4 μm.
Each triangle plate from Misumi is $\$$40, and the custom plate replaces three of them. If the custom plates cost less than $\$$120, it'll be a good deal.

## Combine Column and Triangle Plate

{{< figure src="image-3.png" width=480 align="center" >}}

Goes down slightly to 17.1 μm, and it'll save $\$$80.

## Enclosure

{{< figure src="V7_9_enclosure_open.png" width=480 align="center" >}}

The enclosure will

  1. Keep the noise down
  2. Contain the mess

People have managed to get their Shapeoko and Nomad machines under 56dB ([Carbide3d Forum](https://community.carbide3d.com/t/nomad-and-so3-custom-enclosures-the-enclosure-zoo/1035/2)).

I have four options for the enclosure panels

  * Aluminum (3 mm) &#8211; $\$$106/pc
  * Polycarbonate (5 mm) &#8211; $\$$81/pc
  * Powder Coat Steel (2 mm, 8 hole) &#8211; $\$$72/pc
  * Stainless Steel (2 mm) &#8211; $\$$119

3 and 5 mm panels can be mounted inside the extrusions, everything else has to be bolted on. I'll need to investigate how that affects noise isolation.