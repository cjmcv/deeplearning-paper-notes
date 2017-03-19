---
title: 颜色空间
date: 2015-01-01 09:00:00
categories: fbImgb
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<!--<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>
<center><img src="{{ site.baseurl }}/images/pdBase/svm_smo1.png"></center>-->

1. RGB，红绿蓝三基色，用三维立方体表示；

2. HSV，色调（H），饱和度（S），明度（V），六角锥体模型；（Hue色度，Saturation饱和度，Value 明度）

3. HSI，色调（H），饱和度（S），亮度（I），双六角锥体模型；（Hue色度，Saturation饱和度，lightness亮度）

4. 与HSV相似，仅用亮度（lightness）替代了明度（brightness）。二者区别在于，一种纯色的明度等于白色的明度，而纯色的亮度等于中度灰的亮度。

5. Lab ，亮度（L），a红色到绿色的范围，b表示从黄色到蓝色的范围，颜色被设计来接近人类视觉；

6. YUV，一种颜色编码方法（属于PAL），亮度（Y），色调（Cr），色度（Cb）；

7. YIQ，一种颜色编码方法（属于NTSC），能将图像中的亮度分量分离提取出来的优点，并且YIQ颜色空间与RGB颜色空间之间是线性变换的关系；

