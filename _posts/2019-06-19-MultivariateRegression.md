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



## 1. Basic Matrix Operation

### Basic Matrix Operation

- Let’s briefly review the basic matrix operation and results for vectors and matrices.



- If we denote $L, R, Q$ be the lower-triangular, upper-triangular, and orthogonal matrix, then
  - LR decomposition: $A = LR$
  - Cholesky decomposition: $A = LL^T$
  - QR decomposition: $A = QR$
  
- If $A$ and $B$ are $(J \times J)$ matrices, then
  $$
  \lvert AB \rvert = \lvert A \rvert \lvert B \rvert
  $$
  
- $ \lvert Q \rvert = 1 $
  
- If 
  $$
  \Sigma = \left( \begin{matrix} A & B \\ C & D \\ 
  \end{matrix} \right)
  $$
and A, B are both square and non-singular, then
  $$
  \lvert \Sigma \rvert = \lvert A \rvert \cdot \lvert D-CA^{-1}B \rvert = \lvert A \rvert \cdot \lvert A-BD^{-1}C \rvert
  $$
  
- The rank of $A$, denoted $r(A)$, is the size of the largest submatrix of $A$ that has a nonzero determinant.

  Note that $r(AB) = r(A)$ if $\lvert B \rvert \ne 0$, and, in general, $r(AB) \le \text{min} \left( r(A), r(B) \right) $.

  If $A$ is square, $(J \times J)$, and nonsingular, then a unique $(J \times J)$ *inverse matrix* $A^{-1}$ exists such that $AA^{-1}=I_J$. 

  If $A$ is orthogonal, then $A^{-1}=A^T$. 
  Note that $(AB)^{-1}=B^{-1}A^{-1}$, and $\lvert A^{-1} \rvert = \lvert A \rvert ^{-1}$.

- $$
  (A+BD^{-1}C)^{-1} = A^{-1} - A^{-1}B(D+CA^{-1}B)^{-1}CA^{-1}
  $$

  

  where, $A$ and $D$ are $(J \times J)$ and $(K \times K)$ nonsingular matrices, respectively.
  
  In special case of this result, if $A$ is $(J \times J)$ and **u** and **v** are $J$-vectors, then
  $$
(A+\boldsymbol{uv}^T)^{-1} = A^{-1} - \frac{(A^{-1}\boldsymbol{u})(\boldsymbol{v}^TA^{-1})}{1+\boldsymbol{v}^TA^{-1}\boldsymbol{u}}.
  $$
  
- If $A$ and $D$ are symmetric matrices and $A$ is nonsingular, then,
  $$
  \left( \begin{matrix}
  A & B \\ B^T & D
  \end{matrix} \right) ^{-1} = 
  \left( \begin{matrix}
  A^{-1} + FE^{-1}F^T & -FE^{-1} \\ -EF^T & E^{-1}
  \end{matrix} \right)
  $$
  
  
  where $E = D-B^T A^{-1} B$ is nonsingular and $F = A^{-1} B$.

### Vectoring and Kronecker Products

- vec(**A**) denotes the $(JK \times 1)$-column vector formed <u>by placing the columns of a $(J\times K)$-matrix A under one another successively</u>.
  
- If a $(J \times K)$-matrix $A$ is such that the $jk$th element $A_{jk}$ is itself a submatrix, then $A$ is termed a *block matrix*. 
  The *Kronecker product* of a $(J \times K)$-matrix $A$ and an $(L \times M)$-matrix B is the $(JL \times KM)$ block matrix
  $$
  A \otimes B = (\boldsymbol{A}B_{jk}) = 
   \left( \begin{matrix} 
   \boldsymbol{A}B_{11} & \cdots & \boldsymbol{A}B_{1M} \\ 
   \vdots & & \vdots \\
   \boldsymbol{A}B_{L1} & \cdots & \boldsymbol{A}B_{LM} \\
   \end{matrix} \right).
  $$

  
  This is commonly known as the *left Kronecker product*.
  
