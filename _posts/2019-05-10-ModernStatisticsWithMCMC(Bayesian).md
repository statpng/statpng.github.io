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



### Markov chain

We will have a brief introduction to Markov chain to explain the following algorithms.

Let $X_1, \cdots, Xn$ be \italic{iid} mean $\mu$ and variance $\sigma^2$.

Let $ \bar{X_n}=\frac{1}{m} \sum_{i=1}^n X_i $.

- Law of Large Numbers : $\bar{X_n} \rightarrow \mu $ as $n \rightarrow$ with prob $1$ : Sample mean converges to true mean pointwisely.

Markov chain (example of stochastic processes)

While Law of Large Numbers or Central Limit theorem assumes i.i.d. of r.v’s, Markov chain admit their dependency.

We assumes that $X_0, X_1, X_2, \cdots$ (think of $X_n$ as state of system of (discrete) time $n$).

(Markov property)


$$
\begin{align}
P(X_{n+1}=j | X_n=i, X_{n-1}=i_{n-1}, \cdots, X_0=i_0) &= P(X_{n+1}=j |X_n=i ) \\
& = q_{ij} ~~ \text{(transition probability)}.
\end{align}
$$


--> Future and past are conditionally independent.



# Lecture 1

<iframe height='400' scrolling='yes' title='Fancy Animated SVG Menu' src='https://www.edwith.org/harvardprobability/lecture/30923/' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 90%;'></iframe>

# Lecture 2
<iframe height='400' scrolling='yes' title='Fancy Animated SVG Menu' src='https://www.edwith.org/harvardprobability/lecture/30924/' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 90%;'></iframe>

# Lecture 3
<iframe height='400' scrolling='yes' title='Fancy Animated SVG Menu' src='https://www.edwith.org/harvardprobability/lecture/30925/' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 90%;'></iframe>



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