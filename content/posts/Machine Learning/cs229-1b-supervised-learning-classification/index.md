---
title: "Supervised Learning - Classification"
date: 2022-04-06T22:00:00+08:00
draft: false
ShowToc: true
math: true
url: posts/machine-learning/supervised-learning-classification
---

# Classification

Similar to regression, except $y$ only takes on a small number of discrete values



# Logistic Regression

Start by ignoring the fact that $y$ is discrete

Use linear regression with modified hypothesis function
$$
h_\theta(x) = g(\theta^Tx)
= \frac{1}{1 + e^{-\theta^Tx}}
$$
where $g(z) = \frac{1}{1 + e^{-z}}$ is the **logistic**/**sigmoid** function

$g(z)$ tends to $0$ or $1$ as $z \rightarrow -\infty$ or $z \rightarrow \infty$

**Other Functions**

Other functions can be used

But sigmoid has a useful function that the derivative:
$$
g' = g(1 - g)
$$

## Probabilistic Interpretation

Assume
$$
\begin{aligned}
P(y=1 | x; \theta) &= h_\theta(x) \cr
P(y=0 | x; \theta) &= 1 - h_\theta(x)
\end{aligned}
$$
or equivalently
$$
P(y | x; \theta) = [h_\theta(x)]^y [1-h_\theta(x)]^{1-y}
$$
For $n$ independent training examples the likelihood function is
$$
\begin{aligned}
L(\theta) &= p(y | X;\theta) \cr
&= \prod_{i=1}^{n} p(y^{(i)} | x^{(i)}; \theta) \cr
&= \prod_{i=1}^{n}
	\left( h_\theta(x^{(i)}) \right)^{y^{(i)}}
	\left(1 - h_\theta(x^{(i)}) \right)^{1-y^{(i)}}
\end{aligned}
$$
Which can also be transformed into log likelihood
$$
l(\theta) = \sum_{i=1}^{n} y^{(i)}
\log h(x^{(i)}) + (1 - y^{(i)}) \log(1 - h(x^{(i)}))
$$
And maximise using **gradient ascent**

## Gradient Ascent rule

The updates are
$$
\frac{\partial}{\partial \theta_j} l(\theta) =
(y - h_\theta(x)) x_j
$$

$$
\theta_j := \theta_j + \alpha
\left( y^{(i)} - h_\theta(x^{(i)})
\right)\ x_j^{(i)}
$$

# Newton's Method

An alternative method to gradient descent is newton's method

To **Maximise**
$$
\theta := \theta - \frac{l'(\theta)}{l''(\theta)}
$$

## Vector-Valued Generalisation

Newton-Raphson method
$$
\theta = \theta - H^{-1} \nabla_\theta l(\theta)
$$
Where $H \in \mathbb{R}^{(d+1) \times (d+1)}$ is the **Hessian**
$$
H_{ij} = \frac{\partial^2 l(\theta)}{\partial \theta_i \ \partial \theta_j}
$$

## Comparison with Gradient Descent

**Advantages**

- Faster convergence
- Requires significantly fewer iterations

**Disadvantages**

- Each iteration can be more expensive
  Requires inverting the Hessian
