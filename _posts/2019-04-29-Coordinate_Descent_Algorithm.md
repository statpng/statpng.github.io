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
  	*  http://stanford.edu/~boyd/index.html
  	*  http://hua-zhou.github.io/





## 2. The lasso

- ###  Derivation

  - The lasso solve the problem with constrain $\parallel \beta \parallel _1 < t$  using Lagrange multiplier,

    $$ \parallel y-X\beta \parallel _2^2 + \lambda \parallel \beta \parallel _1 , $$

    

    where $\parallel \cdot \parallel_1$ represents the $\ell _1$ norm. 

    

    ​	A derivation of estimate in the lasso is as follows.

$$
\begin{align} 
L 	& = \frac{1}{2N} \parallel y-X\beta \parallel _2 ^2 + \lambda \parallel \beta \parallel_1 \\
    	& = \frac{1}{2N} \sum_i ( y_i - \sum_j x_{ij} \beta_j )^2 + \lambda \sum_{j=1}^p |\beta_j| \\
    	& = \frac{1}{2N} \sum_i ( y_i - \sum_j x_{ij} \beta_j )^2 + \lambda \sum_{j=1}^p \beta_j sgn(\beta_j) \\
  
  \frac{\partial L}{\partial \beta_k} & = - \frac{1}{N} \sum_i x_{ik} (y_i - \sum_{j\ne k} x_{ij} \beta_j - x_{ik} \beta_k ) + \lambda sgn(\beta_k) \\
  	& = - \frac{1}{N} x_k^T (y-X_{(-k)}\beta_{(-k)} ) + \frac{1}{N} x_k^T x_k \beta_k + \lambda sgn(\beta_k) \\ \\
  
   
  \text{if  } \beta_k > 0, \text{  } \hat{\beta_k} & = (x_k^T x_k)^{-1} \left \{ x_k^T (y-X_{(-k)}\beta_{(-k)} ) - N \lambda \right \} _+  \\ \\
  
  \text{if  } \beta_k < 0, \text{  } \hat{\beta_k} & = (x_k^T x_k)^{-1} \left \{ - x_k^T (y-X_{(-k)}\beta_{(-k)} ) - N  \lambda \right \} _+  \\ \\
  
  
  \therefore \hat{\beta_k} & = (x_k^T x_k)^{-1} \left \{ | x_k^T (y-X_{(-k)}\beta_{(-k)} ) | - N  \lambda  \right \}_+ sgn(\beta_k)  \\
  
  
  \end{align}
  
$$

- Implemetation in C++

  ```c++
  
  #include <iostream>
  #include <RcppArmadillo.h>
  
  using namespace arma;
  using namespace std;
  using namespace Rcpp;
  
  // [[Rcpp::depends(RcppArmadillo)]]
  
  
  double soft_thresholding(double value){
    double out;
    if( value >= 0 ){
      out = value;
    } else {
      out = 0.0;
    }
    
    return out;
  }
  
  // [[Rcpp::export]]
  
  arma::mat thelasso(mat X, mat y, int niter, double lambda){
    
    int p = X.n_cols;
    int N = y.n_rows;
    mat betak;
    
    mat beta(X.n_cols, 1);
    
    mat Diag_lambda (p, p);
    Diag_lambda.fill(0);
    for( int j=0; j<p; j++ ){
      Diag_lambda(j,j) = 0.1;
    }
      
    beta = solve((X.t()*X + Diag_lambda), X.t() * y );
    for( int i=0; i<niter; i++ ){
  
      for( int k=0; k<(int)X.n_cols; k++ ){
        
        mat Xmk;
        mat betamk;
        
        Xmk = X;
        betamk = beta;
        mat Xk = X.col(k);
        
        Xmk.col(k).fill(0);
        betamk.row(k) = 0;
        
        double betak_hat = arma::as_scalar( Xk.t() * (y - Xmk * betamk ) );
        // cout << betak_hat << endl;
        
        if( betak_hat > 0 ){
          betak = soft_thresholding(abs(betak_hat) - N*lambda) / arma::as_scalar(Xk.t() * Xk);
        } else {
          betak = -soft_thresholding(abs(betak_hat) - N*lambda) / arma::as_scalar(Xk.t() * Xk);
        }   
        beta.row(k) = betak;
      }
    }
    return beta; 
  }
  
  ```

  

  


## 3. Ridge

- ### Derivation

