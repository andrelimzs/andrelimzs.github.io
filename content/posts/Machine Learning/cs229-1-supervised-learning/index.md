---
title: "Supervised Learning"
date: 2022-04-06T22:00:00+08:00
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

### Gradient Ascent rule

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



# Generalised Linear Models (GLM)

Show that both linear regression and classification are special cases of **Generalised Linear Models**

## Exponential Family

The class of distributions in the exponential family is
$$
p(y; \eta) = b(y)\ e^{\eta^T\ T(y)}\ e^{-a(\eta))}
$$

- $\eta$ : **natural (canonical) parameter**
- $T(y)$ : **sufficient statistic** \
  Often let $T(y) = y$
- $a(\eta)$ : **log partition function**

$e^{-a(\eta)}$ normalises the distribution and ensures it sums to $1$

### Family/Set

Defined by a (fixed) choice of $(T, a, b)$

And parameterized by $\eta$

### Bernoulli

With mean $\phi$ defines a distribution over $y \in \{0,1\}$

Varying $\phi$ gives different Bernoulli distributions

$$
\begin{aligned}
p(y; \phi) &= \phi^y (1 - \phi)^{1-y} \cr
&= \exp \left( y\log\phi + (1-y) \log(1-\phi) \right) \cr
&= \underbrace{1}_y \exp (
    \underbrace{\left(\log \phi/(1-\phi) \right)}_n
    \ \underbrace{y}_T +
    \underbrace{\log(1-\phi)}_a
    )
\end{aligned}
$$

Which shows that is is part of the exponential family

### Gaussian

$$
\begin{aligned}
p(y; \mu) &= \frac{1}{\sqrt{2\pi}} \left(-\frac{1}{2} 
(y-\mu)^2 \right) \cr
&= \underbrace{\frac{1}{\sqrt{2\pi}}
\exp (-\frac{1}{2} y^2)}_b \cdot
\exp (\underbrace{\mu}_n
	\underbrace{y}_T
	- \underbrace{\frac{1}{2} \mu^2}_a )
\end{aligned}
$$

### Other Distributions

- Multinomial
- Poisson
- Gamma
- Exponential
- Beta
- Dirichlet



## Constructing GLMs

Consider a classification/regression problem where we want to predict $y$ as a function of $x$

Make three assumptions

- $y\ |\ x;\theta \sim \text{ExpFamily}(\eta)$ \
  Given $x$ and $\theta$, y is some exponential family distribution parameterised by $\eta$
- Predict $\mathbb{E}[T(y)]$ given $x$
- Natural parameter $\eta$ and inputs $x$ are linear 

$$
\eta = \theta^T x
$$

Many different/common types of distributions can be modelled as GLMs. 

### Canonical Response Function

Distribution's mean as function of the natural paramter
$$
g(\eta) = \mathbb{E}[T(y); \eta]
$$
### Canonical Link Function

Inverse of the response function
$$
g^{-1}
$$

### Ordinary Least Squares

- Target variable $y$ is continuous
- Model $y\ |\ x$ as Gaussian $\mathcal{N}(\mu, \sigma^2)$

From assumption 2 (Predict $\mathbb{E}[T(y)]$ given $x$)
$$
h_\theta(x) = \mathbb{E}[ y\ |\ x;\theta]
$$
Because it is a Gaussian
$$
h_\theta(x) = \mu
$$
From the formulation of the Gaussian as an exponential family distribution, $\mu = \eta$
$$
h_\theta(x) = \eta
$$
Finally from assumption 3 
$$
h_\theta(x) = \theta^T x
$$


### Logistic Regression

- $y \in \{0, 1 \}$

- Choose Bernoulli family to model $y\ |\ x$

$$
\begin{aligned}
h_\theta(x) &= \mathbb{E}[y\ |\ x;\theta] \cr
&= \phi \cr
&= 1/(1 + e^{-\eta}) \cr
&= 1/1(1+e^{-\theta^Tx})
\end{aligned}
$$



### Softmax Regression

- Classification problem with multiple classes/categories
- Model as a **multinomial** distribution

Express multinomial as an exponential family distribution

#### Parameterise Multinomial

To parameterise over $k$ outcomes, we need to use $k-1$ parameters to prevent redundancy

(The sum of all outcomes must be $1$)

Define $T(y) \in \mathbb{R}^{k-1}$ as
$$
\begin{aligned}
T(1) &= \{ 1, 0, 0, \dots, 0 \} \cr
T(2) &= \{ 0, 1, 0, \dots, 0 \} \cr
T(k-1) &= \{ 0, 0, 0, \dots, 1 \} \cr
T(k) &= \{ 0, 0, 0, \dots, 0 \}
\end{aligned}
$$
Define an **indicator function**:
$$
\begin{aligned}
	&1 &&\text{if True} \cr
    &0 &&\text{otherwise}
\end{aligned}
$$

$$
(T(y))_i = 1 \\{ y=i \\}
$$

Which gives
$$
\mathbb{E} [ (T(y))\_i ] = P(y=i) = \phi_i
$$
Therefore the probability
$$
\begin{aligned}
    p(y; \phi)
    &= \phi_1^{1\\{y=1\\}}
    \phi_2^{1\\{y=2\\}} \dots
    \phi_{k-1}^{1\\{y=k-1\\}}
    \phi_k^{1-\sum_{i=1}^{k-1}1\\{y=i\\}} \cr
    &= \phi_1^{(T(y))\_1}
    \phi_2^{(T(y))\_2} \dots
    \phi_k^{1-\sum_{i=1}^{k-1}1(T(y))\_i} \cr
    &= \exp\left(
    (T(y))\_i \log(\phi_1) + \dots + 
    ((1 - \Sigma_{i=1}^{k-1}(T(y))\_i)) \log(\phi_k)
    \right)
\end{aligned}
$$

$$
p(y;\theta) = 
\underbrace{1}\_b
\exp {\LARGE(}
\underbrace{
(T(y))\_i \log(\frac{\phi_1}{\phi_k}) + \dots +
(T(y))\_{k-1} \log(\frac{\phi_{k-1}}{\phi_k})
}\_\eta + \underbrace{\log(\phi_k)}_a
{\LARGE)}
$$

Which fits the exponential family form

**Link Function** : $\eta_i = \log \frac{\phi_i}{\phi_k}$

**Response Function** : $\phi_i = \frac{e^{n_i}}{\sum_{j=1}^{k} e^{n_j}}$

This mapping from $\eta \rightarrow \phi$ is also called the **softmax** function



**Softmax regression** is a generalisation of logistic regression

It ouputs estimated probabilities for every value of $i = 1, \dots, k$

