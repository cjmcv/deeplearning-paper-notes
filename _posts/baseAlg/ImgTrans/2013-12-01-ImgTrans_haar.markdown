---
title: 哈尔变换
date: 2015-01-01 10:00:00
categories: fbImgT
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<!--<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>
<center><img src="{{ site.baseurl }}/images/pdBase/svm_smo1.png"></center>-->

### 概述

   最普遍最简单的正交小波变换，哈尔函数由Haar提出的一种正交完备函数系；是一种既反映整体又反映局部的函数；具有完备性与归一化正交性；它是小波变换中的典型小波；其矩阵只有+1、-1和另一个以<img src="http://latex.codecogs.com/gif.latex? "/>为基础的系数，它是正交稀疏矩阵，可实现快速运算。（除了哈尔小波，其他小波似乎都不正交）
	
   定义：哈尔变换本身对称、可分离， F是图像矩阵，H是变换矩阵，T为结果二维矩阵。由于是对的每一行作哈尔小波转换，作完后还要对的每一列作哈尔小波转换（与二维傅里叶变换一样），因此公式为：<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>; 其逆变换为<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>。哈尔基函数：<img src="{{ site.baseurl }}/images/pdBase/ImgTrans_haar1.png">.

---

特性：

1. 收敛均匀而迅速；

2. 傅立叶变换的基函数只是频率不同，哈尔函数在尺度和位置上都不同；

3. 哈尔变换具有尺度和位置的双重性；

4. 全域特性和区域特性：哈尔函数系列可分成全域部分和区域部分，全域部分作用于整个变换区间，区域部分作用于局部区域；

5. 小波特性：变换后子波的尺度远小于原始信号；

6. 不需要乘法（只有相加或加减）；

7. 大部分运算为0，不用计算；

8. 输入与输出个数相同.

---

<center><img src="{{ site.baseurl }}/images/pdBase/ImgTrans_haar2.png"></center>

<center><img src="{{ site.baseurl }}/images/pdBase/ImgTrans_haar3.png"></center>