$$
\begin{align}
  L & = \frac{1}{2N} \parallel y-X\beta \parallel_2^2 + \frac{\lambda}{2} \parallel \beta \parallel_2^2 \\
   & = \frac{1}{2N} \sum_{i=1}^{n} \left( y_i - \sum_{j=1}^p x_{ij} \beta_j \right)^2 + \frac{\lambda}{2} \sum_{j=1}^p \beta_j^2 \\
  
   \frac{ \partial L }{\partial \beta_k} & = - \frac{1}{N} \sum_{i=1}^n x_{ik} \left( y_i - \sum_{j\ne k}x_{ij} \beta_j - x_{ik}\beta_k \right) + \lambda \beta_k \\
   & = - \frac{1}{N} x_k^T(y-X_{(-k)}\beta_{(-k)} ) + \frac{1}{N} x_k^Tx_k \beta_k + \lambda \beta_k \\
  
   \therefore \hat{\beta_k} & = ( \frac{1}{N} x_k^T x_k + N \lambda )^{-1}x_k^T (y-X_{(-k)}\beta_{(-k)}).
  
    \end{align}
$$


- Implementation in C++

  ```c++
  #include <iostream>
  #include <RcppArmadillo.h>
  
  using namespace arma;
  using namespace std;
  using namespace Rcpp;
  
  // [[Rcpp::depends(RcppArmadillo)]]
  // [[Rcpp::export]]
  arma::mat ridge_png(mat X, mat y, int niter, double lambda, double tol){
    
    int p = X.n_cols;
    int N = X.n_rows;
    mat betak;
    
    mat beta(X.n_cols, 1);
    
    mat Diag_lambda (p, p);
    Diag_lambda.fill(0);
    for( int j=0; j<p; j++ ){
      Diag_lambda(j,j) = 0.1;
    }
    
    mat fitted_values_old, fitted_values_new;
    
    beta = solve((X.t()*X + Diag_lambda), X.t() * y );
    while( convergence_value > tol ){
      
      fitted_values_old = X * beta;
      
      for( int k=0; k<(int)X.n_cols; k++ ){
        
        mat Xmk;
        mat betamk;
        
        Xmk = X;
        betamk = beta;
        mat Xk = X.col(k);
        
        Xmk.col(k).fill(0);
        betamk.row(k) = 0;
        
        double betak_hat = arma::as_scalar( Xk.t() * (y - Xmk * betamk ) );
        // cout << betak_hat << endl;
        
        betak = betak_hat / arma::as_scalar(Xk.t() * Xk + 158.8954*lambda );
        
        beta.row(k) = betak;
      }
        
      fitted_values_new = X * beta;
      
      double convergence_value=0.0;
      for( int h=0; h<(int)y.n_rows; h++ ){
        convergence_value += arma::as_scalar( (fitted_values_old.row(h)-fitted_values_new.row(h)) * 
          (fitted_values_old.row(h)-fitted_values_new.row(h)) ) / N ;
      } 
    }
  
    return beta; 
  }
  ```

  


## 4. Elastic-net

- ### Derivation

$$
\begin{align}

  L & = \frac{1}{2N} \parallel y-X\beta \parallel_2^2 + \lambda \left( \frac{1-\alpha}{2} \parallel \beta \parallel_2^2 + \alpha \parallel \beta \parallel _1 \right) \\

  & = \frac{1}{2N} \sum_{i=1}^n \left( y_i - \sum_{j=1}^px_{ij}\beta_j \right)^2 + \lambda \left( \frac{1-\alpha}{2} \sum_{j=1}^p \beta_j^2 + \alpha \sum_{j=1}^p | \beta_j | \right) \\

  \frac{\partial L}{\partial \beta_k}  & = - \frac{1}{N} \sum_{i=1}^n x_{ik} \left( y_i - \sum_{j\ne k}^p x_{ij} \beta_j - x_{ik} \beta_k \right) + \lambda \left( (1-\alpha)\beta_k + \alpha  sgn(\beta_k) \right) \\

    & = - \frac{1}{N} x_k^T (y - X_{(-k)} \beta_{(-k)}) + \frac{1}{N} x_k^T x_k \beta_k + \lambda \left( (1-\alpha)\beta_k + \alpha sgn(\beta_k) \right) \\ \\
    
    \therefore \hat{\beta_k} & = \frac{ \left( | x_k^T(y-X_{(-k)}\beta_{(-k)}) | - N \lambda \alpha  \right )_+ sgn(\beta_k) }{x_k^T x_k + N \lambda(1-\alpha) }

  



  \end{align}
