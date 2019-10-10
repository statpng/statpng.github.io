---
layout: post
title:  "Workshop :: scRNA"
date:   2019-06-19
categories: RNA
permalink: bioinformatics
tags: RNA
use_math: true

# author
author: Kipoong Kim
---

<!-- more -->

## Single cell

1. Heterogeneity (Patient, Tumor, Cellular, Genomic, Epigenetic)
2. (a) Bulk sequencing, (b) Single cell sequencing, (c) Spatial analysis
3. Bulk sequencing analysis 한계점
4. Single cell DNA analysis를 하지않는 이유?
5. scRNA workflow, scRNA flatform
6. scRNA sequencing process
7. scRNA trends

# scRNA

- TCGA studies: 환자 안에서의 heterogeneity를 살펴 볼 수 없음 --> intra-tumor heterogeneity를 볼 수 없음.

- *Depth 40X: NGS platform으로 염기서열을 한번만 읽는게 아니라 여러번 읽음.*

- 조직에서 cell을 하나하나 분리하지 않고 수백가지 cell을 모아 녹여서 DNA를 얻는 Bulk seqeuncing은 어떤 Cell/클론에서 왔는지 알수 없음. --> full tumor 수준에서는 결과를 얻을 수 있으나, celluar 수준의 resolution을 얻기가 어렵다. 

  ** clone마다 stage가 다름.

  - bulk
  - single-cell
  - Image of tissue-slides

- 약물 반응성

  - 환자마다의 약물 resistance가 어디서 왔느냐?



![scRNA_1](D:\6.Github\statpng\statpng.github.io[sciblog]\img\scRNA_1.png)
  : Bulk sequencing뿐만 아니라 cell-type에 따른 시퀀싱 분석이 필요하다.

- Computational deconvolution techniques: bulk seqeuncing data를 가지고 cell-type을 추정함.



- scDNA 잘안하는 이유? cell마다 dna를 맵핑하기위해 amplification하는데 에러가 너무 많다.
  그러나 scRNA는 1~2 base pair에서 에러가 발생하더라도 transition은 크게 바뀌지않는다.

- 

![scRNA_2](D:\6.Github\statpng\statpng.github.io[sciblog]\img\scRNA_2.png)

- library 생산을 빨리 해야 cell이 죽지않고 cell profile이 바뀌지 않는다.

  그러나 보통 dissociation까지만 빨리하고 얼리고 다음 단계를 진행하는 추세.

- RNA을 complimentary하게 바꾼 cDNA



- 분석방법 비교

![scRNA_3](D:\6.Github\statpng\statpng.github.io[sciblog]\img\scRNA_3.png)

- Deconvolution 단점: (1) 추정된 ratio가 정확하지 않다. (2) Library에 포함되지 않은 cell-type의 경우 misesd cells로서 분류됨.
- 