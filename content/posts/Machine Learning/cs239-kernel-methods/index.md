---
title: "CS229 Kernel Methods"
date: 2022-04-08T22:30:00+08:00
draft: false
ShowToc: true
math: true
---

# Kernel Methods

> What if $y$ can be more accurately represented as a **nonlinear** function of $x$?

Regression can be performed on a nonlinear basis

The original input **attributes** are mapped to new **feature** variables via a **feature map**, $\phi$

## LMS with Features

Modify gradient descent for ordinary least squares problem
$$
\theta := \theta + \alpha
\sum_{i=1}^{n}{(y^{(i)} - \theta^T x^{(i)})\ x^{(i)}}
$$
With a **feature map** $\phi : \mathbb R^d \rightarrow \mathbb R^p$ that maps $x$ to $\phi(x)$
$$
\theta := \theta + \alpha
\sum_{i=1}^{n}{(y^{(i)} - \theta^T \phi(x^{(i)}))\ \phi(x^{(i)}})
$$
and (SGD update rule)
$$
\theta := \theta + \alpha
(y^{(i)} - \theta^T \phi(x^{(i)}))\ \phi(x^{(i)})
$$

## LMS with Kernel Trick

The gradient descent update becomes **computationally expensive** when the feature map $\phi$ is high dimensional

The size of $\phi(x)$ grows roughly on the order of $O(n^\text{dim})$, which quickly becomes intractable

The kernel trick depends on the fact that the parameter vector $\theta$ is a linear combination of the features $\phi(x^{(i)})$
$$
\theta = \phi(x) \beta^T
$$
Performing a change in coordinates of the original update equation
$$
\beta := \beta + \alpha( y - \theta^T \phi(x) )
$$
And finally substitute $\theta = \phi(x) \beta^T$
$$
\beta := \beta + \alpha( y - \beta \ \phi(x)^T \phi(x) )
$$
Or alternatively
$$
\forall i \in \{1,\dots n\}, \quad
\beta_i := \beta_i + \alpha \left(
	y^{(i)} - \sum_{j=1}^n \beta_j
	\phi(x^{(j)})^T \phi(x^{(i)}))
\right)
$$
​    Where $\phi(x)^T \phi(x)$ is also known as the dot/inner product and often written as
$$
\phi(x)^T \phi(x) = \langle \phi(x),\phi(x) \rangle
$$

### Why Is It Faster

1. The inner product $\langle \phi(x^{(j)}),\phi(x^{(i)}) \rangle$ can be precomputed
2. The inner product can be computed without computing the feature map explicitly

$$
\langle x,z \rangle =
1 + \langle x,z \rangle
+ \langle x,z \rangle^2
+ \langle x,z \rangle^3
$$

Which takes $O(d)$ for $\langle x,z \rangle$ then constant operations for the higher powers



### Kernel

Function that maps $\chi \times \chi \rightarrow \mathbb R$
$$
K(x,z) \triangleq \langle \phi(x), \phi(z) \rangle 
$$

### Procedure

1. Compute kernel

$$
K(x^{(i)},x^{(j)}) \triangleq
\langle \phi(x^{(i)}), \phi(x^{(j)}) \rangle
$$

2. Iterate through samples and update

$$
\forall i \in \{1,\dots n\}, \quad
\beta_i := \beta_i + \alpha \left(
	y^{(i)} - \sum_{j=1}^n \beta_j
	K(x^{(i)},x^{(j)})
\right)
$$

$$
\beta := \beta + \alpha( \vec{y} - K\beta )
$$

The representation $\beta$ is sufficient to compute the prediction $\theta^T \phi(x)$
$$
\theta^T \phi(x) = \sum_{i=1}^{n} \beta_i K(x^{(i)},x)
$$

## Properties of Kernels

### Necessary and Sufficient Condition

Suppose $K$ is a valid kernel corresponding to some feature map $\phi$

Consider $n$ points $\{ x^{(1)}, \dots, x^{(n)}  \}$

Define a square $n\times n$ **kernel matrix** $K$ with the $(i,j)$-entry given by
$$
K_{ij} = K(x^{(i)},y^{(j)})
$$
If $K$ is a valid matrix:

1. $K_{ij} = K_{ji}$ (Symmetric)

2. $z^T K Z \geq 0$ (PSD)

Which means that kernel matrix $K \in \mathbb{R}^{n \times n}$ is **positive semidefinite**
