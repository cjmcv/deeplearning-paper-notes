---
title: 离散余弦变换DCT
date: 2015-01-01 09:00:00
categories: fbImgT
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<!--<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>
<center><img src="{{ site.baseurl }}/images/pdBase/svm_smo1.png"></center>-->

### 概述

   DCT（Discrete Cosine Transform），一种实数域变换，其变换核为实数余弦函数，是有损图像算法JPEG的核心。是简化傅立叶变换的一种方法（DFT变换是频谱分析的有利工具，但DFT变换是基于复数域的运算，因而给实际运算带来了不便）。
	
   DCT变换有许多特点：DCT为实数正交变换，变换矩阵确定（与变换对象无关），具有多种快速算法（其中一种是建立在FFT上 p124），二维DCT是一种可分离的变换（如FFT，先对行后对列做一维变换完成二维变换），余弦变换的能量项低频集中等。

