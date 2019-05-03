---
layout: post
title:  "C++ :: coordinate descent algorithm"
date:   2019-04-30
categories: Cpp
permalink: C++_algorithm
tags: Cpp
use_math: true

# author
author: Kipoong Kim
---

<!-- more -->


# Coordinate descent algorithm



## 1. Linear regression

- ###  Derivation

  - We can consider the linear regression framework as:

    

    $$ y_i=x_i\beta + \epsilon_i $$

    

    The least squares solve the problem

    

    $$ \parallel y-X\beta \parallel _2^2, $$

    

    where $\parallel \cdot \parallel_2$ represents the $\ell _2$ norm. 

    

    ​	However, in several cases, we cannot get an explicit solution. To solve this problem, coordinate descent algorithm(CCD) optimize the obejective function for each variable assuming that others are fixed. 
    ​	For example, there are five variables associated with a continuous response variable. Firstly, we set an initial value to estimate regression coefficients and CCD find a estimates for the first predictor regardless of others. And it estimates the second regression coefficient with the updated initial value, so on.

    ​	A derivation of estimate in linear regression is as follows.

$$
\begin{align} 
L 	& = \frac{1}{2} \parallel y-X\beta \parallel _2 ^2 \\
    	& = \frac{1}{2} \sum_i ( y_i - \sum_j x_{ij} \beta_j )^2 \\
  
  \frac{\partial L}{\partial \beta_k} & = -\sum_i x_{ik} (y_i - \sum_j x_{ij} \beta_j ) \\
  	& = -\sum_i x_{ik} (y_i - \sum_{j\ne k} x_{ij} \beta_j - x_{ik} \beta_k ) \\
  	& = - x_k^T (y-X_{(-k)}\beta_{(-k)} ) + x_k^T x_k \beta_k \\ \\
  \therefore \hat{\beta_k} & = (x_k^T x_k)^{-1}x_k^T (y-X_{(-k)}\beta_{(-k)} )  
  \end{align}
$$