$$

- Implementation in C++ (Standardized predictor with intercept)

  ```c++
  
  #include <iostream>
  #include <RcppArmadillo.h>
  
  using namespace arma;
  using namespace std;
  using namespace Rcpp;
  
  // [[Rcpp::depends(RcppArmadillo)]]
  
  double CalcMSE(arma::mat oldY, arma::mat newY){
    int N = oldY.n_rows;
    double MSE=0.0;
    for( int h=0; h<N; h++ ){
      MSE += arma::as_scalar( (oldY.row(h)-newY.row(h)) * 
        (oldY.row(h)-newY.row(h)) ) / N;
    }
    return MSE;
  }
  
  double soft_thresholding(double value){
    double out;
    if( value >= 0 ){
      out = value;
    } else {
      out = 0.0;
    }
    return out;
  }
  
  mat standardize(mat XX){
    int N = XX.n_rows;
    int p = XX.n_cols;
    mat SDofX(p, 1);
    
    for( int jj=0; jj<p; jj++ ){
      double meanX = 0.0;
      double sqmeanX = 0.0;
      for( int ii=0; ii<N; ii++ ){
        meanX += XX(ii,jj)/N;
        sqmeanX += pow(XX(ii,jj), 2)/N;
      }
      SDofX.row(jj) = sqrt( sqmeanX - pow(meanX,2) );
      XX.col(jj) = ( XX.col(jj) - meanX ) / arma::as_scalar(SDofX.row(jj));
    }
    
    return XX;
  }
  
  // [[Rcpp::export]]
  arma::mat elnet_png(mat X, mat y, double lambda, double alpha, double tol){
    
    int p = X.n_cols;
    int N = X.n_rows;
    double meanY = 0.0;
    double sqmeanY = 0.0;
    double SDofY;
    
    // mat I = arma::eye<mat>(N, N);
    // mat J(N, N);
    // J.fill(1/N);
    // X.insert_cols( 0, ones<vec>(N) );
    mat SDofX(p, 1);
    mat MEANofX(p, 1);
    mat SQofX(p, 1);
    
    for( int jj=0; jj<p; jj++ ){
      double meanX = 0.0;
      double sqmeanX = 0.0;
      for( int ii=0; ii<N; ii++ ){
        meanX += X(ii,jj)/N;
        sqmeanX += pow(X(ii,jj), 2)/N;
      }
      SQofX.row(jj) = sqmeanX;
      MEANofX.row(jj) = meanX;
      SDofX.row(jj) = sqrt( sqmeanX - pow(meanX,2) );
      X.col(jj) = ( X.col(jj) - meanX ) / arma::as_scalar(SDofX.row(jj));
    }
     
    for( int ii=0; ii<N; ii++ ){
      meanY += arma::as_scalar( y.row(ii) )/N;
      sqmeanY += pow( arma::as_scalar( y.row(ii) ), 2 )/N;
    }
    
    SDofY = sqrt(sqmeanY-pow(meanY, 2));
    y = (y-meanY) / SDofY;
    
    mat betak;
    mat beta(X.n_cols, 1);
    
    mat Diag_lambda (p, p);
    Diag_lambda.fill(0);
    for( int j=0; j<p; j++ ){
      Diag_lambda(j,j) = 0.1;
    }
    
    mat fitted_values_old, fitted_values_new(N, 1);
    beta = solve((X.t()*X + Diag_lambda), X.t() * y );
    
    double convergence_value = tol+1.0;
    while( convergence_value > tol ){
      
      fitted_values_old = X * beta;
      
      for( int k=0; k<(int)X.n_cols; k++ ){
        
        mat Xmk;
        mat betamk;
        
        Xmk = X;
        betamk = beta;
        mat Xk = X.col(k);
        
        Xmk.col(k).fill(0);
        betamk.row(k) = 0;
        
        double betak_hat = arma::as_scalar( Xk.t() * (y - Xmk * betamk ) );
        if( betak_hat > 0 ){
          betak = soft_thresholding(abs(betak_hat) - N*lambda*alpha) / arma::as_scalar(Xk.t() * Xk + N * lambda * (1.0-alpha) );
        } else {
          betak = - soft_thresholding(abs(betak_hat) - N*lambda*alpha) / arma::as_scalar(Xk.t() * Xk + N * lambda * (1.0-alpha) );
        }
        beta.row(k) = betak;
      }
      
      fitted_values_new = X * beta;
      
      convergence_value = CalcMSE(fitted_values_old, fitted_values_new);
        
    }
    
    cout << "The convergence value is = " <<
      convergence_value << endl;
    
    cout << "Beta = " << beta << endl;
    
   mat unstd_beta(p,1);
    double intercept=meanY;
    for( int jj=0; jj<p; jj++ ){
      unstd_beta.row(jj) = ( beta.row(jj) * (SDofY/SDofX.row(jj)) ) ;
      intercept += - arma::as_scalar( unstd_beta.row(jj)*MEANofX.row(jj) );
    }
    
    cout << "intercept = " << intercept << endl;
    
    unstd_beta.insert_rows(0, intercept);
    
    return unstd_beta;
  }
  
  ```

