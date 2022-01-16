---
title: "CLRS - Getting Started"
date: 2022-01-16T17:43:50+08:00
draft: false
ShowToc: true
math: true
---

## Objective

The main objective of CLRS is to introduce  frameworks to design and analyze algorithms.



## Terminology

### Loop Invariant

Something used to prove the correctness of an algorithm, with three properties:

1. **Initialization** \
   It is true prior to the first iteration of the loop
2. **Maintenance** \
   If it is true before an iteration, it remains true after that iteration
3. **Termination** \
   It helps show that the algorithm is correct after the loop terminates \
   Commonly used in combination with the termination condition

### Input Size, $n$

Depends on the problem, but commonly:

- *Number of items in the input* (sorting, FFT)
- *Number of bits* (multiplication)

It can also be described by more than one number, for example:

- Graph vertices and edges.

### Runtime, $T(n)$

> Number of primitive operations, or 'steps' executed

Assume each line requires a constant amount of time. The running time of an algorithm is now:
$$
\sum \text{each individual line} \times \text{num of times executed}
$$
 

### RAM Model

#### Instructions

The RAM model contains commonly found instructions:

- **Arithmetic** \
  (add, subtract, multiple, divide, remainder, floor, ceiling)
- **Data Movement** \
  Load, store copy
- **Control** \
  (Conditional, unconditional branch, subroutine call, return)

#### Data Types

- **Integers**
- **Floating point**

And assume a limit on size of each word of data.

> For inputs of size $n$, assume integers are represented by $c \log n$ bits for some constant $c \geq 1$.
>
> - $c \geq 1$ : So that each word can hold the value of $n$ to allow us to index the last element
> - $c$ constant : So that word size cannot grow arbitrarily large

#### Grey Area - Instructions

There are some instructions included in real computers that are not in the list above. For example, exponentiation. CLRS assumes that they are constant-time operations for small exponent values.

#### Memory Hierarchy

The RAM model does **not** model memory hierarchy, such as caches or virtual memory. This is usually sufficient, and RAM models are excellent at predicting performance of real machines.



## Analyzing Algorithms

>  "Predict the resources an algorithm requires"

This resource is most often *computation time*. 

This kind of analysis requires a model of the hardware that's being used to detail the costs of the different capabilities. Most of CLRS assumes the one-processor **random-access machine** (**RAM**) model. Instructions are executed sequentially.

Analysis is focused on **worst-case running time**, for three reasons:

- Gives an upper bound
- Happens fairly often (for some algorithms)
- Close to the "*average case*" for many algorithms

Later chapters will explore **average-case running time** via *probabilistic analysis*. There are also *randomized algorithms* that have an *expected running time*.



### Order of Growth

The analysis can be further simplified by reducing it to the **rate of growth**/**order of growth**. Therefore ignore:

- All lower order terms
  (Only the leading term is significant as $n \rightarrow \infty$)
- Constant coefficients
  (Less significant as $n \rightarrow \infty$)

This means that worst-case running time is only really relevant for very large $n$.
