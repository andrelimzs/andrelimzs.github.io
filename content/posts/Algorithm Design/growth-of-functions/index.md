---
title: "Growth of Functions"
date: 2022-01-17T23:05:22+08:00
draft: true
ShowToc: true
math: true
---

## Objective

Order of growth is a simple characterization of an algorithm's efficiency and allows for comparison between algorithms.



### Asymptotic Efficiency

When we look at input size $n$ large enough that only the order of growth matters. We are concerned with the running time *in the limit* as input increases without bound.

---



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

{{< figure src="theta-notation.JPG" width=360  align="center" >}}

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



### Asymptotic Notation in Equations

- $n = O(n^2)$ \
  Refers to **set membership** $n \in O(n^2)$

- In a formula ( $\dots = 2n^2 + \Theta(n)$ ) \
  Stand-in for **some anonymous function**
  ( $\Theta(n) = f(n)$ )
- The number of anonymous functions is equal to the number of times it appears \
  Eg: $\sum_i^n{ O(i) }$ appears once and not $O(1) + \dots + O(n)$
- On the left-hand side (LHS) (It usually appears on the RHS) \
  Means that there is always a way to choose LHS and RHS to make the equation hold \
  The RHS is a **coarser** level of detail than the LHS
- Can be **chained** \
  Each equation can be interpreted separately



### o-notation

$o$-notation is a bound that is **not** asymptotically tight. ($O$-notation may or may not be asymptotically tight.)
$$
\begin{aligned}
o \big( g(n) \big)
= \\{ f(n) :\ \text{for any +ve constant } c>0, \text{ there exists } \\\\
 \text{a constant } n_0 > 0 \text{ s.t. } 0 \leq f(n) < c g(n) \enspace \forall n \geq n_0
\\}
\end{aligned}
$$
The main difference between $o$-notation and $O$-notation is that the bound $0 \leq f(n) \leq cg(n)$ holds for all constants $c > 0$ instead of **some** constant.



### $\omega$-notation

Lower bound that is **not** asymptotically tight.
$$
\begin{aligned}
o \big( g(n) \big)
= \\{ f(n) :\ \text{for any +ve constant } c>0, \text{ there exists } \\\\
 \text{a constant } n_0 > 0 \text{ s.t. } 0 \leq c g(n) < f(n) \enspace \forall n \geq n_0
\\}
\end{aligned}
$$


## Comparing Functions

### Transitivity

$f(n) = \Theta / O / \Omega (g(n))$ and $g(n) = \Theta / O / \Omega (h(n))$

Implies $f(n) = \Theta / O / \Omega (h(n))$

### Reflexivity

$f(n) = \Theta / O / \Omega (f(n))$

### Symmetry

$f(n) = \Theta(g(n)) \Leftrightarrow g(n) = \Theta(f(n))$

### Transpose Symmetry

$f(n) = O(g(n)) \Leftrightarrow g(n) = \Omega(f(n))$

$f(n) = o(g(n)) \Leftrightarrow g(n) = \omega(f(n))$



These properties allow for an analogy between asymptotic notation and inequalities.

$f(n) = O(g(n))  \quad\leftrightarrow\quad a \leq b \quad | \quad f(n) = o(g(n))  \quad\leftrightarrow\quad a < b$

$f(n) = \Omega(g(n))  \quad\leftrightarrow\quad a \geq b \quad | \quad f(n) = \omega(g(n))  \quad\leftrightarrow\quad a > b$

$f(n) = \Theta(g(n))  \quad\leftrightarrow\quad a = b$



---

## Standard Notations

### Monotonicity

$f(n)$ is **monotonically increasing** if $m \leq n$ implies $f(m) \leq f(n)$.

$f(n)$ is **strictly increasing** if $m<n$ implies $f(m) < f(n)$

### Floors and Ceilings

- Floor $\lfloor x \rfloor$ : The least integer $\leq x$ 

- Ceiling $\lceil x \rceil$ : The least integer $\geq x$

Both are **monotonically increasing**

Properties (for real $x$):

- $x-1 < \lfloor x \rfloor \leq x \leq \lceil x \rceil < x+1$

- $\lceil n/2 \rceil + \lfloor n/2 \rfloor = n$
- $ \lceil \frac{1}{b} \lceil \frac{x}{a} \rceil \rceil = \lceil \frac{x}{ab} \rceil$
- $\lceil \frac{a}{b} \rceil = \frac{a + b - 1}{b}$



### Modular Arithmetic

For integer $a$ and positive integer $n$, $a \text{ mod } n$ is the remainder of $a/n$

$a \mod n = a - n \lfloor a/n \rfloor$

And therefore $0 \leq a \mod n \leq n$

**Equivalent** : $a \equiv b\ (\text{mod } n )$ if they have the same remainder.

This also implies that $n$ is a divisor of $a-b$



### Polynomials

$$
p(n) = \sum^d_{i=0} {a_i n^i}
$$

For integer $d \geq 0$

Coefficients: $a_i$ and $a_d \neq 0$

Asymptotically positive $ \Leftrightarrow a_d >0$

