---
title: 插值算法
date: 2015-01-01 09:00:00
categories: fbImgb
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<!--<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>
<center><img src="{{ site.baseurl }}/images/pdBase/svm_smo1.png"></center>-->

### 最近邻插值

   也叫零阶插值法，选择离它所映射到的位置最近的输入像素的灰度值为插值结果。对二维图像，是取待测样点周围4 个相邻像素点中距离最近1 个相邻点的灰度值作为待测样点的像素值。

### 线性插值

   假设我们已知坐标 (x0, y0) 与 (x1, y1)，要得到 [x0, x1] 区间内某一位置 x 在直线上的值。

### 双线性插值

   又叫一阶插值法，要经过三次插值才能获得最终结果，是对最近邻插值法的一种改进，先对两水平方向进行一次线性插值，然后再在垂直方向上进行一次线性插值。

### 双三次插值

   又叫立方卷积插值，是对双线性插值的改进，它能创造出比双线性插值更平滑的图像边缘，函数 f 在点 (x, y) 的值可以通过矩形网格中最近的十六个采样点的加权平均得到，在这里需要使用两个多项式插值三次函数，每个方向使用一个。

<center><img src="{{ site.baseurl }}/images/pdBase/imgb_inter1.png"></center>
