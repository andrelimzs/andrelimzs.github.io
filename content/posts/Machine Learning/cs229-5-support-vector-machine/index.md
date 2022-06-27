---
title: "Support Vector Machine (SVM)"
date: 2022-04-09T22:00:00+08:00
draft: false
ShowToc: true
math: true
url: posts/machine-learning/support-vector-machines
---

# Support Vector Machines

For the linear classifier 
$$
h_{w,b}(x) = g(w^T x + b)
$$
with $y \in \{-1, 1\}$

## Margins

Margins represent the idea of how **confident** and **correct** a prediction is.

### Functional Margin, $\hat \gamma$

The functional margin is defined as
$$
\hat{\gamma}^{(i)} = y^{(i)}
(w^T x^{(i)} + b)
$$

And it requires some sort of normalization condition.

Otherwise the functional margin can be made arbitrarily large by scaling $w$ and $b$ without changing the decision boundary.

### Geometric Margin, $\gamma$

The geometric margin is the Euclidean distance from a sample to the decision boundary, defined as

$$
\gamma = \frac{\hat{\gamma}}{||w||}
$$

Or
$$
\gamma^{(i)} = \frac{1}{||w||} y^{(i)}
\left(
\left( w \right)^T x^{(i)} + b
\right)
$$

The geometric margin is invariant to scaling of the parameters $w$ and $b$.

When $||w|| = 1$, the functional margin equals the geometric margin.

### On a Training Set

Given a training set $S = \{ x^{(i)}, y^{(i)}; i=1,\dots,n \}$ the margins are defined as the smallest of the individual training examples
$$
\hat{\gamma} = \min_{i=1,\dots,n} \hat\gamma^{(i)}
\quad \text{or} \quad
\gamma = \min_{i=1,\dots,n} \gamma^{(i)}
$$

## Optimal Margin Classifier

A natural desire would be to maximize the geometric margin.

Assume that the training set is linearly separable (can be separated with a hyperplane),  the problem can be formulated as:
$$
\begin{aligned}
&&\max_{\gamma,w,b} &&&\gamma \cr
&&\text{s.t.} &&&y^{(i)} (w^T x^{(i)} + b) \geq \gamma, \quad
i = 1, \dots, n \cr
&&&&& ||w|| = 1
\end{aligned}
$$
However $||w||=1$ is a difficult (non-convex) constraint, and cannot be easily solved.

Using $\gamma = \frac{\hat{\gamma}}{||w||}$, transform the problem into
$$
\begin{aligned}
&&\max_{\gamma,w,b} &&&
	\frac{\hat\gamma}{||w||} \cr
&&\text{s.t.} &&&
	y^{(i)} (w^T x^{(i)} + b) \geq \hat\gamma, \quad
i = 1, \dots, n
\end{aligned}
$$
Because the absolute magnitude of $\hat \gamma$ doesn't matter, we can set $\hat\gamma = 1$ to get
$$
\max_{w,b} \quad \frac{1}{||w||}
$$
which is equivalent to
$$
\min_{w,b} \quad ||w||^2
$$
To finally get
$$
\begin{aligned}
&&\min_{w,b} &&&
	\frac{1}{2} ||w||^2 \cr
&&\text{s.t.} &&&
	y^{(i)} (w^T x^{(i)} + b) \geq \hat\gamma, \quad
i = 1, \dots, n
\end{aligned}
$$
Which is a **quadratic programming** (QP) problem.



## Lagrange Duality

Useful in solving constrained optimization problems

Consider a problem of the form
$$
\begin{aligned}
&&\min_{w} &&&
	f(w) \cr
&&\text{s.t.} &&&
	h_i(w) = 0,
	\quad i=1,\dots,l
\end{aligned}
$$
Define the **Lagrangian**
$$
\mathcal{L}(w,b) = f(w) + \sum_{i=1}^l \beta_i h_i(w)
$$
With **Lagrangian multipliers** $\beta_i$

And we can solve $w$ and $b$ by setting the partial derivatives of $\mathcal L$ to zero
$$
\frac{\partial \mathcal L}{\partial w_i}=0, \quad
\frac{\partial \mathcal L}{\partial \beta_i}=0
$$

### Constrained Optimization

The method can be generalized to include inequality constraints. 

For the **primal** optimization problem
$$
\begin{aligned}
&&\min_{w} &&&
	f(w) \cr
&&\text{s.t.} &&&
	g_i(w) \leq 0, \quad i = 1, \dots, k \cr
