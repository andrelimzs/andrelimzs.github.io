---
title: "CS229 Probability Theory"
date: 2022-04-05T22:00:00+08:00
draft: false
ShowToc: true
math: true
---

# Review of Probability Theory

## Overview

Probability is an important aspect of machine learning, and forms a useful basis for concepts. It is the study of **uncertainty**



## Elements of Probability

Defining probability on a set requires the following:

**Sample Space** : $\Omega$

- The set of all possible outcomes of a random experiment
- Each outcome $\omega \in \Omega$ is the complete description of the state at the end of the experiment

**Event Space (Set of events)** : $\mathcal{F}$

- A set whose elements $A \in \mathcal{F}$ (events) are subsets of $\Omega$
- Eg: $A$ is the set of possible outcomes for an experiment

**Probability Measure** : $P : \mathcal F \rightarrow \mathbb R$ which satisfies the **Axioms of Probability**

- $P(A) \geq 0$ for all $A \in \mathcal F$
- $P(\Omega) = 1$
- If $A_1, A_2, \dots$ are disjoint events \
  $P(\cup_i A_i) = \sum_i P(A_i)$



### Conditional Probability

For an event $B$ with non-zero probability

The conditional probability of $A$ given $B$ is
$$
P(A|B) \triangleq \frac{P A \cap B}{P(B)}
$$

### Independence

If and only if

1. $P(A \cap B) = P(A) P(B)$
2. $P(A|B) = P(A)$



## Random Variables

A function $X : \Omega \rightarrow \mathbb R$ that maps a sample space to a real-valued outcome

### Cumulative Distribution Functions (CDF)

Function $F_X : \mathbb R \rightarrow [0,1]$ that calculates the probability of an event
$$
F_X(x) \triangleq P(X \leq x)
$$
**Properties**

- $0 \leq F_X(x) \leq 1$
- $\underset{x \rightarrow -\infty}{\lim} F_X(x) = 0$
- $\underset{x \rightarrow \infty}{\lim} F_X(x) = 1$
- $x \leq y \implies F_X(x) \leq F_Y(y)$



### Probability Mass Function (PMF)

Function $p_X : \Omega \rightarrow \mathbb R$ that directly specifies the probability of each value for a discrete RV
$$
p_X(x) \triangleq P(X=x)
$$
**Properties**

- $0 \leq p_X(x) \leq 1$
- $\underset{x\in Val(X)}{\sum} p_X(x) = 1$
- $\underset{x\in A}{\sum} p_X(x) = P(X\in A)$



### Probability Density Functions (PDF)

Derivative of the CDF
$$
f_X(x) \triangleq \frac{dF_X(x)}{dx}
$$
If the CDF is differentiable everywhere

**Properties**

- $f_X(x) \geq 0$
- $\int_{-\infty}^{\infty} f_X(x) = 1$
- $\int_{x\in A} f_X(x) dx = P(X \in A)$



## Expectation

The "weighted average" of the values that a distribution can take

**Discrete RV**
$$
E[g(X)] \triangleq \sum_{x\in Val(A)} g(x) p_X(x)
$$
**Continuous RV**
$$
E[g(X)] \triangleq \int_{-\infty}^{\infty} g(x) f_X(x)\ dx
$$
**Properties**

- $E[a] = a$ for any constant $a$
- $E[a f(X)] = aE[f(X)]$ for any constant $a$
- (Linearity of Expectation) \
  $E[f(X) + g(X)] = E[f(X)] + E[g(X)]$
- For discrete RV \
  $E[1\{ X=k \}] = P(X = k)$



## Variance

Measure of how *concentrated* a distribution is around it's mean
$$
Var[X] \triangleq E[(X - E(X))^2]
$$
or equivalently
$$
Var[x] = E[X^2] - E[X]^2
$$
**Properties**

- $Var[a] = 0$ for any constant $a$
- $Var[a f(X)] = a^2 Var[f(X)]$ for any constant $a$



