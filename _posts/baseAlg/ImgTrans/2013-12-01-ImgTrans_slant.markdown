---
title: 斜变换Slant
date: 2015-01-01 10:00:00
categories: fbImgT
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<!--<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>
<center><img src="{{ site.baseurl }}/images/pdBase/svm_smo1.png"></center>-->

### 概述

   基本思想在于：根据图像信号的相关性，某行的亮度具有基本不变或线性渐变的特点，可以编造一个变换矩阵，来反映这种递增或递减（线性渐变）特性的行向量。

   斜变换是由WHT中矩形状态变化变为倾斜状态变化所得的结果，因此也称为倾斜哈达玛变换。适于灰度逐渐改变的图像信号，已成功用于图像编码。

---

   性质:

1. 斜变换为实的正交变换<img src="http://latex.codecogs.com/gif.latex? S = S^*"/>，<img src="http://latex.codecogs.com/gif.latex? S^{ - 1}  = S^T"/>； 

2. 斜变换具有快速计算特性，对N*1向量只需要<img src="http://latex.codecogs.com/gif.latex? {\rm{O(Nlog}}_{\rm{2}} {\rm{N)}}"/>次操作； 

3. 斜变换具有更好的能量压缩特性。

