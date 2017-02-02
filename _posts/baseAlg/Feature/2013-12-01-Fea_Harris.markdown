---
title: Harris角点检测
date: 2015-01-01 11:00:00
categories: fbFeature
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<!--<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>
<center><img src="{{ site.baseurl }}/images/pdBase/svm_smo1.png"></center>-->

### 概述

   当图像窗口平移产生灰度变化：<img src="http://latex.codecogs.com/gif.latex? E(u,v) = \sum\limits_{x,y} {w(x,y)\left[ {I(x + u,y + v) - I(x,y)} \right]^2 } "/>；式中w(x,y)是窗函数在位置(x,y)处的系数，它可以是常数也可以是高斯函数。根据泰勒展开，对图像I(x,y)在平移(u,v)后进行一阶近似，可以得到：<img src="http://latex.codecogs.com/gif.latex? I(x + u,y + v) = I(x,y) + I_x u + I_y v + O(u^2 ,v^2 )"/>；所以<img src="http://latex.codecogs.com/gif.latex? E(u,v) = \sum\limits_{x,y} {w(x,y)\left[ {I_x u + I_y v + O(u^2 ,v^2 )} \right]^2 } "/> 。。（注：<img src="http://latex.codecogs.com/gif.latex? O(u^2 ,v^2 )"/>不用考虑，<img src="http://latex.codecogs.com/gif.latex? I_x  = {\partial I}/{\partial x}"/> ）。 

   因为<img src="http://latex.codecogs.com/gif.latex? \left[ {I_x u + I_y v} \right]^2  = [u,v]\left[ {\begin{array}{*{20}c}
   {I_x^2 } & {I_x I_y }  \\
   {I_x I_y } & {I_y^2 }  \\
\end{array}} \right]\left[ {\begin{array}{*{20}c}
   u  \\
   v  \\
\end{array}} \right]
"/>，于是对于局部微小的移动量 [u,v]，可以近似得到下面的表达： <img src="http://latex.codecogs.com/gif.latex? E(u,v) \cong \left[ {u,} \right.\left. v \right]\begin{array}{*{20}c}
   {}  \\
\end{array}M\begin{array}{*{20}c}
   {}  \\
\end{array}\left[ {\begin{array}{*{20}c}
   u  \\
   v  \\
\end{array}} \right]
"/>.

   所以，图像中点x上的对称半正定矩阵为<img src="http://latex.codecogs.com/gif.latex? M_1  = \nabla I\nabla I^T  = \left[ {\begin{array}{*{20}c}
   {I_x }  \\
   {I_y }  \\
\end{array}} \right]\left[ {\begin{array}{*{20}c}
   {I_x } & {I_y }  \\
\end{array}} \right]
"/>，用权重矩阵W卷积，可得：<img src="http://latex.codecogs.com/gif.latex? M = W*M_1  = \sum\limits_{x,y} {w(x,y)\left[ {\begin{array}{*{20}c}
   {I_x^2 } & {I_x I_y }  \\
   {I_x I_y } & {I_y^2 }  \\
\end{array}} \right]}  = \left[ {\begin{array}{*{20}c}
   A & C  \\
   C & B  \\
\end{array}} \right]
"/>，M为Harris矩阵，关键是M的特征值。

   其中用w(x,y)卷积的目的是得到M1在周围像素上的局部平均，特征值会依赖于局部图像特性而变化。如果图像的梯度在该区域变化，那么M的第二个特征值将不再为0。如果图像的梯度变化，M的特征值也不会变化。

---

   二次项函数在本质上来说，是一个椭圆函数。椭圆的扁率和尺寸是由M的特征值<img src="http://latex.codecogs.com/gif.latex? \lambda _1 ,\lambda _2 "/>决定，椭圆的方向是由M的特征矢量决定的。 其与图像的关系有3种情况:

1. 当图像上是直线的时候，则一个特征值大而一个特征值小;

2. 当图像上是平面的时候，则两个特征值都很小，且近似相等;

3. 当图像上是角点的时候，则两个特征值都很大，且近似相等。

---

   为了避免计算矩阵M的特征值，定义角点相应函数<img src="http://latex.codecogs.com/gif.latex? R = \det M - k\left( {trace M} \right)^2"/>，  <img src="http://latex.codecogs.com/gif.latex? \det M = \lambda _1 \lambda _2  = AC - B^2 "/>，  <img src="http://latex.codecogs.com/gif.latex? {\mathop{\rm trace}\nolimits} M = \lambda _1  + \lambda _2  = A + C"/> 。IF  R>threshold THEN “yes”。。 

   其中，det M是矩阵M的行列式; trace M是矩阵M的迹，k是经验常数，一般在0.04~0.06的范围内选取。为了去除加权常数k，或可以使<img src="http://latex.codecogs.com/gif.latex? R = {\det M} / {(trace M)^2 }"/>；
最直观的说法：角点：在水平、竖直两个方向上变化均较大的点，即Ix、Iy都较大；   边缘：仅在水平、或者仅在竖直方向有较大的变化量，即Ix和Iy只有其一较大； 
平坦地区：在水平、竖直方向的变化量均较小，即Ix、Iy都较小；

---

最直观的说法：

* 角点：在水平、竖直两个方向上变化均较大的点，即Ix、Iy都较大；

* 边缘：仅在水平、或者仅在竖直方向有较大的变化量，即Ix和Iy只有其一较大； 

* 平坦地区：在水平、竖直方向的变化量均较小，即Ix、Iy都较小；

---

性质：1）旋转不变性；   2）对于图像灰度的仿射变化具有部分的不变性 ；   3）对于图像几何尺度变化不具有不变性，随几何尺度变化，Harris角点检测的性能下降；

---

在图像间找到对应点：可使用归一化的相关矩阵，通过减去均值和除以标准差，使图像亮度变化具有稳健性。需详细查阅“什么是相关矩阵”。

粗略概括步骤：

1. 用一个窗口对图像进行扫描，记录灰度变化（窗口带系数，求局部平均，因为特征值会依赖局部图像特性而变化）；

2. 对这个灰度变化公式使用泰勒展开进行一阶近似（为了得到点I(x,y)移动后的灰度）；

3. 将近似公式转换成矩阵形式，并将平移变量u、v分离出来得到Harris矩阵（就是假设u、v非常小，只留下梯度信息），求出其特征值；

4. 根据特征值判断是否为角点（都大为角，一大一小为线，都小为面）。


