---
layout: post
title:  "C++ :: coordinate descent algorithm"
date:   2019-04-30
categories: C++
permalink: C++_algorithm
tags: C++

# author
author: Kipoong Kim
---

<!-- more -->

# Coordinate descent algorithm



## 1. Notion

### (1) Linear regression

- We can consider the linear regression framework as:
$$
  y_i=x_i\beta + \epsilon_i
$$
  The least squares solve the problem
$$
  \parallel y-X\beta \parallel _2^2,
$$
  where $\parallel \cdot \parallel_2$ represents the $\ell _2$ norm. 

  ​	However, in several cases, we cannot get an explicit solution. To solve this problem, coordinate descent algorithm(CCD) optimize the obejective function by variable assuming that others are fixed. 
  ​	For example, there are five variables associated with a continuous response variable. Firstly, we set an initial value to estimate regression coefficients and CCD find a estimates for the first predictor regardless of others. And it estimates the second regression coefficient with the updated initial value, so on.

  ​	In linear regression,
$$
\begin{align*}
	L 	& = \frac{1}{2} \parallel y-X\beta \parallel _2 ^2 \\
    	& = \frac{1}{2} \sum_i ( y_i - \sum_j x_{ij} \beta_j )^2 \\
  
  \frac{\partial L}{\partial \beta_k} & = \sum_i x_{ik} (y_i - \sum_j x_{ij} \beta_j ) \\
  	& = \sum_i x_{ik} (y_i - \sum_{j\ne k} x_{ij} \beta_j - x_{ik} \beta_k ) \\
  	& = x_k^T (y-X_{(-k)}\beta_{(-k)} ) - x_k^T x_k \beta_k \\
  \therefore \hat{\beta_k} & = (x_k^T x_k)^{-1}x_k^T (y-X_{(-k)}\beta_{(-k)} ) . \\
\end{align*}
$$


### (2) Implementation in C++

- 
  ```c++
  #include <iostream>
  #include <armadillo>
  
  using namespace arma;
  using namespace std;
  
  int main(){
  
      mat X; 
      X.randn(20, 5);
      mat Y(20, 1);
      mat beta(X.n_cols, 1); 
      beta.fill(0);
      
      mat Xj;
      mat Xmj;
      double prob;    
  
      mat err;
      err.randn(20,1);
      Y = X.col(1) + 2*X.col(2) + 3*X.col(3) + 4*X.col(4) + 5*X.col(1) + err;
      
      for( int i=0; i<10; i++ ){
          for( int j=0; j<X.n_cols; j++){            
              cout << "Current beta is " << beta.t() << endl;
              
              Xj = X.col(j);
              Xmj = X;
              Xmj.col(j).fill(0);
              mat betamj = beta;
              betamj.row(j).fill(0);
              mat betaj = ( Xj.t() * (Y - Xmj * betamj) ) / arma::as_scalar(Xj.t() * Xj);
              beta.row(j) = betaj;
              
              prob = arma::as_scalar( (Y-X*beta).t() * (Y-X*beta) );
              
              cout << "The value of loss is " << prob << endl ;
          }
      }
  
      X.save("Xdata.csv", raw_ascii);
      Y.save("Ydata.csv", raw_ascii);
  
      return 0;
  }
  
  //   -0.0041
  //    6.3584
  //    1.6550
  //    3.0154
  //    4.5035
  ```

- Notification

  * Armadillo library can be easily installed in UNIX operating system, but windows have challanges.

### (3) References

- [Coursera]: https://www.coursera.org/lecture/ml-regression/coordinate-descent-for-least-squares-regression-normalized-features-wkbZU

- [Ryan Tibshirani]: https://www.cs.cmu.edu/~ggordon/10725-F12/slides/25-coord-desc.pdf

- [Linear algebra with C++]: https://www.asc.ohio-state.edu/physics/ntg/6810/readings/hjorth-jensen_notes2012_06.pdf