---
title: "Supervised Learning"
date: 2022-04-07T22:00:00+08:00
draft: false
ShowToc: true
math: true
---

# Supervised Learning

## Terminology

- **Input (Features)** : $x^{(i)}$
- **Output (Target)** : $y^{(i)}$
- **Training example** : $(x^{(i)}, y^{(i)})$
- **Training set** : Set of training examples
- **Hypothesis** : $h(x)$

## Types

- **Regression** : Continuous values
- **Classification** : Discrete values



# (Linear) Regression

First decide the hypothesis function

Where the convention is to let $x_0 = 1$
$$
h_\theta(x) = \theta_0 + \theta_1 x_1 + \theta_2 x_2
$$

$$
h(x) = \sum_{i=0}^d \theta_i x_i = \theta^T x
$$

## Objective

Learn the parameters $\theta$ to best predict $y$



### Cost Function

Define
$$
J(\theta) = \frac{1}{2} \sum_{i=1}^{n} ( h_\theta (x^{(i)} - y^{(i)}))^2
$$
This is the *least-squares cost function* that gives **ordinary least squares** regression



## LMS Algorithm

Least mean squares, or Widrow-Hoff learning rule

1. Start with an initial guess
2. Repeatedly perturb $\theta$ 
3. Hope we converge

### Gradient Descent

$$
\theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_j} J(\theta)
$$

- **Learning Rate** : $\alpha$ 

**Single training example case $(x,y)$**

Reduces to
$$
\theta_j := \theta_j + \alpha (y^{(i)} - h_\theta (x^{(i)})) x_j^{(i)}
$$
The update is **proportional** to the **error**
$$
\propto (y - h_\theta(x))
$$

### Batch Gradient Descent

Group updates into vector $x$
$$
\theta := \theta + \alpha \sum_{i=1}^{n}
\left(
	y^{(i)}- h_\theta(x^{(i)})
\right) x^{(i)}
$$
Which is equivalent to gradient descent on the original cost function $J$

### Stochastic Gradient Descent

$$
\theta := \theta + \alpha
\left(
	y^{(i)}- h_\theta(x^{(i)})
\right) x^{(i)}
$$

And repeatedly run through the training set

### Batch vs SGD

| Batch               | SGD                     |
| ------------------- | ----------------------- |
| Slower convergence  | Faster convergence      |
| Converge to minimum | Oscillate about minimum |

In most cases being close to the minimum is good enough, and therefore people choose SGD for the faster convergence



## The Normal Equations

Minimise explicitly by taking the derivatives w.r.t $J$ and setting to zero

### Matrix Derivatives

For function $f : \mathbb{R}^{n \times d} \rightarrow \mathbb{R}$

$\nabla_A f(A)$ is the Jacobian

### Least-Squares

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

### Likelihood Function

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



## Locally Weighted Linear Regression (LWR)

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



# Classification

Similar to regression, except $y$ only takes on a small number of discrete values

## Logistic Regression

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
