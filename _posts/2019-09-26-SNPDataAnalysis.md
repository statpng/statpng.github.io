---
layout: post
title:  "Genetics:: SNPDataAnalysis"
date:   2019-09-26
categories: category
permalink: permalink
tags: tags
use_math: true

# author
author: Kipoong Kim
---

<!-- more -->

# Analysis pipeline for SNP data

### Single Nucleotide Polymorphism (SNP)

- There are many types of NGS dataset such as Whole Genome Sequencing (WGS), Whole Exome Sequencing (WES), RNA-seq, ChIP-seq. We'll see how to do pre-processing for the DNA sequencing data with high quality.

  >SNP: DNA sequence variations that occur when a single nucleotide (A, G, C, or T) in the genome sequence is changed.

  

### An outline for analysis  of genomic data is as follows:

![GenomicAnalysisPipeline](C:\Users\png\Desktop\GenomicAnalysisPipeline.gif)



### Preprocessing

#### 1. Sample quality control

- Total sample size N = ___

  Removing duplication --> N = ___

  Removing missing proportion > 10% --> N = ___

  


#### 2. Variants quality control

- Chromosome: 1 - 22 (or including X, Y)

- Total number of SNPs *p*= ___

  * Filtering out SNPs 
    with a **call rate** < (75%, <u>95%</u>) ,
    not in **HWE** (P < (<u>10e−7</u>, 10e-6) in controls or P<10E−12 in cases),
    with **heterozygous call** (heterogeneity) > (<u>10%</u>, 20%),
    with **MAF** < (1%, <u>0.1%</u>)
    or with **concordance** > 98% among 11,260 duplicate pairs  

  --> # of SNPs after pre-processing *p* = ___

- An example of summary for # of SNPs 

| Chromosome | Total # of SNPs | # of SNPs after preprocessing | Proportion of valid SNPs |
| ---------- | --------------- | ----------------------------- | ---------- |
| 1          | 19865           | 13400                         | 67.46% |
| 2          | 22213 | 14866 | 66.92% |
| ... |  |  |  |
| 22 | 2519 | 1674 | 66.45% |
| X | 5705 | 799 | 14.01% |
| Undefined | 47 | 24 | 51.06% |
| Total | 262264 | 177008 | 67.49% |



### 3. Heterogeneity in population

- The difference from where the subjects are collected can lead to the heterogeneity of data such as MAF. Therefore, we need to figure out whether the given data is homogeneous or not. Toward this end, we'll see the Principal Component plot with two-dimensional space.



### 4. Imputation