- Implementation in C++ (Full version which is confirmed for only centered predictors)

  ```c++
  #include <iostream>
  #include <RcppArmadillo.h>
  
  using namespace arma;
  using namespace std;
  using namespace Rcpp;
  
  // [[Rcpp::depends(RcppArmadillo)]]
  
  double CalcMSE(arma::mat oldY, arma::mat newY){
    int N = oldY.n_rows;
    double MSE=0.0;
    for( int h=0; h<N; h++ ){
      MSE += arma::as_scalar( (oldY.row(h)-newY.row(h)) * 
        (oldY.row(h)-newY.row(h)) ) / N;
    }
    return MSE;
  }
  
  double soft_thresholding(double value){
    double out;
    if( value >= 0 ){
      out = value;
    } else {
      out = 0.0;
    }
    return out;
  }
  
  mat scaling(mat XX){
    int N = XX.n_rows;
    int p = XX.n_cols;
    mat SDofX(p, 1);
    
    for( int jj=0; jj<p; jj++ ){
      double meanX = 0.0;
      double sqmeanX = 0.0;
      for( int ii=0; ii<N; ii++ ){
        meanX += XX(ii,jj)/N;
        sqmeanX += pow(XX(ii,jj), 2)/N;
      }
      SDofX.row(jj) = sqrt( sqmeanX - pow(meanX,2) );
      XX.col(jj) = ( XX.col(jj) - meanX ) / arma::as_scalar(SDofX.row(jj));
    }
    
    return XX;
  }
  
  // [[Rcpp::export]]
  arma::mat elnet_png(mat X, mat y, double lambda, double alpha, double tol, bool standardize, bool intercept){
    
    int p = X.n_cols;
    int N = X.n_rows;
    double meanY = 0.0;
    double sqmeanY = 0.0;
    double SDofY;
    
    // mat I = arma::eye<mat>(N, N);
    // mat J(N, N);
    // J.fill(1/N);
    // X.insert_cols( 0, ones<vec>(N) );
    
    mat SDofX(p, 1);
    mat MEANofX(p, 1);
    mat SQofX(p, 1);
    
    if( standardize ){
      for( int jj=0; jj<p; jj++ ){
        double meanX = 0.0;
        double sqmeanX = 0.0;
        for( int ii=0; ii<N; ii++ ){
          meanX += X(ii,jj)/N;
          sqmeanX += pow(X(ii,jj), 2)/N;
        }
        SQofX.row(jj) = sqmeanX;
        MEANofX.row(jj) = meanX;
        SDofX.row(jj) = sqrt( sqmeanX - pow(meanX,2) );
        X.col(jj) = ( X.col(jj) - arma::as_scalar( MEANofX.row(jj) ) ) / arma::as_scalar(SDofX.row(jj));
      }
    } else {
      MEANofX.fill(0);
      SDofX.fill(1);
    }
    
    if( intercept ){
      X.insert_cols(0, ones<mat>(N, 1));
      MEANofX.insert_rows(0, zeros<mat>(1,1));
      SDofX.insert_rows(1, ones<mat>(1,1));
      p = X.n_cols;
    }
    
     
    for( int ii=0; ii<N; ii++ ){
      meanY += arma::as_scalar( y.row(ii) )/N;
      sqmeanY += pow( arma::as_scalar( y.row(ii) ), 2 )/N;
    }
    
    SDofY = sqrt(sqmeanY-pow(meanY, 2));
    y = (y-meanY) / SDofY;
    lambda = lambda/SDofY;
    
    mat betak;
    mat beta(X.n_cols, 1);
    
    mat Diag_lambda (p, p);
    Diag_lambda.fill(0);
    for( int j=0; j<p; j++ ){
      Diag_lambda(j,j) = 0.1;
    }
    
    mat fitted_values_old, fitted_values_new(N, 1);
    beta = solve((X.t()*X + Diag_lambda), X.t() * y );
    
    cout << "MEANofX = " << MEANofX << endl;
    cout << "SDofX = " << SDofX << endl;
    cout << "beta = " << beta << endl;
    
    int conv_count=0;
    double convergence_value = tol+1.0;
    while( convergence_value > tol ){
      
      conv_count += 1;
      cout << conv_count << endl;
      
      fitted_values_old = X * beta;
      
      for( int k=0; k<(int)X.n_cols; k++ ){
        
        mat Xmk;
        mat betamk;
        
        Xmk = X;
        betamk = beta;
        mat Xk = X.col(k);
        
        Xmk.col(k).fill(0);
        betamk.row(k) = 0;
        
        double betak_hat = arma::as_scalar( Xk.t() * (y - Xmk * betamk ) );
        if( betak_hat > 0 ){
          betak = soft_thresholding(abs(betak_hat) - N*lambda*alpha) / arma::as_scalar(Xk.t() * Xk + N * lambda * (1.0-alpha) );
        } else {
          betak = - soft_thresholding(abs(betak_hat) - N*lambda*alpha) / arma::as_scalar(Xk.t() * Xk + N * lambda * (1.0-alpha) );
        }
        beta.row(k) = betak;
      }
      
      fitted_values_new = X * beta;
      
      convergence_value = CalcMSE(fitted_values_old, fitted_values_new);
        
    }
    
    cout << "The convergence value is = " <<
      convergence_value << endl;
    
    cout << "Beta = " << endl << beta << endl;
  
    mat unstd_beta(p,1);
    for( int jj=0; jj<p; jj++ ){
      unstd_beta.row(jj) = ( beta.row(jj) * (SDofY/SDofX.row(jj)) ) ;
    }  
  
    cout << "UnstdBeta = " << endl << unstd_beta << endl;
    
    if( intercept ){
      unstd_beta.row(0) = unstd_beta.row(0) + meanY;
    } else {
      unstd_beta.insert_rows(0, zeros<mat>(1,1));
    }
      
    double intcpt;
    if( intercept ){
      // mat unstd_beta(p,1);
      intcpt = meanY;
      for( int jj=1; jj<p; jj++ ){
        // unstd_beta.row(jj) = ( beta.row(jj) * (SDofY/SDofX.row(jj)) ) ;
        unstd_beta.row(0) += - arma::as_scalar( unstd_beta.row(jj)*MEANofX.row(jj) );
        intcpt += - arma::as_scalar( unstd_beta.row(jj)*MEANofX.row(jj) );
      }
    
      cout << "intercept = " << intcpt << endl;
      
    } else {
      intcpt = 0.0;
    }
    
    // mat out(p+1, 1);
    // out.rows(1, p) = unstd_beta;
    // out.rows(0, 0) = intcpt;
    
    return unstd_beta;
    
  }
  ```

  




