---
title: 常用的阈值化算法
date: 2015-01-01 09:00:00
categories: fbImgb
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<!--<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>
<center><img src="{{ site.baseurl }}/images/pdBase/svm_smo1.png"></center>-->

### 双峰法

   在一些简单的图像中，物体的灰度分布比较有规律，背景与各个目标在图像的直方图各自形成一个波峰，即区域与波峰一一对应，每两个波峰之间形成一个波谷。那么，选择双峰之间的波谷所代表的灰度值T作为阈值，即可实现两个区域的分割。

### P参数法

   当不同区域（即目标）之间的灰度分布有一定的重叠时，双峰法的效果就很差。如果预先知道每个目标占整个图像的比例P，则可以采用P参数法进行分割。假设已知整个直方图中目标区域所占的比例为P：1)  计算图像的直方图分布<img src="http://latex.codecogs.com/gif.latex? p(i)"/>其中t=0,1,2,…,255，表示图像的灰度值； 2)  从最低的灰度值开始，计算图像的累计分布直方图<img src="http://latex.codecogs.com/gif.latex? p_1 (t) = \sum\limits_{i = 0}^t {p(i)} ,\;\;\;t = 1,2,...,255"/> ；3)  计算阈值T,有<img src="http://latex.codecogs.com/gif.latex? T = \arg \min \left| {p_1 (t) - P} \right|"/>，也就是说，阈值就是与P1最为接近的累积分布函数所对应的灰度值t。（仅适用于实现已知目标所占全图像百分比的场合）。

### 最大类间方差法阈值分割法（Otsu）

   一种自适应的阈值确定的方法，按图像的灰度特性,将图像分成背景和目标两部分。类间方差法对噪音和目标大小十分敏感，它仅对类间方差为单峰的图像产生较好的分割效果。
最大类间方差法公式推导：记T为目标与背景的分割阈值，目标的像素点个数占图像的像素点个数的比例记为<img src="http://latex.codecogs.com/gif.latex? \omega _{\rm{0}} "/>，平均灰度记为<img src="http://latex.codecogs.com/gif.latex? \mu _0 "/>，背景像素点占图像像素点的比例记为<img src="http://latex.codecogs.com/gif.latex? \omega _1 "/>，平均灰度记为<img src="http://latex.codecogs.com/gif.latex? \mu _1 "/>，图像的总平均灰度记为<img src="http://latex.codecogs.com/gif.latex? \mu"/>，类间方差值记为g。则图像的总平均灰度值为：<img src="http://latex.codecogs.com/gif.latex? \mu  = \omega _0 {\rm{*}}\mu _{\rm{0}}  + \omega _1 {\rm{*}}\mu _{\rm{1}} "/>；     前景和背景的方差值为： <img src="{{ site.baseurl }}/images/pdBase/imgb_thresh1.png">；

   通过前面所述，当方差g最大时，认为此时目标和背景相差最大，此时的T就是最佳阈值。

   编程思想：可以用遍历的方法找到这个阈值：给图像建立灰度直方图，范围从[0,254]。从灰度级为0开始，0为背景，1-254为前景，计算方差；0、1为背景，2-254为前景，计算。遍历全部，求最大方差。

   编程步骤：算灰度直方图（横轴为0-255，纵轴为个数每次加1），归一化，得到累计直方图A得到的每个bin就是从0到当前灰度值的像素数量所占的概率 ；由灰度直方图直接建立累计直方图B，其纵轴为灰度从0到当前灰度的总灰度值<img src="http://latex.codecogs.com/gif.latex? g_0"/>，除以对应的总数得到<img src="http://latex.codecogs.com/gif.latex? \mu _0"/>。而<img src="http://latex.codecogs.com/gif.latex? \mu_1"/>则为在累计直方图B中255对应的数减去<img src="http://latex.codecogs.com/gif.latex? g_0"/>再除以个数。其中个数可由累计直方图A中的<img src="http://latex.codecogs.com/gif.latex? \omega _{\rm{0}} "/>乘上总像素得到。

### 最大熵阈值分割

<center><img src="{{ site.baseurl }}/images/pdBase/imgb_thresh2.png"></center>

### 迭代法（最佳阈值法）

<center><img src="{{ site.baseurl }}/images/pdBase/imgb_thresh3.png"></center>
