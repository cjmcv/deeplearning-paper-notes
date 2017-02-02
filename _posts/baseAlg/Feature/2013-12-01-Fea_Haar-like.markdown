---
title: Haar-like特征
date: 2015-01-01 11:00:00
categories: fbFeature
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<!--<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>
<center><img src="{{ site.baseurl }}/images/pdBase/svm_smo1.png"></center>-->

### 概述

   Haar-like特征是一种简单矩形特征，因类似于Haar小波而得名。特征模板内有白色和黑色两种矩形，并定义该模板的特征值为白色矩形像素和减去黑色矩形像素和（白减黑）。Haar特征值反映了图像的灰度变化情况。通过改变特征模板的大小和位置（如：先用其中一类模板，大小设为2x2，平移遍历全图得到特征集，换类、遍历、换类、遍历、换...再改成用4x4遍历，8x8...），可在图像子窗口中穷举出大量的特征。

   使用积分图来提高图像特征值的计算效率。积分图定义为<img src="http://latex.codecogs.com/gif.latex? ii(x,y) = \sum\limits_{x' \in x} {\sum\limits_{y' \in y} {I(x',y')} } "/>，ii为积分图累计块，I为图像。

   举例：如图所示，计算时ii1=Sum（A），同理，点2、点3、点4的积分图像分别为 ii2=Sum(A)+Sum(B);     ii3=Sum(A)+Sum(C);    ii4=Sum(A)+Sum(B)+Sum(C)+Sum(D)；则矩形区域D内的所有像素灰度积分可由矩形端点的积分图像值得到Sum(D)=ii1+ii4-(ii2+ii3) ；而矩形特征的特征值是两个不同的矩形区域像素和之差，如图2.4 所示，该特征原型的特征值定义为:Sum（A）-Sum（B）；Sum(A)=ii4+ii1-(ii2+ii3);    Sum(B)=ii6+ii3-(ii4+ii5); 所以此类特征原型的特征值为:(ii4-ii3)-(ii2-ii1)+(ii4-ii3)-(ii6-ii5)

   重点注意：一个类型的矩形在一个位置代表一个特征，而特征值即白减黑。一个特征可作为一个弱分类器，但未必是有效的分类器，有效即正确率达1/2以上。一个haar弱分类器包含有：特征值大小、阈值和指示不等号方向。如一c矩形矩阵，足够大，横跨整个眼睛部位，即眼黑对着中间矩形黑色部分，眼黑与眼白等有明显特征，而白减黑的值将很大，设定好一个阈值，使该分类器大于该值则为正，否则分为负类，这将会是一个较好的弱分类器，用该弱分类器（位置、大小、方向等相同）在其他样本中所得到的特征值也较相似。又如一矩形a，比较小，在额头部分，平坦，相减接近于0，无明显特征，其特征值无多大意义，所以该弱分类器不是一个有效的分类器。而adaboost就是将这些弱分类器进行筛选后，将有效的弱分类器组合起来成为一个强分类器。 继而用级联将多个强分类器组合用于识别人脸。
	
   弱分类器阈值的确定：对于每个特征 f（弱分类器），计算所有训练样本的特征值，并将其排序。通过扫描排好序的特征值，可以为该特征确定一个最优的阈值，从而训练成一个弱分类器。具体来说，对排好序的表中的每个元素，计算下面四个值：

1. 全部人脸样本的权重的和<img src="http://latex.codecogs.com/gif.latex? T^ + "/>；

2. 全部非人脸样本的权重的和<img src="http://latex.codecogs.com/gif.latex? T^ - "/>；

3. 排序在该元素之前的人脸样本的权重的和<img src="http://latex.codecogs.com/gif.latex? S^ + "/>；

4. 排序在该元素之前的非人脸样本的权重的和<img src="http://latex.codecogs.com/gif.latex? S^ - "/>。

--

   这样，当选取当前任意元素的特征值作为阈值时，所得到的弱分类器就在当前元素处把样本分开———也就是说这个阈值对应的弱分类器将当前元素前的所有元素分类为人脸（或非人脸），而把当前元素后（含）的所有元素分类为非人脸（或人脸）。可以认为这个阈值所带来的分类误差为:<img src="http://latex.codecogs.com/gif.latex? \varepsilon  = \min (S^ +   + (T^ -   - S^ -  ),S^ -   + (T^ +   - S^ +  ))"/>。于是，通过把这个排序的表从头到尾扫描一遍就可以为弱分类器选择使分类误差最小的阈值（最优阈值），也就是选取了一个最佳弱分类器。同时，选择最小权重错误率的过程中也决定了弱分类器的不等式方向。

<center><img src="{{ site.baseurl }}/images/pdBase/fea_haar1.png"></center>
