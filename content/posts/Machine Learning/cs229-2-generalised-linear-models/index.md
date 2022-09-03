---
title: "Generalised Linear Models (GLM)"
date: 2022-04-06T22:00:00+08:00
draft: false
ShowToc: true
math: true
url: posts/machine-learning/generalised-linear-models
---

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

