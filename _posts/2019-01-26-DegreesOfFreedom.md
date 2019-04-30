---
layout: post
title:  "lecture :: Degrees of freedom"
date:   2018-01-29
categories: book
permalink: DegreesOfFreedom
tags: lecture

# author
author: Kipoong Kim
---

Degrees of freedom has been widely used over statistics, such as hypothesis testing, regression, regularization.
I introduce the degrees of freedom with an example. When someone want to know what is the degrees of freedom in your class, I hope this example is somewhat good.

In wikipedia, the degrees of freedom is explained as follows.

*In statistics, the number of degrees of freedom is the number of values in the final calculation of a statistic that are free to vary*

<!-- more -->

As a starting point, suppose that we have a samples of independent normally distributed observations,
$$X_1, \dots, X_n.$$

This can be represented as an $n$-dimensional random vector:
$$ X= \left( \begin{matrix} X_1 \\ \vdots \\ X_n \end{matrix} \right). $$

#### (1) Residuals

Now, let $\bar{X}$ be the sample mean of $X$. The random vector could be decomposed as the sample mean, $\bar{X}$, plus a vector of residuals, $\{X_i - \bar{X}\}_{i=1, \cdots, n}$:
$$ \left( \begin{matrix} X_1 \\ \vdots \\ X_n \end{matrix} \right) = \left( \begin{matrix} \bar{X} \\ \vdots \\ \bar{X} \end{matrix} \right) + \left( \begin{matrix} X_1-\bar{X} \\ \vdots \\ X_n-\bar{X} \end{matrix} \right). $$

The second vector on the right-hand side is constrained by the relation $ \sum_{i=1}^n (X_i - \bar{X}) = 0.$ The first $n-1$ components of this vector can be anything, but the last component should be the value of $X_n = n\bar{X} - \sum_{i=1}^{n-1} X_i.$  Once you know the first $n-1$ components, the contrain tell you the value of $n$-th component. Therefore, this vector $\left( \begin{matrix} X_1 \\ \vdots \\ X_n \end{matrix} \right)$ has $n-1$ degrees of freedom.

#### (2) Sample variance

As shown above, the residuals have $n-1$ degrees of freedom. What is degrees of freedom of the sample variance?
The residual sum of squares from the sample variance can be decomposed as the sum of squares minus the $n$ squares of the sample mean:

$$ \sum_{i=1}^n (X_i-\bar{X})^2 = \sum_{i=1}^n X_i^2 + n\bar{X}^2.$$

The first vector on the right-hand side is $n$ degrees of freedom, and the second vector is $1$ degree of freedom.
Thus the vector on the left-hand side has $n-1$ degrees of freedom.

#### (3) Linear models

Let us assume that we have the independent observations $X_1, \dots, X_n, ~ Y_1, \dots, Y_n, ~Z_1, \dots, Z_n$ from the three populations.
The restriction to three groups and equal sample sizes simplifies notation, but the ideas are easily generalized.

The observations can be decomposed as
$$
  X_i = \bar{M} + (\bar{X} - \bar{M}) + (X_i - \bar{X}) \\
  Y_i = \bar{M} + (\bar{Y} - \bar{M}) + (Y_i - \bar{Y}) \\
  Z_i = \bar{M} + (\bar{Z} - \bar{M}) + (Z_i - \bar{Z}),
$$
where $\bar{X},~ \bar{Y}, ~\bar{Z}$ is the sample means of indivisual samples for the tree population and $\bar{M} = (\bar{X}+\bar{Y}+\bar{Z})/3 $ is the mean of all $3n$ observations.

In vector notation, this decomposition can be written as:
$$\left( \begin{matrix} X_1 \\ \vdots \\ X_n \\ Y_1 \\ \vdots \\ Y_n \\ Z_1 \\ \vdots \\ Z_n \end{matrix} \right) = \bar{M}\left( \begin{matrix} 1 \\ \vdots \\ 1 \\ 1 \\ \vdots \\ 1 \\ 1 \\ \vdots \\ 1 \end{matrix} \right) + \left( \begin{matrix} \bar{X}-\bar{M} \\ \vdots \\ \bar{X}-\bar{M} \\ \bar{Y}-\bar{M} \\ \vdots \\ \bar{Y}-\bar{M} \\ \bar{Z}-\bar{M} \\ \vdots \\ \bar{Z}-\bar{M} \end{matrix} \right) + \left( \begin{matrix} X_1 - \bar{X} \\ \vdots \\ X_n - \bar{X} \\ Y_1 - \bar{Y} \\ \vdots \\ Y_n - \bar{Y} \\ Z_1 - \bar{Z} \\ \vdots \\ Z_n - \bar{Z} \end{matrix} \right).$$

The observation vector, on the left-hand side, has $3n$ degrees of freedom.
On the right-hand side, the first vector has $1$ degree of freedom for the overall mean. Since the second vector has a constrain, $\bar{X}+\bar{Y}+\bar{Z}=\bar{M}$, the vector therefore must lie in a 2-dimensional subspace. The remaining $3n-3$ degrees of freedom are in the residual vector.