## 5. Block-wise coordinate descent (for multi-response elastic-net)

- #### Let us consider a framework to simulate multivariate multiple regression

  $$
  Y = XB+W,
  $$

  where $ W \sim MVN(0, \Sigma_Y) $, and $\Sigma_Y$ is an equicorrelation matrix with $\rho_y=0, 0.2, 0.5, 0.8$.

- ### Derivation of estimator for multi-response elastic-net

$$
\begin{align}

  L & = \frac{1}{2N} \parallel Y-XB \parallel_2^2 + \lambda \left( \frac{1-\alpha}{2} \parallel B \parallel_F^2 + \alpha \sum_{j=1}^p \parallel B_{j\cdot} \parallel_2 \right) \\

  & = \frac{1}{2N} \sum_{i=1}^n \parallel y_{i\cdot} - \sum_{j=1}^p x_{ij} B_{j\cdot} \parallel_2^2 + \lambda \left( \frac{1-\alpha}{2} \sum_{j=1}^p \parallel B_{j\cdot} \parallel_2^2 + \alpha \sum_{j=1}^p \parallel B_{j\cdot} \parallel_2 \right) \\


  \frac{\partial L}{\partial B_{k\cdot}}  & = - \frac{1}{N} \sum_{i=1}^n x_{ik} ( y_{i\cdot} - \sum_{j\ne k}^p x_{ij} B_{j\cdot} - x_{ik} B_{k\cdot} ) + \lambda \left( (1-\alpha)B_{k\cdot} + \alpha \frac{B_{k\cdot}}{\parallel B_{k\cdot} \parallel_2} sgn(B_{k\cdot}) \right) \\

    & = - \frac{1}{N} x_k^T ( Y - \sum_{j\ne k}^p x_{\cdot j} B_{j\cdot} )  + \frac{1}{N} x_k^T x_k B_{k\cdot} + \lambda (1-\alpha) B_{k\cdot} + \lambda \alpha \frac{B_{k\cdot}}{\parallel B_{k\cdot} \parallel_2} sgn(B_{k\cdot} ) \\\\

  \text{Since }B_{k\cdot} & =  (x_k^T x_k)^{-1} x_k^T ( Y - X_{(-k)} B_{(-k)} ), \\\\

  \frac{\partial L}{\partial B_{k\cdot}} & = - \frac{1}{N} x_k^T ( Y - \sum_{j\ne k}^p x_{\cdot j} B_{j\cdot} )  + \frac{1}{N} x_k^T x_k B_{k\cdot} + \lambda (1-\alpha) B_{k\cdot} + \lambda \alpha \frac{x_k^T (Y-X_{(-k)}B_{(-k)})} {\parallel x_k^T( Y-X_{(-k)}B_{(-k)} ) \parallel_2} sgn(B_{k\cdot}) ) \\

    \therefore \hat{B}_{k\cdot} & = \frac{1}{x_k^T x_k + N \lambda(1-\alpha) }  { \left( 1 - \frac{ N \lambda \alpha} {\parallel x_k^T (Y-X_{(-k)}B_{(-k)}) \parallel_2 }  \right) _+ x_k^T (Y-X_{(-k)} B_{(-k)}) }

  \end{align}
