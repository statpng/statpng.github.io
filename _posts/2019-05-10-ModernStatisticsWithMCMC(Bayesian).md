---
layout: post
title:  "Algorithm :: ModernStatisticsWithMCMC(Bayesian)"
date:   2019-05-10
categories: Algorithm
permalink: Bayesian_algorithm
tags: Bayesian Algorithm
use_math: true

# author
author: Kipoong Kim
---

<!-- more -->

### Metropolis-Hastings algorithm

- Algorithm 1
  Given $ x^{(t)} $,

  1. Generate 
   $ Y_t \sim q( y | x^{(t)} ) $ 
  
  2. Take
     $$
     x^{(t+1)} = \begin{cases} 
     Y_t &, \text{ with probability } P(x^{(t)}, Y_t) \\
     x^{(t)} & , \text{ with probability } 1-P(x^{(t)}, Y_t).
     \end{cases}\\
     \text{where } P(x, y)=\text{min}\left\{ \frac{\frac{f(y)}{q(y|x)}}{\frac{f(x)}{q(x|y)}}, 1 \right\}
  $$
  
- Algorithm 2
  Choose proposal symmetric matrix $ Q = (Q_{ab}), Q_{ab} = Q_{(a, b)} = P(b|a) $
  $\pi(x) = \frac{ \tilde{\pi(x)} }{Z} $, where $Z>0$ is a normalizing constant.

  Given initial point $x_0 \in \chi $
  
  For $ i = 0, 1, 2, \cdots, n-1 : $
  
  1. Sample $x$ from $ Q(x_i, x). $
  
  2. Sample $u$ from uniform(0, 1). 
     
  3. If $u < \frac{\tilde{\pi(x)}}{\tilde{\pi(x_i)}}, \text{ then } x_{i+1} = x$, else $x_{i+1} = x_i$.
  
  Return: $x_0, x_1, \cdots, x_n$.