&& &&&
	h_i(w) = 0, \quad i = 1, \dots, l
\end{aligned}
$$
Define the **generalized Lagrangian**
$$
\mathcal{L} (w,\alpha,\beta) =
f(w) + \sum_{i=1}^k{\alpha_i g_i(w)} + \sum_{i=1}^l{\beta_i h_i(w)}
$$
With Lagrange multipliers $\alpha, \beta$

Define $\theta_\mathcal{P}$
$$
\theta_\mathcal{P}(w) = \max_{\alpha,\beta:\alpha_i \geq 0}
\mathcal L(w,\alpha,\beta)
$$
With values
$$
\theta_\mathcal{P} = \left\\\{ 
\begin{aligned}
f&(w) && \text{if w satisfies primal constraints} \cr
&\infty && \text{otherwise}
\end{aligned}
\right.
$$
Therefore the problem
$$
\min_w \theta_\mathcal{P}(w) = \min_w \max_{\alpha,\beta:\alpha_i\geq0}
\mathcal{L}(w,\alpha,\beta)
$$
is the same as the original problems

Define the optimal value of the objective $p*$
$$
p* = \min_w \theta_\mathcal{P}(w)
$$


Define
$$
\theta_\mathcal{D}(\alpha,\beta) =
\min_{w} \mathcal L(w,\alpha,\beta)
$$


Pose the **dual optimization** problem
$$
\max_{\alpha,\beta:\alpha_i\geq0} \theta_\mathcal{D}(\alpha,\beta) =
\max_{\alpha,\beta:\alpha_i\geq0} \min_{w} \mathcal{L}(w,\alpha,\beta)
$$
Which is the same as the primal problem, with the order of $\max$ and $\min$ swapped

The primal and dual problems are related via
$$
d^* = \max_{\alpha,\beta:\alpha_i\geq0} \min_{w}
\mathcal L(w,\alpha,\beta)
\leq
\min_{w} \max_{\alpha,\beta:\alpha_i\geq0}
\mathcal L(w,\alpha,\beta)
= p^*
$$
Under **Karush-Kuhn-Tucker** (KKT) conditions
$$
d^* = p^*
$$
and this can be used to solve the primal problem



## Optimal Margin Classifiers

For the (primal) optimization problem
$$
\begin{aligned}
&&\min_{w,b} &&&
	\frac{1}{2} ||w||^2 \cr
&&\text{s.t.} &&&
	y^{(i)} (w^T x^{(i)} + b) \geq 1, \quad
i = 1, \dots, n
\end{aligned}
$$
Rewrite the constraints as
$$
g_i(w) = -y^{(i)} (w^T x^{(i)} + b) + 1 \leq 0
$$
With one constraint per training example

Construct the Lagrangian
$$
\mathcal L(w,\beta,\alpha) =
\frac{1}{2} ||w||^2 -
\sum_{i=1}^{n} \alpha_i \left[
	y^{(i)} (w^T x^{(i)} + b) - 1
\right]
$$
Find the dual form

First minimize $\mathcal L(w,\beta,\alpha)$ w.r.t $w$
$$
\nabla_w \mathcal L(w,\beta,\alpha) = 
w - \sum_{i=1}^{n} \alpha_i y^{(i)} x^{(i)} = 0
$$
Which implies
$$
w = \sum_{i=1}^{n} \alpha_i y^{(i)} x^{(i)}
$$
Then minimize $\mathcal L(w,\beta,\alpha)$ w.r.t $b$
$$
\frac{\partial}{\partial\beta} \mathcal L(w,\beta,\alpha)
= \sum_{i=1}^{n} \alpha_i y^{(i)} = 0
$$
Combine to obtain the dual optimization problem
$$
\begin{aligned}
&&\max_{\alpha} &&&
	W(\alpha) = 
    \sum_{i=1}^{n} \alpha_i - 
    \frac{1}{2} \sum_{i,j=1}^{n}
    y^{(i)} y^{(j)} \alpha_i \alpha_j
    \langle x^{(i)} , x^{(j)} \rangle \cr
&&\text{s.t.} &&&
	\alpha_i \geq 0, \quad
i = 1, \dots, n \cr
&& &&&
	\sum_{i=1}^{n} \alpha_i y^{(i)} = 0
\end{aligned}
$$
The original problem satisfies the KKT conditions and therefore we can solve this dual problem in lieu of the primal problem.

