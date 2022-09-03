---
title: "Kernel Methods"
date: 2022-04-08T22:30:00+08:00
draft: false
ShowToc: true
math: true
url: posts/machine-learning/kernel-methods
---

# Kernel Methods

Kernel methods are an efficient way to perform **nonlinear** regression or classification, by calculating the dot product instead of the entire feature map.

## Feature Maps

A feature map $\phi$ is a function that maps **input attributes** to some new (nonlinear) **feature variables**.



# LMS with Features

First lets define:

- Input : $x \in \mathbb R ^ d$
- Features : $\phi(x) \in \mathbb R^{p}$
- Weights : $\theta \in \mathbb R^d$

Modify gradient descent for ordinary least squares problem
$$
\theta := \theta + \alpha
\sum_{i=1}^{n}{(y^{(i)} - \theta^T x^{(i)})\ x^{(i)}}
$$
With a **feature map** $\phi : \mathbb R^d \rightarrow \mathbb R^p$ that maps $x$ to $\phi(x)$
$$
\theta := \theta + \alpha
\sum_{i=1}^{n}{(y^{(i)} - \theta^T
\underbrace{\phi(x^{(i)})}
)\ \underbrace{\phi(x^{(i)}})}
$$
Which will have SGD update rule
$$
\theta := \theta + \alpha
(y^{(i)} - \theta^T \phi(x^{(i)}))\ \phi(x^{(i)})
$$



## LMS with Kernel Trick

The gradient descent update becomes **computationally expensive** when the feature map $\phi$ is high dimensional because the size of $\phi(x)$ grows roughly on the order of $O(n^\text{dim})$.

The kernel trick depends on the fact that the parameter vector $\theta$ is a linear combination of the features $\phi(x^{(i)})$. For the next section I will switch to matrix notation to make it clearer.
$$
\underbrace{
	\theta
}\_{ \mathbb R^{d} } =
\underbrace{ 
	\Phi(x)
}\_{ \mathbb R^{d \times n} }
\underbrace{
	\beta
}\_{ \mathbb R^{n} }
$$

- $\Phi(x) = \begin{bmatrix} \phi(x)^{(1)} & \dots & \phi(x)^{(n)}\end{bmatrix} \in \mathbb R^{d \times n}$

Which expanded is
$$
\begin{bmatrix}
	\theta_1 \cr \vdots \cr \theta_n 
\end{bmatrix} =
\begin{bmatrix}
	\phi(x^{(1)})_1\ \beta_1 + \dots + \phi(x^{(n)})_1\ \beta_n \cr
	\vdots \cr
	\phi(x^{(1)})_n\ \beta_1 + \dots + \phi(x^{(n)})_n\ \beta_n
\end{bmatrix}
$$
Next, the original update equation
$$
\underbrace{\theta}\_{\mathbb R^d} :=
\theta + \alpha
\ \underbrace{\Phi(x)}\_{\mathbb R^{d \times n}}
\underbrace{\left(
	Y^T - \Phi(x)^T \theta
\right)}\_{\mathbb R^{n}}
$$
- $Y = \begin{bmatrix} y^{(1)} & \dots & y^{(n)}\end{bmatrix} \in \mathbb R^{1 \times n}$
- $\Phi(x) = \begin{bmatrix} \phi(x)^{(1)} & \dots & \phi(x)^{(n)}\end{bmatrix} \in \mathbb R^{d \times n}$



can be transformed to coordinates of $\beta$
$$
\beta := \beta + \alpha( Y^T - \Phi(x)^T \theta )
$$
And finally substituted with $\theta = \Phi(x) \beta$
$$
\beta := \beta + \alpha( Y^T - 
\underbrace{\Phi(x)^T \Phi(x)}\_\text{inner product}
\ \beta )
$$
Where $\Phi(x)^T \Phi(x)$ is also known as the dot/inner product and often written as
$$
\Phi(x)^T \Phi(x) = \langle \Phi(x),\Phi(x) \rangle
$$

## Why Is It Faster

1. The inner product $\langle \phi(x^{(j)}),\phi(x^{(i)}) \rangle$ can be precomputed
2. The inner product can be computed without computing the feature map explicitly

$$
\langle x,z \rangle = 
1 + \langle x,z \rangle +
\langle x,z \rangle^2 +
\langle x,z \rangle^3
$$

Which takes $O(d)$ for $\langle x,z \rangle$ then constant operations for the higher powers



# Kernels

Function that maps $\chi \times \chi \rightarrow \mathbb R$
$$
K(x,z) \triangleq \langle \phi(x), \phi(z) \rangle
$$

## Procedure

1. Compute kernel

$$
K(x^{(i)},x^{(j)}) \triangleq
\langle \phi(x^{(i)}), \phi(x^{(j)}) \rangle
$$

2. Iterate through samples and update

$$
\beta_i := \beta_i + \alpha \left(
	y^{(i)} - \sum_{j=1}^n \beta_j
	K(x^{(i)},x^{(j)})
\right), \quad \forall i \in \{1,\dots n\}
$$

Or equivalently
$$
\beta := \beta + \alpha( Y^T - K\beta )
$$

The representation $\beta$ is sufficient to compute the prediction $\theta^T \phi(x)$
$$
\theta^T \phi(x) = \sum_{i=1}^{n} \beta_i K(x^{(i)},x)
$$

## Properties of Kernels

## Necessary and Sufficient Condition

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