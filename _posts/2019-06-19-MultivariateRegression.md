---
layout: post
title:  "Topic :: Multivariate regression"
date:   2019-06-19
categories: statistics topic
permalink: statistics
tags: statistics
use_math: true

# author
author: Kipoong Kim
---

<!-- more -->

## 1. Multivariate regression

- Consider the multivariate regression framework:
  $$
  \begin{align*}
  Y = XB + W,
  \end{align*}
  $$
  where $Y$ is an $n \times q$ matrix of responses, $X$ is an $n \times p$ design matrix, $B$ is a $p \times q$ regression coefficients matrix to be estimated, and $W$ is an matrix of errors.

- Least squares figure out the regression coefficients that minimize the following objective function:
  $$
  \parallel Y-XB \parallel_F^2,
  $$
  $\parallel \cdot \parallel_F$ is the Frobenius norm. 

- Let us assume that the responses are correlated with each other. Then, since the above objective function do not include the correlation among responses, we will use the Mahalanobis distance instead of Eucleadian distance of Frobenius norm.
  $$
  tr \left\{ (Y-XB)\Sigma^{-1} (Y-XB)^T ) \right\}
  $$
  

- 

