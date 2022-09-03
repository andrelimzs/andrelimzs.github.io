---
title: 'Inverse Kinematics, Velocity & Acceleration Analysis'
author: andrelimzs
date: 2020-05-24T16:27:03+00:00
showToc: true
math: true
draft: false
---
## Overview

### Notation

$$
\begin{aligned}
    &\Theta &&: \text{Joint velocities} \cr
    &\Theta_{a/p} &&: \text{Active/Passive} \cr
    &W &&: \text{Twist} \cr
    &V &&: \text{Velocity of end-effector} \cr
    & \Omega &&: \text{Angular Velocity} \cr
    & X &&: \text{Generalized coordinates}
\end{aligned}
$$

{{< figure src="3rpr_planar_robot_schematic-1.png" width=360 align="center" >}}

### Outline

**Inverse Kinematics** : Relates position

**Inverse Jacobian** : Relates velocities (in generalized coordinates)

**Inverse Kinematic Jacobian** : Relates velocities (in cartesian coordinates)

## Inverse Kinematics

Inverse kinematics are a way of getting joint coordinates from a desired end-effector pose. For a $3{\text -}RPR$ planar robot it is given by

$$
\begin{cases}
	\rho_1^2 = &x^2 + y^2 \cr
	\rho_2^2 = &(x+l_2 \cos({\Phi}) - c_2)^2 + (y+l_2 \sin({\Phi}))^2 \cr
	\rho_2^2 = &(x+l_3 \cos({\Phi + \theta}) - c_3)^2 + (y+l_3 \sin({\Phi + \theta}) - d_3)^2
\end{cases}
$$


## Inverse Jacobian

The joint coordinates and generalized coordinates are related by

$$
A \dot\bold \Theta_a + B\dot\bold X + C \dot\bold \Theta_p = 0
$$

For the _minimal kinematic set_ (MKS), where the joint velocities $\dot X$ are reduced to the active joints $\dot \Theta_a$ , we can ignore $\dot \Theta_p$ and get

$$
A \dot \bold \Theta_a + B \dot \bold X = 0
$$

If $B^{-1}A$ is invertible,

$$
\dot \Theta_a = \underbrace{-A^{-1} B}_{J^{-1}}\ \dot\bold X
$$

The inverse jacobian is given by

$$
J^{-1} = -A^{-1}B
$$

## Inverse Kinematic Jacobian 

The inverse kinematic jacobian $J_k^{-1}$ relates joint velocities to end-effector velocities. For the MKS

$$
A \dot \bold \Theta_a + B
\ \underbrace{
	H^{-1} \bold W 
}_{\dot\bold X} = 0
$$

$J_k$ is a transformation from generalized coordinates to cartesian twist.  
The inverse kinematic jacobian is therefore

$$
\begin{aligned}
	J_k^{-1} &= -A^{-1} B H^{-1} \cr
	&= \enskip J^{-1} H^{-1}
\end{aligned}
$$

## Acceleration Analysis 

Directly by differentiating

$$
\dot \bold \Theta = J_K^{-1} \bold W
$$

to get

$$
\ddot \Theta = J_k^{-1} \dot \bold W + \dot J_K^{-1} \bold W
$$

## Summary

Inverse Kinematic

$$
\begin{cases}
    \rho_1^2 = &x^2 + y^2 \cr
    \rho_2^2 = &(x+l_2 \cos({\Phi}) - c_2)^2 + (y+l_2 \sin({\Phi}))^2 \cr
    \rho_2^2 = &(x+l_3 \cos({\Phi + \theta}) - c_3)^2 + (y+l_3 \sin({\Phi + \theta}) - d_3)^2
\end{cases}
$$

Inverse Kinematic Jacobian

$$
\begin{aligned}
J_k^{-1} &= -A^{-1} B H^{-1} \cr
&= \ \ \ J^{-1} H^{-1}
\end{aligned}
$$

Acceleration

$$
\ddot \Theta = J_k^{-1} \dot \bold W + \dot J_K^{-1} \bold W
$$