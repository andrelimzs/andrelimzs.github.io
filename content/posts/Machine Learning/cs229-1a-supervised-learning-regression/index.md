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



# Gradient Descent

An optimisation algorithm commonly used to fit machine learning models. It is an iterative, first-order approach. It is also known as the **Least mean square (LMS)** update rule or **Widrow-Hoff** learning rule.

The update (for single training example)
$$
\theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_j} J(\theta)
$$

moves $\theta$ in the direction of best improvement



## Batch Gradient Descent

More generally, gradient descent can be applied to multiple training examples **simultaneously** by grouping updates into vector $x$
$$
\theta := \theta + \alpha
\underbrace{\sum_{i=1}^{n}
\left(
	y^{(i)}- h_\theta(x^{(i)})
\right) x^{(i)} }_{\large = \frac{\partial J}{\partial \theta} \quad}
$$
Which is equivalent to gradient descent on the original cost function $J$.

Batch gradient descent looks at **every** example on every iteration and can therefore be slow.



## Stochastic Gradient Descent (SGD)

An alternative to batch gradient descent is stochastic gradient descent (also known as incremental gradient descent).

SGD updates the weights $\theta$ **on every iteration**, making much quicker progress for large datasets.

The update rule is given by
$$
\theta := \theta + \alpha
\left(
	y^{(i)}- h_\theta(x^{(i)})
\right) x^{(i)}
$$

which is repeatedly run through a loop



## Batch vs SGD

| Batch               | SGD                     |
| ------------------- | ----------------------- |
| Slower convergence  | Faster convergence      |
| Converge to minimum | Oscillate about minimum |

In most cases being close to the minimum is good enough, and therefore people choose SGD for the faster convergence



# The Normal Equations (Explicit Minimisation)

In the case of linear regression, $J$ can be minimised explicitly by taking the derivatives w.r.t $J$ and setting them to zero

**Matrix Derivatives** : For function $f : \mathbb{R}^{n \times d} \rightarrow \mathbb{R}$ the Jacobian is $\nabla_A f(A)$

## Least-Squares via Moore-Penrose Psuedoinverse

Set $\nabla_\theta J(\theta) = 0$

$$
\underbrace{\nabla_\theta J(\theta)}_{= 0} =
X^TX \theta - X^T y
$$
Therefore
$$
X^TX \theta = X^T y
$$
Which gives the closed-form solution
$$
\theta = (X^TX)^{-1} X^T y
$$

## Probabilistic Interpretation

Linear regression with least-squares cost can be derived from statistical methods to give a probability interpretation.  

Assume target and inputs are related via
$$
y^{(i)} = \theta^T x^{(i)} + 
\underbrace{ \epsilon^{(i)} }_\text{noise}
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
Which implies that $y$ given $x$ parameterised by $\theta$ is gaussian
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

The likelihood function answers the question 

>  Given $X$ and $\theta$, what is the distribution of $y^{(i)}$s?

$$
\begin{aligned}
L(\theta)
	&= L(\theta; X, \vec{y}) \cr
	&= p(\vec{y}|X;\theta)
\end{aligned}
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
## Maximum Likelihood Estimation (MLE) of $\theta$

The **Principle of maximum likelihood** says that we should choose $\theta$ to maximise $L(\theta)$ (make the probability of $y$ given $x$ as high as possible)

To simplify the procedure, the we can maximise any strictly increasing function of $L(\theta)$ such as **log likelihood**, $l(\theta)$.
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


# Locally Weighted Linear Regression (LWR)

The choice of features is important because it can result in overfitting or underfitting.

{{< figure src="feature-choice.jpg" width=800 align="center" >}}

But one method to make the choice of features less critical is **locally weighted linear regression** (LWR), assuming there is sufficient training data.

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

