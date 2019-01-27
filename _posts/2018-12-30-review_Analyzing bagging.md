---
layout: post
title:  "review :: Analyzing bagging"
date:   2018-12-30
categories: review
permalink: review_Analyzing bagging
tags: review bagging selprob

# author
author: Kipoong Kim
---

(1) Analyzing bagging

(2) On bagging and Nonlinear estimation

<!-- more -->

Stability selection
=========================

# (1) Analyzing bagging
> BY PETER BÜHLMANN AND BIN YU1 <br />
> The Annals of Statistics, Vol. 30, No. 4, 927–961 (2002)

## Abstract

- **Bagging**은 특히 high-dimensional data set에서 unstable  estimator or classfier을 개선시켜주는 가장 효과적인 computationally intensive한 방법이다.
- **Instability**에 대한 notion 정립
- **Hard decision** [Indicator; I(X>x)] 문제에서 variance reduction에 대한 이론적 분석 결과 (testing in regression, decision tree)
- **Hard decision**은 매우 instable한 결과를 제공하고, **bagging**은 **hard decision**과 같은 문제를 smooth하도록 하므로, **bagging**을 통해 smaller variance와 mean squared error를 얻을 수 있다.
- **Subagging** with theoretical explanation.
  (1) Computationally cheaper
  (2) Approximately equilvalant accuracy
- Obtaining an asymptotic limiting distribution at the cube-root rate for the split point


### Definition of **bagging**

1. Construct a bootstrap sample $$L_i^* = (Y_i^*, X_i^*), i=1, \cdots, n $$ according to the empirical distribution of the pairs $$L_i = (Y_i, X_i)$$.<br />
2. Compute the bootstrapped predictor $$\hat{\theta}_n^*(x)$$ by the plug-in principle; that is, $$\hat{\theta}_n^*(x)=h_n(L_1^*, \dots, L_n^*)(x), \text{where } \hat{\theta}_n=h_n(L_1, \cdots, L_n)(x)$$ <br />
3. The bagged predictor is $$\hat{\theta}_{n;B}(x)=E[\hat{\theta}_n^*(x)]$$.<br />
$$E[\hat{\theta}_n^*(x)]$$ can be implemented by Monte Carlo: for every bootstrap simulation $$j\in{1,\dots,J}$$ from **3**, we compute $$E[\hat{\theta}_n^*(x)] \approx \frac{1}{J} \sum_{j=1}^J \hat{\theta}^*_{n;(j)}(x) $$


### Variable selection via testing in linear models.

Since variable selection is also in a field of hard decision problem in linear model,
bagging also acts as smoothing or softening and leads to a reduced variance without much sacrifice on the bias.

$$
\hat{\theta}_n(x) = \sum_{j=1}^p{\hat{\beta_j} 1_{[|\hat{\beta_j}|>u_{n,j}]}x^{(j)} }
$$

![Figure of example of estimating with bagged samples in linear model](/images/ExampleOfBagging.png)
<br>
### Proposition 3.1
Let $$\hat{\theta}_n(\cdot)=h_n(L_1, \dots, L_n)(\cdot)$$ be any predictor which is symmetric in the data $$L_1, \dots, L_n$$. Assume that $$m \le n$$ and $$E[h_m(L_1, \dots, L_m)(x)]^2 < \infty $$ for all $$x$$. Then, for any $$x$$,

$$
E[\hat{\theta}_{n;SB(m)}(x)] = E[ h_m(L_1, \dots, L_m)(x)], \\
Var(\hat{\theta}_{n;SB(m)}(x)) \le \frac{m}{n}Var(h_m(L_1, \dots, L_m)(x)).
$$



## Comments by statpng
- A motivation of selection probability is that the hard decision for selecting predictor can be smoothed by using bagging. Moreover, subagging can have more cheaper on compuational time and same accuracy.
- Likewise, Gira's paper also smooth hard decision of the selection of rare variants.


# (2) On bagging and Nonlinear estimation
> BY Jerome H. Friedman and Peter Hall <br />
> Preprint. (2000)


## Abstract

- Statistical estimator의 Linear, higher order로의 decomposition 또는 4차이상으로 optimize되는 objective function의 decomposition에 대한 연구
- Bagging은 estimate를 its expected value로 바꿈으로써 nonlinear component의 variability를 감소시켰다. (linear part는 X)
- Different resampling scheme을 조사하였고, half-resampling without replacement 방법이 bootstrap sampling과 virtually 동등함을 보였다.
- 1/2 이외의 다른 sampling fraction이 좋을 때도 있었으며, optimal value를 선택하는 것은 vias-variance trade-off 문제일 것이다.
- Bagging은 variance를 감소시키는 것 뿐만 아니라, decision tree와 같은 non-linear estimator의 bias 역시 감소시키는 것으로 보인다.

> keypoints:: Bagging, Non-linear estimation, Half-sampling, Optimal value, Reducing the variance and bias.
