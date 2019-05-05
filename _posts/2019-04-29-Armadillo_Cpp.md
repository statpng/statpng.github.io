---
layout: post
title:  "C++ :: Armadillo library in C++"
date:   2019-04-26
categories: C++
permalink: Armadillo_Cpp
tags: C++

# author
author: Kipoong Kim
---

<!-- more -->

# Armadillo

<https://youtu.be/w7VpGVmphC4>

## 1. 행렬 기초 명령어

### (1) Notification

- 아래의 모든 코드는 Rcpp을 이용하였으며, 아래의 header를 모두 포함하여야한다.

  ```c++
  #include <RcppArmadillo.h>
  // [[Rcpp::depends(RcppArmadillo)]]
  
  using namespace arma;
  using namespace std;
  
  // [[Rcpp::export]]
  ```



### (2) 행렬 생성

```c++
int main()
{
  mat A;  // initialize
  A << -1 << 2 << endr  // input
    << 3 << 5;
  cout << A << endl;  // printout
  
  fmat B;  // float matrix
  B.randu(4, 5);  // input random uniform values
  cout << B << endl;
  
  B.reshape(6, 8);  // reshape by column
  cout << B << endl << endl;
  
  fmat X;
  X.randu(1, 8);
  B.insert_rows(0, X);  // insert X to the first row of B
  cout << B << endl;
  
  mat x;
  x.randu(3, 3);
  
  cout << x << endl;
  cout << "Rows = " << x.n_rows << endl  // nrow
       << "COlumns = " << x.n_cols << endl  // ncol
       << "Elements = " << x.n_elem << endl;  // length
  
  cout << x.diag(0) << endl;  // diag(x). The argument represent that which diagonal entries will be printed out far from diagonal elements.
  
  return 0;
}
```



- 구현코드
- 

```c++
int main2()
{
  mat A;
  A.randu(3, 3);
  
  mat B;
  mat C;
  mat D;
  
  B.zeros(3, 1);  // generate a zero vector of length 3
  C.ones(1, 2);  // generate a vector of 1 values
  D.zeros(1, 1);  // 1 constant
  
  A.diag(0) = B;
  A.diag(1) = C;
  A.diag(-2) = D;
  cout << A;
  
  return 0;
}
```





- 
  RcppArmadillo examples
  [reference](<https://www.asc.ohio-state.edu/physics/ntg/6810/readings/hjorth-jensen_notes2012_06.pdf>)

```c++
#include <iostream>
#include <RcppArmadillo.h>

using namespace arma;
using namespace std;

// [[Rcpp::depends("RcppArmadillo")]]
// [[Rcpp::export]]
int main()
{
  cout << "Armadillo version: " << arma_version::as_string() << endl;
  mat A;
  // Hard coding of the matrix
  // endr indicates "end of row"
  A << 0.165300 << 0.454037 << 0.995795 << 0.124098 << 0.047084 << endr
  << 0.688782 << 0.036549 << 0.552848 << 0.937664 << 0.866401 << endr
  << 0.348740 << 0.479388 << 0.506228 << 0.145673 << 0.491547 << endr
  << 0.148678 << 0.682258 << 0.571154 << 0.874724 << 0.444632 << endr
  << 0.245726 << 0.595218 << 0.409327 << 0.367827 << 0.385736 << endr;
  // .n_rows = number of rows
  // .n_cols = number of columns
  cout << "A.n_rows = " << A.n_rows << endl;
  cout << "A.n_cols = " << A.n_cols << endl;
  // Print the matrix A
  A.print("A =");
  // Computation of the determinant
  cout << "det(A) = " << det(A) << endl;
  // inverse
  cout << "inv(A) = " << endl << inv(A) << endl;
  // save to disk
  A.save("MatrixA.txt", raw_ascii);
  // Define a new matrix B which reads A from file
  mat B;
  B.load("MatrixA.txt");
  B += 5.0*A;
  B.print("The matrix B:");
  // transpose of B
  cout << "trans(B) =" << endl;
  // maximum from each column (traverse along rows)
  cout << "max(B) =" << endl;
  cout << max(B) << endl;
  // sum of all elements B
  cout << "sum(B) = " << sum(B) << endl;
  cout << "sum(sum(B)) = " << sum(sum(B)) << endl;
  cout << "accu(B) = " << accu(B) << endl;
  // trace = sum along diagonal
  cout << "trace(B) = " << trace(B) << endl;
  // generate the identity matrix
  mat C = eye<mat>(5,5);
  C.print("Matrix C:");
  // random matrix -- values are uniformly distributed in the [0,1] interval
  mat D = randu<mat>(5,5);
  D.print("Matrix D:");
  // sum of four matrices (no temporary matrices are created)
  mat E = A+B + C + D;
  E.print("E:");
  return 0;
}
```

