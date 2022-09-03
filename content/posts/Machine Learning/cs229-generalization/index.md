---
title: "Generalization"
date: 2022-06-23T22:00:00+08:00
draft: false
ShowToc: true
math: true
---

# Generalization

The ultimate goal of machine learning is to create a predictive model which performs well on unseen examples (it generalizes well). 

**Generalization** is a model's performance on *unseen* test data, measured by **test error** (loss on unseen test data).



# Test Error

Loss/error on *test examples* $(x,y)$ sampled from a *test distribution* $\mathcal{D}$ 
$$
L(\theta) = \mathbb{E}\_{(x,y)\sim\mathcal{D}}
[ (y-h_\theta(x))^2]
$$
The expectation $\mathbb E$ can be approximated by averaging many samples

In classical statistical learning, both training and test examples are drawn from the same distribution.



## Reasons for Large Test Error

There are two main reasons for large test error:

| Underfitting / Bias                                          | Overfitting / Variance                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Both training and test error are large                       | Test error is large                                          |
| Model family is unable to capture the true relationship of the data | Model is fitting to noise which does not reflect the true relationship of the data |



## Underfitting / Bias

> Model family is unable to capture the true relationship of the data

Test error even if the model were fitted to an infinitely large training set



## Overfitting / Variance

> Model is fitting to noise which does not reflect the true relationship of the data



# Tradeoff between Bias & Variance

If the model is:

- Too **simple** : It may have large **bias**
- Too **complex** : It may have large **variance**



## Decomposition of Test Error

Test error can be decomposed into three terms
$$
\text{Test Error} = 
\underbrace{\quad \sigma^2 \quad}\_\text{noise} +
\underbrace{(h^*(x) - h_{avg}(x))^2}\_\text{bias} +
\underbrace{\text{var}(h_S(x))}\_\text{variance}
$$
Which results in a convex curve with one global optimum

{{< figure src="tradeoff_bias_variance.jpg" width=632 align="center" caption="CS229 https://cs229.stanford.edu/" >}}



# Double Descent Phenomenon

Empirical observation that test error can have a second descent, in the over-parameterized regime (where number of parameters is larger than number of data points)

{{< figure src="double_descent.jpg" width=480 align="center" caption="CS229 https://cs229.stanford.edu/" >}}

Interestingly in many cases there is no second ascent. Meaning that larger models always lead to better performance



## Complexity Measure of Models

It's not clear that number of parameters is the best way to measure complexity of a model

If the norm is used instead, there is no double descent phenomenon.

{{< figure src="norm_complexity.jpg" width=800 align="center" caption="CS229 https://cs229.stanford.edu/" >}}