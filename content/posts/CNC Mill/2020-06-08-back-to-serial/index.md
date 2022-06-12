---
title: Back to Serial
author: andrelimzs
date: 2020-06-08T17:24:57+00:00
url: /back-to-serial/
neve_meta_enable_content_width:
  - off
categories:
  - CNC Machine
showToc: true
math: true
draft: false
---
## Tripteron

{{< figure src="V2_2_front.jpg" width=480 align="center" >}}

The Tripteron is a 3 DoF parallel cartesian robot. It is _decoupled_, which means each axis can be controlled independently and _isotropic,_ which means it's dexterity is unchanged throughout the workspace.

## Practical Limitations

But there are some serious obstacles to building a parallel CNC as a first machine. It needs many precise, high strength joints. Precise because any backlash gets magnified by the length of the chain. Strong because the length also results in a large moment.

{{< figure src="10300374620.jpg" width=280 align="center" >}}

Misumi has a side mounting hinge that will fit on standard 20 $\times$ 20 extrusion. They are 110 per set, for a total of 990 (the entire serial design cost 900). This definitely won'&#8217;'t be feasible unless I can machine them myself.

## Inherent Advantages?

It's not clear if a parallel mechanism has any inherent advantage in a 3-axis design. The axes are normally split:

  * XY table + Z spindle
  * Y table + XZ spindle

In either case two axes are well supported across their entire length, so only the last axis experiences any significant deflection. Compared to the parallel mechanism which has three chains which all experience deflection.

If we assume that the total length of each chain is the same as the serial design, then they must have a larger second/polar moment of area. If we stick to conventional extrusion profiles, the Tripteron will require **more** material to achieve the same stiffness.

## Conclusion

I might reexamine a parallel 2 DoF mechanism for the XY table in the future, after I have a working CNC mill. But for now it seems like my best bet is to stick with a conventional design, and maximize strength and rigidity.