- The following operation hold for *Kronecker products*:
  
  $$
  \begin{align}
  (A \otimes B)\otimes C ~~&=~~ A \otimes (B \otimes C) \\
  (A \otimes B ) (C \otimes D) ~~&=~~ (AC) \otimes (BD) \\
  (A+B) \otimes C ~~&=~~ (A \otimes C ) + (B \otimes C) \\
  (A \otimes B)^T ~~&=~~ A^T \otimes B^T \\
  \text{tr}(A \otimes B ) ~~&=~~ (\text{tr}(A))(\text{tr}(B)) \\
  r(A \otimes B ) ~~&=~~ r(A) \cdot r(B) \\
  \end{align}
  $$
  
  If $A$ is $(J \times J)$ and $B$ is $(K \times K)$, then
  $$
  \lvert A \otimes B \rvert = \lvert A \rvert ^K \lvert B \rvert ^J
  $$
  
  If $A$ is $(J \times K)$ and $B$ is $(L \times M)$, then
  $$
  A \otimes B = (A \otimes I_L ) ( I_K \otimes B )
  $$
  
  If $A$ and $B$ are square and nonsingular, then, 
  $$
  (A \otimes B)^{-1} = A^{-1} \otimes B^{-1}
  $$
  
  One of the most useful results that combines vectoring with *Kronecker products* is that
  $$
  \text{vec}(ABC) = (A \otimes C^T ) \text{vec}(B).
  $$



### Eigenanalysis for Square Matrices

- If $A$ is a $(J \times J)$-matrix, then $\lvert A - \lambda I_J \rvert $ is a polynomial of order $J$ in $\lambda$. 
  The equation 
  $$
  \lvert A - \lambda I_J \rvert = 0
  $$
  will have $J$ (possibly complex-valued) roots denoted by $\lambda_j = \lambda_j (A), ~ j=1, 2, \cdots, J$. 
  
  The root $\lambda_j$ is called the *eigenvalue (characteristic root, latent root)* of $A$, and the set $\{\lambda_j \}$ is called the *spectrum* of $A$. 
  

Associated with $\lambda_j$, there is a $J$-vector $v_j=v_j(A)$ (not all of whose entries of zero) such that

$$
  (A - \lambda_j I_J) \text{v}_j = 0.
$$

  The vector $\text{v}_j$ is called *eigenvector (characteristic vector, latent vector)* associated with $\lambda_j$. 

- Eigenvectors $\text{v}_j$ and $\text{v}_k$ associated with distinct eigenvalues $(\lambda_j \ne \lambda_k)$ are orthogonal. 
  If $V = (v_1, v_2, \cdots, v_J)$, then
  
  $$
  AV = V\Lambda
  $$
where $\Lambda = \text{diag} ( \lambda_1, \lambda_2, \cdots, \lambda_J ) $ is a matrix with the eigenvalues along the diagonal and zeros elsewhere, and $V^T V = I_J$.
  
- The “outer product” of a $J$-vector $\text{v}$ with itself is the $(J\times J)$-matrix $\text{v} \text{v}^T$, which has rank 1.

  The *spectrum theorem* expresses the $(J\times J)$-matrix $A$ as a weighted average of rank-1 matrices,

  $$
  A = V \Lambda V^T = \sum_{j=1}^J \lambda_j v_j v_j ^T,
  $$
  

  where $I_J = \sum_{j=1} ^J v_j v_j ^T$, and where the weights, $\lambda_1, \cdots, \lambda_J$, are the eigenvalues of $A$.
  
- The rank of $A$ is the number of nonzero eigenvalues, the trace is
  $$
  \text{tr}(A) = \sum_{j=1}^J \lambda_j(A),
  $$
  
  
  and the determinant is
  $$
  \lvert A \rvert = \prod_{j=1} ^J \lambda_j(A)
  $$

### Functions of Matrices

- If $A$ is a symmetric $(J\times J)$-matrix and $\phi : R^J \rightarrow R^J$ is a function, then
  $$
  \phi (A) = \sum_{j=1}^J \phi(\lambda_j)v_j v_j ^T,
  $$
where $\lambda_j$ and $v_j$ are the $j$th eigenvalue and corresponding eigenvector, respectively, of $A$.
  
  
  
- Examples include the following:
  $$
  \begin{align}
  A^{-1} & = V \Lambda ^{-1} V^T = \sum_{j=1}^J \lambda_j ^{-1} v_j v_j ^T, \text{ if $A$ is nonsingular} \\
  A^{1/2} & = V \Lambda ^{1/2} V^T = \sum_{j=1}^J \lambda_j ^{1/2} v_j v_j ^T \\
  \text{log}(A) & = \sum_{j=1}^J (\text{log}(\lambda_j)) v_j v_j ^T, \text{ if $\lambda_j \ne 0,$ all $j$} \\
  \end{align}
  $$
  
  
  Hence, $\lambda_j (\phi (A)) = \phi ( \lambda_j (A) )$ and $v_j (\phi (A)) = v_j (A)$. 





## 2. Multivariate regression

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

