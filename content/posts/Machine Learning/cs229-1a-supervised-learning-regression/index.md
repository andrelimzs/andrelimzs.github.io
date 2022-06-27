---
title: "Supervised Learning - Regression"
date: 2022-04-06T22:00:00+08:00
draft: false
ShowToc: true
math: true
url: posts/machine-learning/supervised-learning-regression
---

# Supervised Learning

Supervised learning is the task of learning a function mapping from input to output
$$
y = f(x)
$$
The relationship can be linear/nonlinear or convex/nonconvex. The approach learns from labeled data.



## Terminology

**Input (Features)** : $x^{(i)}$

**Output (Target)** : $y^{(i)}$

**Training example** : $(x^{(i)}, y^{(i)})$

**Hypothesis** : $h(x)$

**Parameters / Weights** : $\theta$

## Types

**Regression** : Continuous values

**Classification** : Discrete values



# Linear Regression

**Objective**

Learn parameters $\theta$ for a given hypthesis function $h$ to best predict output $y$ from input $x$

**Hypothesis Function**

Candidate model that we think will best fit the data

>  [Example] A linear model can be represented as
> $$
> \begin{aligned}
> h(x) &= \sum_{i=0}^d \theta_i x_i \cr
> &= \theta^T x
> \end{aligned}
> $$



**Cost / Loss Function**

Measure of the accuracy or error of the model. The cost is driven to zero to fit the model to the data.

>  [Example] A common cost function is least-squares
> $$
> J(\theta) = \frac{1}{2} \sum_{i=1}^{n} ( h_\theta (x^{(i)} - y^{(i)}))^2
> $$
>  which results in **ordinary least squares** regression



# Least Mean Squares (LMS) Algorithm

Least mean squares, or Widrow-Hoff learning rule

1. Start with an initial guess
2. Repeatedly perturb $\theta$ 
3. Hope it converges



## Gradient Descent

$$
\theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_j} J(\theta)
$$

**Learning Rate** : $\alpha$ 

The **single training example case $(x,y)$** reduces to
$$
\theta_j := \theta_j + \alpha (y^{(i)} - h_\theta (x^{(i)})) x_j^{(i)}
$$
The update is **proportional** to the **error**
$$
\text{update} \propto (y - h_\theta(x))
$$



## Batch Gradient Descent

Group updates into vector $x$
$$
\theta := \theta + \alpha \sum_{i=1}^{n}
\left(
	y^{(i)}- h_\theta(x^{(i)})
\right) x^{(i)}
$$
Which is equivalent to gradient descent on the original cost function $J$



## Stochastic Gradient Descent

$$
\theta := \theta + \alpha
\left(
	y^{(i)}- h_\theta(x^{(i)})
\right) x^{(i)}
$$

And repeatedly run through the training set

## Batch vs SGD

| Batch               | SGD                     |
| ------------------- | ----------------------- |
| Slower convergence  | Faster convergence      |
| Converge to minimum | Oscillate about minimum |

In most cases being close to the minimum is good enough, and therefore people choose SGD for the faster convergence



# The Normal Equations

Minimise explicitly by taking the derivatives w.r.t $J$ and setting to zero

**Matrix Derivatives** : For function $f : \mathbb{R}^{n \times d} \rightarrow \mathbb{R}$

The Jacobian is
$$
\nabla_A f(A)
$$


## Least-Squares

Set $\nabla_\theta J(\theta) = 0$

$$
\begin{aligned}
\nabla_\theta J(\theta) &= X^TX \theta - X^T y \cr
 X^TX \theta &= X^T y
\end{aligned}
$$
Which gives
$$
\theta = (X^TX)^{-1} X^T y
$$

## Probabilistic Interpretation

Assume target and inputs are related via
$$
y^{(i)} = \theta^T x^{(i)} + \epsilon^{(i)}
$$
where $\epsilon^{(i)}$ are unmodelled effects/noise

Assume $\epsilon^{(i)}$ are Gaussian IID
$$
p(\epsilon^{(i)}) =
\frac{1}{\sqrt{2\pi\sigma}}
\exp \left(
	- \frac{(\epsilon^{(i)})^2}{2\sigma^2}
\right)
$$
which implies
$$
p(y^{(i)} | x^{(i)};\theta) =
\frac{1}{\sqrt{2\pi\sigma}}
\exp \left(
	- \frac{ (y^{(i)} - \theta^T x^{(i)})^2 }
	{ 2\sigma^2 }
\right)
$$
or
$$
y^{(i)} | x^{(i)}; \theta \sim \mathcal{N}(\theta^T x ^{(i)}, \sigma^2)
$$

## Likelihood Function

Given $X$ and $\theta$, what is the distribution of $y^{(i)}$s?
$$
L(\theta) = L(\theta; X, \vec{y}) = p(\vec{y}|X;\theta)
$$
Can be written as
$$
\begin{aligned}
L(\theta) &= \prod_{i=1}^{n} p(y^{(i)} | x^{(i)}; \theta) \cr
&= \prod_{i=1}^{n} \frac{1}{\sqrt{2\pi\sigma}}
\exp \left(
	- \frac{ (y^{(i)} - \theta^T x^{(i)})^2 }
	{ 2\sigma^2 }
\right)
\end{aligned}
$$
The **Principle of maximum likelihood** says that we should choose $\theta$ to make the probability as high as possible.

Maximising probability is equivalent to maximising **log likelihood**, $l(\theta)$.
$$
l(\theta) = 
n \log \frac{1}{\sqrt{2\pi\sigma}} -
\frac{1}{2\sigma^2} \sum_{i=1}^{n} 
( y^{(i)} - \theta^T x^{(i)} )^2
$$
Which is the same as minimizing the least-squares costs
$$
J(\theta) = \frac{1}{2} \sum_{i=1}^{n} 
( y^{(i)} - \theta^T x^{(i)} )^2
$$
[*] This result does not depend on $\sigma$



# Locally Weighted Linear Regression (LWR)

Assuming sufficient training data, makes choice of features less important

**Procedure**

1. Fit $\theta$ to minimise $\sum_i w^{(i)} ( y^{(i)} - \theta^T x^{(i)} )^2$
2. Output $\theta^T x$

[*] Only difference is the weight $w^{(i)}$
$$
w^{(i)} = \exp \left( 
	-\frac{(x^{(i)} - x)^2}{2\tau^2}
\right)
$$
Which biases around the query point

**Bandwidth** : $\tau$, controls the falloff distance

This is an example of a **non-parametric Algorithm**

- Requires data even after fitting
- Non-parametric means hypothesis $h$ grows linearly with size of training set