$$

​		- I cannot understand why $ B_{k\cdot} $ could be replaced by the partial residual term.

  * ### **Remark**

    If $  B_{j1} = \cdots = B_{jM}=b, \text{ where } j=1,\cdots,p $, then

    $$
    \begin{align}
       \parallel B_{j\cdot} \parallel_2 
       & = \sqrt{\sum_{m=1}^{M} B_{jm}^2} \\
       & = \sqrt{ M \times b^2 } \\
       & = \sqrt{M} \times \lvert b \rvert \\
       & = \sum_{m=1}^M \frac{\lvert b \rvert}{\sqrt{M}} \\
       \therefore \sum_{j=1}^p\parallel B_{j\cdot} \parallel_2 
       & = \sum_{m=1}^M \frac{1}{\sqrt{M}} \parallel B_{\cdot m} \parallel_1
       
    \end{align}
    $$

    where $ m=1,\cdots,M $.

    When $ \rho_y $ is high and $\gamma=M$(the number of response variables), the assumption of $  B_{j1} = \cdots = B_{jM}=b $ can be satisfied.

    The objective function can be expressed as

    $$
        L = \sum_{m=1}^M \left\{ \frac{1}{2N} \parallel Y_1 - XB_{\cdot m} \parallel_2^2  + \lambda ( \frac{1-\alpha}{2}\parallel B_{\cdot m} \parallel_2^2 + \frac{\alpha}{\sqrt{M}} \parallel B_{\cdot m} \parallel_1 ) \right\}. \\
    $$

    This yields the results of selection probabilities equivalent to univariate elastic-net. 

    

* <!--On the other hand, when $ \rho_y $ is low and $\gamma=M$,  the causal variants in mgaussian are more frequently selected than in univariate elastic-net as $ \sqrt{\frac{1}{M} \sum{m=1}^M B{jm}^2} \ge  B_{jm} $-->

  

  <!--In the case where at least one coefficient $ B_{jm} $ differs from the other responses, the causal regression coefficients were selected more often as the denominator $ \parallel x_k^T (Y-X_{(-k)}B_{(-k)}) \parallel_2 (= \parallel B_{k \cdot} \parallel_2 )$ of *mgaussian* is larger than that of univariate elastic-net from this:--> 

  <!--
  $$
  \hat{B}_{k\cdot} = \frac{1}{x_k^T x_k + N \lambda(1-\alpha) }  { \left( 1 - \frac{ N \lambda \alpha} {\parallel x_k^T (Y-X_{(-k)}B_{(-k)}) \parallel_2 }  \right) _+ x_k^T (Y-X_{(-k)} B_{(-k)}) }
  $$
  -->



  * ### Implementation in C++

```

```