---
title: Forstner角点检测
date: 2015-01-01 11:00:00
categories: fbFeature
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<!--<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>
<center><img src="{{ site.baseurl }}/images/pdBase/svm_smo1.png"></center>-->

### 概述

   核心思想：首先通过计算图像各个像元的Robert’s梯度和以该像素为中心的一个窗口的灰度协方差矩阵，选为特征候选点的都是误差最接近圆的椭圆点。

   步骤:

<center><img src="{{ site.baseurl }}/images/pdBase/fea_forstner1.png"></center>

   特点：具有较快的计算速度和较高的精度，速度比Moravec较慢，但精度较高。