## Common Random Variables

### Discrete

**Bernoulli**

Event happens with probability $p$
$$
p(x) = \left\\{
\begin{aligned}
	&p && \text{if } p = 1 \cr
	&1-p && \text{if } p = 0
\end{aligned}
\right.
$$
**Binomial**

Number of outcomes in $n$ independent events
$$
p(x) = \begin{pmatrix} n \cr x \end{pmatrix}
p^x (1-p^{n-x})
$$
**Geometric**

Number of independent events until outcome is true
$$
p(x) = p (1-p)^{x-1}
$$
**Poisson**

Probability distribution over nonnegative integers
$$
p(x) = e^{-\lambda} \frac{\lambda^x}{x!}
$$


### Continuous

**Uniform**

Equal probability density to every value between $a$ and $b$
$$
f(x) = \left\\{
\begin{aligned}
	&\frac{1}{b-a} && \text{if } a\leq x \leq b \cr
	&0 && \text{otherwise}
\end{aligned}
\right.
$$


**Exponential**

Decaying probability density over nonnegative reals
$$
f(x) = \left\\{
\begin{aligned}
	&\lambda e^{-\lambda x} && \text{if } x\geq 0 \cr
	&0 && \text{otherwise}
\end{aligned}
\right.
$$
**Normal**

Gaussian distribution
$$
f(x) = \frac{1}{\sqrt{2\pi}\sigma} \exp \left(
-\frac{1}{2\sigma^2}(x-\mu)^2
\right)
$$


## Joint and Marginal Distributions

### CDF

Of $X$ and $Y$
$$
F_{XY}(x,y) = P(X \leq x, Y \leq y)
$$
The joint CDF is related to the marginal CDF $F_X(x) / F_Y(y)$ via
$$
\begin{aligned}
	F_X(x) &= \lim_{y \rightarrow \infty} F_{XY}(x,y) dy \cr
	F_Y(y) &= \lim_{x \rightarrow \infty} F_{XY}(x,y) dx
\end{aligned}
$$
**Properties**

- $0 \leq F_{XY}(x,y) \leq 1$
- $\lim_{x,y \rightarrow \infty} F_{XY}(x,y) = 1$
- $\lim_{x,y \rightarrow -\infty} F_{XY}(x,y) = 0$
- $F_X(x) = \lim_{y \rightarrow \infty} F_{XY}(x,y)$



### PMF

$$
p_{XY}(x,y) = P(X=x, Y=y)
$$

And is related to the marginal PMF via
$$
\begin{aligned}
	p_X(x) &= \sum_y p_XY(x,y) \cr
	p_Y(y) &= \sum_x p_XY(x,y)
\end{aligned}
$$

### PDF

$$
f_{XY}(x,y) = \frac
{\partial^2 F_{XY}(x,y)}
{\partial x \partial y}
$$

Where
$$
\begin{aligned}
	\iint_{x\in A} f_XY(x,y)\ dx\ dy = P((X,Y) \in A) \cr
\end{aligned}
$$
And
$$
\begin{aligned}
	f_X(x) = \int_{-\infty}^{\infty} f_{XY}(x,y) dy \cr
	f_Y(y) = \int_{-\infty}^{\infty} f_{XY}(x,y) dx
\end{aligned}
$$

## Conditional Distribution

> What is the probability distribution over $Y$ given that we know $X = x$?

**Discrete**
$$
p_{Y|X} (y|x) = \frac{p_{XY}(x,y)}{p_X(x)}
$$
**Continuous**
$$
f_{Y|X} (y|x) = \frac{f_{XY}(x,y)}{f_X(x)}
$$


## Bayes's Rule

Used to derive conditional probability of one variable given another
$$
P(A|B)\ P(B) = P(B|A)\ P(A)
$$
And therefore
$$
P(A|B) = \frac{ P(B|A)\ P(A) }{ P(B) }
$$


## Independence

Two RVs $X$ and $Y$ are independent if
$$
P(X,Y) = P(X)\ P(Y)
$$
for all values of $x$ and $y$



## Expectation and Covariance

Let $X$ and $Y$ be RVs and $g : \mathbb{R}^2 \rightarrow \mathbb R$

### Expectation

**Discrete**
$$
E{g(X,Y)} \triangleq
\sum_{x \in Val(X)} \sum_{y \in Val(Y)}
g(x,y) p_{XY}(x,y)
$$
**Continuous**
$$
E[g(X,Y)] = \int_{-\infty}^{\infty}
\int_{-\infty}^{\infty}
g(x,y) f_{XY}(x,y)\ dx\ dy
$$

### Convariance

$$
\begin{aligned}
	Cov[X,Y] &\triangleq
	E[(X-E[X])(Y-E(Y))] \cr
	&= E[XY] - E[X] E[Y]
\end{aligned}
$$

**Uncorrelated** : $Cov[X,Y] = 0$

**Properties**

- (Linearity of Expectation)
- $Var[X+Y] = Var[ X ] + Var[Y] + 2 Cov[X,Y]$
- If $X$ and $Y$ independent \
  $\implies E[f(X)g(Y)] = E[f(X)]\ E[g(Y)]$



## Multiple RV

The notation for two RVs can be generalised to $n$ RVs

### Properties

### Chain Rule

$$
f(x_1, x_2, \dots, x_n) = 
f(x_1) f(x_2 | x_1)
\prod_{i=3}^{n} f(x_i | x_1, \dots, x_{i-1})
$$

### Independence

Events $A_1, \dots, A_k$ are **mutually independent** if
$$
P(\underset{i \in S}{\cap} A_i) = \prod_{i\in S} P(A_i)
$$
Random variables $X_1, \dots, X_n$ are **independent** if
$$
f(x_1, \dots, x_n) = f(x_1) f(x_2) \dots f(x_n)
$$


## Random Vectors

A vector of random variables is a **random vector**

It is a mapping from $\Omega \rightarrow \mathbb R^n$

It is an alternative notation for multiple RVs
$$
X = \begin{bmatrix}
X_1 \\ \vdots \\ X_n
\end{bmatrix}
$$

### Expectation

$$
E[g(X)] = \begin{bmatrix}
E[g_1(X)] \\ \vdots \\ E[g_m(X)]
\end{bmatrix}
$$

### Covariance

$$
\Sigma = 
\begin{bmatrix}
E[X_1^2] & \dots & E[X_1 X_n] \cr
\vdots & \ddots & \vdots \cr
E[X_n X_1] & \dots & E[X_n^2]
\end{bmatrix}
$$

or
$$
\begin{aligned}
	\Sigma &= E[XX^T] - E[X]\ E[X]^T \cr
		&= E[(X-E[X])(X-E[X])^T] 
\end{aligned}
$$

**Properties**

- Positive semidefinite : $\Sigma \succeq 0$
- Symmetric : $\Sigma = \Sigma^T$



## Multivariate Gaussian Distribution

Particularly important example

Has a 

**Mean** : $\mu \in \mathbb R^n$

**Covariance** : $\Sigma \in \mathbb S_{++}^n$ (symmetric positive definite matrices)
$$
f_{X_1, \dots, X_n} (x_1, \dots, x_n; \mu, \Sigma)
= \frac{1}{(2\pi)^{\pi/2} |\Sigma|^{1/2} }
\exp \left(
	-\frac{1}{2} (x-\mu)^T \Sigma^{-1} (x-\mu)
\right)
$$
Written as $X \sim \mathcal N(\mu, \Sigma)$

**Usefulness**

Comes up extremely often due to the **Central Limit Theorem** (CLT) - A large number of independent RVs will tend toward a Gaussian distribution.

Useful because it has simple closed-form solutions.
