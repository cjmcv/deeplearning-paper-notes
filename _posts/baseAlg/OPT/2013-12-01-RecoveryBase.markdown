---
title: 图像复原基本概念
date: 2015-01-01 11:01:00
categories: fbOpt
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

### 图像算法分类

<center><img src="{{ site.baseurl }}/images/pdBase/recovery_a1.png"></center>

       
### 恢复问题的病态性与奇异性

<center><img src="{{ site.baseurl }}/images/pdBase/recovery_a2.png"></center>
---

### 图像增强和图像复原的区别

相同点：图像增强与图像恢复都是改善给定图像的质量。

不同点：

1. 图像恢复是利用退化过程的先验知识，来建立图像的退化模型，再采用与退化相反的过程来恢复图像，而图像增强一般无需对图像降质过程建立模型。

2. 图像恢复是针对图像整体，以改善图像的整体质量。而图像增强是针对图像的局部，以改善图像的局部特性，如图像的平滑和锐化。

3. 图像恢复主要是利用图像退化过程来恢复图像的本来面目，它是一个客观过程，最终的结果必须有一个客观的评价准则；而图像增强主要是用各种技术来改善图像的视觉效果，以适应人的心理、生理需要，而不考虑处理后的图像是否与原图像相符，也就很少涉及统一的客观评价准则。 

