---
title: 离散沃尔什变换Walsh
date: 2015-01-01 10:00:00
categories: fbImgT
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<!--<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>
<center><img src="{{ site.baseurl }}/images/pdBase/svm_smo1.png"></center>-->

### 概述

   DFT或DCT变换都是有正弦或余弦三角函数为基本的正交函数基，而DFT的快速运算是在复数范围内进行运算，不仅运算量庞大，而且运算复杂，DCT变换虽然避免了复数运算，但需要进行三角函数运算，运算复杂程度亦然很高。因此DFT和DCT运算占用时间仍然较多。
	
   沃尔什变换就是一种更为有效和便利的变换方法。由只有+1和-1两个数值所构成的完备二值正交基组成。

---

从排序熵可将沃尔什函数分成三种定义方法：

1. 按照佩利排序来定义（按自然排序）；

2. 按沃尔什排序来定义（按列率排序）；

3. 按照哈达玛排序来定义，又称为哈达玛变换。
