---
title: "CS229 Deep Learning"
date: 2022-04-08T22:00:00+08:00
draft: false
ShowToc: true
math: true
---

# Deep Learning

## Supervised with Nonlinear Models

Supervised learning is

- Predict $y$ from input $x$
- Suppose model/hypothesis is $h_\theta(x)$

Previous methods have considered

- Linear regression : $h_\theta(x) = \theta^Tx$
- Kernel method : $h_\theta(x) = \theta^T \phi(x)$

Both are linear in $\theta$



Now consider models that are nonlinear in both

- Parameters : $\theta$
- Inputs : $x$

The most common of which is the **neural network**



### Cost/Loss Function

Define least-squares cost for one sample
$$
J^{(i)}(\theta) = \frac{1}{2} ( h_\theta(x^{(i)}) - y^{(i)})^2
$$
and mean-square cost for the dataset
$$
J(\theta) = \frac{1}{n} \sum_{i=1}^{n} J^{(i)}(\theta)
$$

### Optimisers

SGD or it's variants is commonly used
$$
\theta := \theta - \alpha \nabla_\theta J(\theta)
$$
with $\alpha > 0$

**Hardware parallelisation** means that it's often faster to compute the gradient of $B$ examples simultaneously

$\Rightarrow$ Mini-batch SGD is most commonly used


## Neural Networks

>  Refers to any type of nonlinear model/parameterisation $h_\theta(x)$ that involves matrix multiplications and other entrywise nonlinear operations

### Activation Function

A (1D) nonlinear function that maps $\mathbb{R}$ to $\mathbb{R}$

Common ones include

- ReLU : Rectified Linear Unit

### Single Neuron

$$
h_\theta(x) = ReLU(w^Tx + b)
$$

- **Input** : $x \in \mathbb{R}^d$
- **Weights** : $w \in \mathbb{R}^d$
- **Bias** : $b \in \mathbb{R}$

### Stacking Neurons

Enabels more complex networks

**[Example] Two-layer Fully-connected Network**

{{< figure src="fcn_two_layer.jpg" width=360  align="center" >}}

For a hidden layer with three neurons
$$
a_1 = ReLU(w_1^T x + b_1) \\\
a_2 = ReLU(w_2^T x + b_2) \\\
a_3 = ReLU(w_3^T x + b_3)
$$
Or equivalently
$$
\begin{aligned}
&z_i = (w_i^{[1]})^T x + b_i^{[1]}
\quad \forall i \in [1,\dots,m] \cr
& a_i = ReLU(z_i) \cr
& a = [ a_1,\ \dots,\ a_m ]^T \cr
& h_\theta(x) = (w^{[2]})^T a + b^{[2]}
\end{aligned}
$$

