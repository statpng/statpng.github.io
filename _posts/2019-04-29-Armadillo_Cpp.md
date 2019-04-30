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