- ### Implementation in C++

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

 * Notification: Armadillo library can be easily installed in UNIX operating system, but windows have challanges.

 * ### References

   	*  [Coursera](https://www.coursera.org/lecture/ml-regression/coordinate-descent-for-least-squares-regression-normalized-features-wkbZU)
      	*  [Ryan Tibshirani](https://www.cs.cmu.edu/~ggordon/10725-F12/slides/25-coord-desc.pdf)
         	*  [Linear algebra with C++](https://www.asc.ohio-state.edu/physics/ntg/6810/readings/hjorth-jensen_notes2012_06.pdf)




## 2. The lasso

- ###  Derivation

  - The lasso solve the problem with constrain $\parallel \beta \parallel _1 < t$  using Lagrange multiplier,

    $$ \parallel y-X\beta \parallel _2^2 + \lambda \parallel \beta \parallel _1 , $$

    

    where $\parallel \cdot \parallel_1$ represents the $\ell _1$ norm. 

    

    ​	A derivation of estimate in the lasso is as follows.

$$
\begin{align} 
L 	& = \frac{1}{2} \parallel y-X\beta \parallel _2 ^2 + \lambda \parallel \beta \parallel_1 \\
    	& = \frac{1}{2} \sum_i ( y_i - \sum_j x_{ij} \beta_j )^2 + \lambda \sum_{j=1}^p |\beta_j| \\
    	& = \frac{1}{2} \sum_i ( y_i - \sum_j x_{ij} \beta_j )^2 + \lambda \sum_{j=1}^p \beta_j sgn(\beta_j) \\
  
  \frac{\partial L}{\partial \beta_k} & = - \sum_i x_{ik} (y_i - \sum_{j\ne k} x_{ij} \beta_j - x_{ik} \beta_k ) + \lambda sgn(\beta_k) \\
  	& = - x_k^T (y-X_{(-k)}\beta_{(-k)} ) + x_k^T x_k \beta_k + \lambda sgn(\beta_k) \\ \\
  
   
  \text{if  } \beta_k > 0, \text{  } \hat{\beta_k} & = (x_k^T x_k)^{-1} \left \{ x_k^T (y-X_{(-k)}\beta_{(-k)} ) - \lambda \right \} _+  \\ \\
  
  \text{if  } \beta_k < 0, \text{  } \hat{\beta_k} & = -(x_k^T x_k)^{-1} \left \{ - x_k^T (y-X_{(-k)}\beta_{(-k)} ) - \lambda \right \} _+  \\ \\
  
  
  \therefore \hat{\beta_k} & = (x_k^T x_k)^{-1} \left \{ | x_k^T (y-X_{(-k)}\beta_{(-k)} ) | - \lambda  \right \}_+ sgn(\beta_k)  \\
  
  
  \end{align}
$$

### 







## 3. Ridge

- ### Derivation

  $$
  \begin{align}
  L & = \frac{1}{2} \parallel y-X\beta \parallel_2^2 + \frac{\lambda}{2} \parallel \beta \parallel_2^2 \\
   & = \frac{1}{2} \sum_{i=1}^{n} \left( y_i - \sum_{j=1}^p x_{ij} \beta_j \right)^2 + \frac{\lambda}{2} \sum_{j=1}^p \beta_j^2 \\
   
   \frac{ \partial L }{\partial \beta_k} & = - \sum_{i=1}^n x_{ik} \left( y_i - \sum_{j\ne k}x_{ij} \beta_j - x_{ik}\beta_k \right) + \lambda \beta_k \\
   & = - x_k^T(y-X_{(-k)}\beta_{(-k)} ) + x_k^Tx_k \beta_k + \lambda \beta_k \\
   
   \therefore \hat{\beta_k} & = (x_k^T x_k + \lambda )^{-1}x_k^T (y-X_{(-k)}\beta_{(-k)}).

  \end{align}
$$
  

  
  

## 4. Elastic-net

- ### Derivation

  $$
  \begin{align}
  
  L & = \frac{1}{2} \parallel y-X\beta \parallel_2^2 + \lambda \left( \frac{1-\alpha}{2} \parallel \beta \parallel_2^2 + \alpha \parallel \beta \parallel _1 \right) \\
  
  & = \frac{1}{2} \sum_{i=1}^n \left( y_i - \sum_{j=1}^px_ij\beta_j \right)^2 + \lambda \left( \frac{1-\alpha}{2} \sum_{j=1}^p \beta_j^2 + \alpha \sum_{j=1}^p | \beta_j | \right) \\
  
  \frac{\partial L}{\partial \beta_k}  & = - \sum_{i=1}^n x_{ik} \left( y_i - \sum_{j\ne k}^p x_ij \beta_j - x_{ik} \beta_k \right) + \lambda \left( (1-\alpha)\beta_k + \alpha  sgn(\beta_k) \right) \\
  
    & = - x_k^T (y - X_{(-k)} \beta_{(-k)}) + x_k^T x_k \beta_k + \lambda \left( (1-\alpha)\beta_k + \alpha sgn(\beta_k) \right) \\ \\
    
    \therefore \hat{\beta_k} & = \frac{ \left( | x_k^T(y-X_{(-k)}\beta_{(-k)}) | - \alpha  \right )_+ sgn(\beta_k) }{x_k^T x_k + \lambda(1-\alpha) }
  
  
  \end{align}
  $$

  





## 5. Block coordinate descent (multi-response elastic-net)

- ### Derivation

  $$
  \begin{align}
  
  L & = \frac{1}{2} \parallel Y-XB \parallel_2^2 + \lambda \left( \frac{1-\alpha}{2} \parallel B \parallel_F^2 + \alpha \sum_{j=1}^p \parallel B_{j\cdot} \parallel_2 \right) \\
  
  & = \frac{1}{2} \sum_{i=1}^n \parallel y_{i\cdot} - \sum_{j=1}^p x_{ij} B_{j\cdot} \parallel_2^2 + \lambda \left( \frac{1-\alpha}{2} \sum_{j=1}^p \parallel B_{j\cdot} \parallel_2^2 + \alpha \sum_{j=1}^p \parallel B_{j\cdot} \parallel_2 \right) \\
  
  
  \frac{\partial L}{\partial B_{k\cdot}}  & = - \sum_{i=1}^n x_{ik} ( y_{i\cdot} - \sum_{j\ne k}^p x_{ij} B_{j\cdot} - x_{ik} B_{k\cdot} ) + \lambda \left( (1-\alpha)B_{k\cdot} + \alpha \frac{B_{k\cdot}}{\parallel B_{k\cdot} \parallel_2} sgn(B_{k\cdot}) \right) \\
  
    & = - x_k^T ( Y - \sum_{j\ne k}^p x_{\cdot j} B_{j\cdot} )  + x_k^T x_k B_{k\cdot} + \lambda (1-\alpha) B_{k\cdot} + \lambda \alpha \frac{B_{k\cdot}}{\parallel B_{k\cdot} \parallel_2} sgn(B_{k\cdot}) ) \\\\
  
  
  \text{Since }B_{k\cdot} & =  (x_k^T x_k)^{-1} x_k^T ( Y - X_{(-k)} B_{(-k)} ), \\\\
  
  \frac{\partial L}{\partial B_{k\cdot}} & = - x_k^T ( Y - \sum_{j\ne k}^p x_{\cdot j} B_{j\cdot} )  + x_k^T x_k B_{k\cdot} + \lambda (1-\alpha) B_{k\cdot} + \lambda \alpha \frac{x_k^T (Y-X_{(-k)}B_{(-k)})} {\parallel x_k^T( Y-X_{(-k)}B_{(-k)} ) \parallel_2} sgn(B_{k\cdot}) ) \\
  
    \therefore \hat{B}_{k\cdot} & = \frac{1}{x_k^T x_k + \lambda(1-\alpha) }  { \left( 1 - \frac{ \lambda \alpha} {\parallel x_k^T (Y-X_{(-k)}B_{(-k)}) \parallel_2 }  \right )_+ x_k^T (Y-X_{(-k)} B_{(-k)}) }
  
  \end{align}
  $$

  

  - I cannot understand why $ B_{k\cdot} $ is replaced by the partial residual term.


  