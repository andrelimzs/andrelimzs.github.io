---
title: "Supervised Learning - Classification"
date: 2022-04-06T22:00:00+08:00
draft: false
ShowToc: true
math: true
url: posts/machine-learning/supervised-learning-classification
---

# Classification

Classification is similar to regression, except output $y$ only takes on a small number of discrete values, or **classes**.



# Logistic Regression

Ignoring the fact that $y$ is discrete will often result in very poor performance. $h_\theta(x)$ should also be constrained to $y \in \{ 0, 1 \}$ 

One approach is to modify the hypothesis function to use the **logistic** / **sigmoid** function
$$
h_\theta(x) = g(\theta^Tx)
= \frac{1}{1 + e^{-\theta^Tx}}
$$
where $g(z) = \frac{1}{1 + e^{-z}}$ is the **logistic**/**sigmoid** function

**Properties of Sigmoid**

$g(z)$ tends to:
$$
\left\\{ \begin{aligned}
0 && z \rightarrow -\infty \cr
1 && z \rightarrow \infty
\end{aligned} \right.
$$
The derivative is
$$
g' = g(1 - g)
$$

## Probabilistic Interpretation

Similar to least-square regression, the classification model can be derived as a *maximum likelihood estimator* of $\theta$

Assume
$$
\begin{aligned}
P(y=1 | x; \theta) &= h_\theta(x) \cr
P(y=0 | x; \theta) &= 1 - h_\theta(x)
\end{aligned}
$$
or equivalently
$$
P(y | x; \theta) =
\left( h_\theta(x) \right)^y \ \left( 1-h_\theta(x) \right)^{1-y}
$$
For $n$ independent training examples the *likelihood function* (distribution of $y$ given $x$ and parameterised by $\theta$) is
$$
\begin{aligned}
L(\theta) &= p(y | X;\theta) \cr
&= \prod_{i=1}^{n} p(y^{(i)} | x^{(i)}; \theta) \cr
&= \prod_{i=1}^{n}
	\left( h_\theta(x^{(i)}) \right)^{y^{(i)}}
	\left(1 - h_\theta(x^{(i)}) \right)^{1-y^{(i)}}
\end{aligned}
$$
Which can be transformed into *log likelihood*
$$
l(\theta) = \sum_{i=1}^{n} y^{(i)}
\log h(x^{(i)}) + (1 - y^{(i)}) \log(1 - h(x^{(i)}))
$$
And maximised using **gradient ascent**

## Gradient Ascent rule

The update rule is
$$
\frac{\partial}{\partial \theta_j} l(\theta) =
(y - h_\theta(x)) \ x_j
$$

Which gives the stochastic gradient ascent rule
$$
\theta_j := \theta_j + \alpha
\left( y^{(i)} - h_\theta(x^{(i)})
\right)\ x_j^{(i)}
$$

# Other Optimizers - Newton's Method

An alternative method to gradient descent is newton's method. It uses the fact that the maxima of a function is the point where the first derivative is zero
$$
\theta := \theta - \frac{l'(\theta)}{l''(\theta)}
$$

## Vector-Valued Generalisation

The generalisation is called the **Newton-Raphson** method
$$
\theta = \theta - H^{-1} \nabla_\theta l(\theta)
$$
Where $\nabla_\theta l(\theta)$ is the *Jacobian* and $H$ is the **Hessian**
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
