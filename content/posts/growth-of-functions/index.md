---
title: "Growth of Functions"
date: 2022-01-17T23:05:22+08:00
draft: false
ShowToc: true
math: true
---

## Objective

Order of growth is a simple characterization of an algorithm's efficiency and allows for comparison between algorithms.

### Asymptotic Efficiency

When we look at input size $n$ large enough that only the order of growth matters. We are concerned with the running time *in the limit* as input increases without bound.



## Asymptotic Notation

Notation is defined as a function on domain $\mathbb{N} = \{ 0,1,2,\dots \}$, the set of natural numbers. It is mainly used to characterise running time, but can also be used on memory/space/etc.



### $\Theta$-notation

>Encloses the function from above and below

Useful in analyzing the average-case complexity of an algorithm. 
$$
\begin{aligned}
\Theta \big( g(n) \big)
= \\{ f(n) :\ \text{there exists } c_1,\ c_2,\ n_0 \text{ such that } \\\\
 0 \leq c_1 g(n) \leq f(n) \leq c_2 g(n) \enspace \forall n \geq n_0
\\}
\end{aligned}
$$
$\Theta(n)$ is a set, but it is usually written as $f(n) = \Theta(n)$.

{{< figure src="theta-notation.JPG" width=480 align="center" >}}

$f(n)$ belongs to set $\Theta(g(n))$ if $c_1 \cdot g(n)$ and $c_2 \cdot g(n)$ can sandwidch the function, for large $n \geq n_0$. 

The function $f(n)$ is equal to $g(n)$ to within a constant factor, which makes $g(n)$ an **asymptotically tight bound** for $f(n)$.

The definition of $\Theta(n)$ requires all $f(n) \in \Theta(g(n))$ be **asymptotically nonnegative** ($f(n) \geq 0$ as $n \rightarrow \infty$. This implies that $g(n)$ is also asymptotically nonnegative so that $\Theta(g(n))$ is not empty. Therefore assume that every function in $\Theta$-notation is asymptotically nonnegative.

> **Example** Show that $ \frac{3}{4} n^2 - 5n + \frac{3}{n} = \Theta(n^2) $
>
> $c_1 n^2 \leq \frac{3}{4} n^2 - 5n + \frac{3}{n} \leq c_2 n^2$   for all $n \geq n_0$
>
> $c_1 \leq \frac{3}{4} - \frac{5}{n} + \frac{3}{n^2} \leq c_2$
>
> $c_1 = \frac{1}{2}$, $c_2 = 1$ and $n_0 = 10$ satisfy the inequality

> **Example** Show that $5n^3 \neq \Theta(n^2)$
>
> $c_1 \leq 5 \frac{n^3}{n^2} \leq c_2$ 
>
> Which cannot hold for arbitrarily large $n$

This results in two observations:

- The **lower-order terms** can be ignored
  (They become of the form $1 / n^k$ which go to $0$)
- Coefficients can be ignored
  (Can be divided into $c_1, c_2$)

**Constants** are degree-0 polynomials, which can be expressed as $\Theta(n^0)$ or more commonly $\Theta(1)$.



### $O$-notation

> Asymptotic upper bound

Provides an upper bound on a function to within a constant factor.
$$
\begin{aligned}
O \big( g(n) \big)
= \\{ f(n) :\ \text{there exists } c,\ n_0 \text{ such that } \\\\
 0 \leq f(n) \leq c g(n) \enspace \forall n \geq n_0
\\}
\end{aligned}
$$
$\Theta(g(n))$ implies $O(g(n))$ becauses $\Theta$-notation is stronger than $O$-notation.
$$
\Theta(g(n)) \subseteq O(g(n))
$$
$O$-notation provides an asymptotic upper bound but not a **tight** one. Distinguishing between both is stanard in algorithms literature, but not in informal use.

Inspection can often reveal the $O$-notation running time. For example, double nested loops are $O(n^2)$.

When $O$-notation is used to bound the worst-case running time, it also bounds *every input*. This is different from $\Theta$-notation, where the worse-case does not imply the running time for *every* input.



### $\Omega$-notation

> Asymptotic lower bound

$$
\begin{aligned}
O \big( g(n) \big)
= \\{ f(n) :\ \text{there exists } c,\ n_0 \text{ such that } \\\\
 0  \leq c g(n) \leq f(n) \enspace \forall n \geq n_0
\\}
\end{aligned}
$$



### Theorem 3.1

For any two functions $f(n)$ and $g(n)$, we have $f(n) = \Theta(g(n))$ if and only if $f(n) = O(g(n)) and f(n) = \Omega(g(n))$

Commonly used to get asymptotic upper/lower bounds ( $O$ / $\Omega$ ) from tight bounds ( $\Theta$ )

> **Example**
>
> $ \Omega(n^2) = an^2 + bn + c$   and  $O(n^2) = an^2 + bn + c$
>
> imply that $ \Theta(n^2) = an^2 + bn + c $
