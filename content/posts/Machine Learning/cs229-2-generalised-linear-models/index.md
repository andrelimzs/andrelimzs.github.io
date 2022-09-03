---
title: "Generalised Linear Models (GLM)"
date: 2022-04-06T22:00:00+08:00
draft: false
ShowToc: true
math: true
url: posts/machine-learning/generalised-linear-models
---

# Generalised Linear Models (GLM)

Generalised Linear Models (GLMs) are a family of models which include many common distributions such as Gaussian, Bernoulli and Multinomial.

## The Exponential Family

The exponential family serves as a starting point for GLMs.

The exponential family is defined as
$$
p(y; \eta) = b(y)
\ \exp{\left( \eta^T\ T(y) - a(\eta) \right)}
$$

- **Natural (Canonical) parameter** : $\eta$ 
- **Sufficient statistic** : $T(y)$ 
- **Log Partition function** : $a(\eta)$

$T(y)$ is often chosen to be $T(y) = y$

$e^{-a(\eta)}$ serves as a normalisation constant, and ensures the distribution sums to $1$

### Family / Set Of Distributions

A family or set of distributions is defined by $(T, a, b)$ and parameterized by $\eta$

### Bernoulli as Exponential Family

The Bernoulli distribution is in the exponential family. It is a distribution over $y \in \{0,1\}$, with mean $\phi$. Varying $\phi$ gives different Bernoulli distributions.

The Bernoulli distribution can be shown to be an exponential family distribution by manipulating the equation into the form $p = b \exp \left( \eta T - a \right)$
$$
\begin{aligned}
p(y; \phi) &= \phi^y (1 - \phi)^{1-y} \cr
&= \exp \left( y\log\phi + (1-y) \log(1-\phi) \right) \cr
&= \underbrace{1 \vphantom{\frac{}{}} }_b
\exp {\large(}
	\underbrace{\log \frac{\phi}{1-\phi} }\_\eta
	\ \underbrace{y \vphantom{\frac{}{}} }_T +
	\underbrace{ \log(1-\phi) \vphantom{\frac{}{}} }_a
{\large)}
\end{aligned}
$$



### Gaussian as Exponential Family

Similarly for the Gaussian distribution
$$
\begin{aligned}
p(y; \mu) &= \frac{1}{\sqrt{2\pi}} \left(-\frac{1}{2} 
(y-\mu)^2 \right) \cr
&= \underbrace{\frac{1}{\sqrt{2\pi}}
\exp (-\frac{1}{2} y^2)}_b \cdot
\exp (
	\underbrace{\mu \vphantom{\frac{}{}} }_n
	\underbrace{y \vphantom{\frac{}{}} }_T -
	\underbrace{\frac{1}{2} \mu^2}_a
)
\end{aligned}
$$

### Other Distributions in the Exponential Family

There are many other common distributions such as:

- Multinomial, Poisson
- Gamma, Exponential
- Beta, Dirichlet



## Constructing GLMs

Consider a classification/regression problem where we want to predict $y$ as a function of $x$

If we make three assumptions

1 . Given $x$ and $\theta$, $y$ is some exponential family distribution with parameter $\eta$
$$
y\ |\ x;\theta \sim \text{ExpFamily}(\eta)
$$
2 . Given $x$, predict $\mathbb{E}[T(y)]$ 
$$
\text{Want } h(x) = \mathbb{E} [ T(y) \ | \ x]
$$
3 . Natural parameter $\eta$ and inputs $x$ are linearly related
$$
\eta = \theta^T x
$$

Many different/common types of distributions can be modelled as GLMs, and GLMs possess desirable properties such as ease of learning.

**Canonical Response Function** : Distribution's mean as function of the natural parameter
$$
g(\eta) = \mathbb{E}[T(y); \eta]
$$
**Canonical Link Function** : Inverse of the response function
$$
g^{-1}
$$

### Ordinary Least Squares

Ordinary least squares can be derived as a GLM with the following properties:

- Target variable $y$ is **Continuous**
- $y$ given $x$ is modelled as **Gaussian** $\mathcal{N}(\mu, \sigma^2)$

Formulate the hypothesis function from assumption 2
(Predict $\mathbb{E}[T(y)]$ given $x$)
$$
h_\theta(x) = \mathbb{E}[ y\ |\ x;\theta]
$$
Because it is Gaussian
$$
h_\theta(x) = \mu
$$
From the formulation of the Gaussian as an exponential family distribution, $\mu = \eta$
$$
\begin{aligned}
h_\theta(x) &= \mu \cr
&= \eta
\end{aligned}
$$
Finally from assumption 3 ($\eta = \theta^T x$)
$$
\begin{aligned}
h_\theta(x) &= \eta \cr
&= \theta^T x
\end{aligned}
$$

The **canonical response function** of the Gaussian distribution is the identity function.



### Logistic Regression

Logistic regression can also be formulated as a GLM.

- $y \in \{0, 1 \}$

- Choose Bernoulli family to model $y\ |\ x$

The hypothesis function is 
$$
\begin{aligned}
h_\theta(x) &= \mathbb{E}[y\ |\ x;\theta] \cr
&= \phi \vphantom{\frac{1}{1}} \cr
&= \frac{1}{(1 + e^{-\eta})} \cr
&= \frac{1}{(1+e^{-\theta^Tx})}
\end{aligned}
$$

The **canonical response function** of the Bernoulli distribution is the logistic function.



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
\left\\{
    \begin{aligned}
        &1 &&\text{if True} \cr
        &0 &&\text{otherwise}
    \end{aligned}
\right.
$$

$$
(T(y))_i = 1 \\{ y=i \\}
$$

Which gives
$$
\begin{aligned}
\mathbb{E} [ (T(y))\_i ] &= P(y=i) \cr
&= \quad \phi_i
\end{aligned}
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

Fits the exponential family form
$$
p(y;\theta) = 
\underbrace{1 \vphantom{\frac{1}{1}} }\_b
\exp {\LARGE(}
\underbrace{
(T(y))\_i \log(\frac{\phi_1}{\phi_k}) + \dots +
(T(y))\_{k-1} \log(\frac{\phi_{k-1}}{\phi_k})
}\_\eta + \underbrace{ \log(\phi_k) \vphantom{\frac{1}{1}}  }_a
{\LARGE)}
$$

With: 

- **Link Function** : $\eta_i = \log \frac{\phi_i}{\phi_k}$

- **Response Function** : $\phi_i = \frac{e^{n_i}}{\sum_{j=1}^{k} e^{n_j}}$

This mapping from $\eta \rightarrow \phi$ is also called the **softmax** function



**Softmax regression** is a generalisation of logistic regression

It outputs estimated probabilities for every value of $i = 1, \dots, k$

