---
title: ShuffleNet（Face++, 2017）
date: 2018-05-08 19:00:00
categories: fCompress
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

论文： Zhang X, Zhou X, Lin M, et al. ShuffleNet: An Extremely Efficient Convolutional Neural Network for Mobile Devices[J]. 2017.

### 论文算法概述

       提出一种轻量级网络suffernet，该网络框架主要使用了两种新的操作，逐点的组卷积(pointwise group convolution，1x1的组卷积)和通道打乱(channel shuffle)，极大减少运算量并维持准确率。
	   优化结构，也考虑速度。
	   
### Channel Shuffle for Group Convolutions

   一些优秀的网络如Xception和ResNeXt中，引入了深度可分离卷积(depthwise separable convolutions)和组卷积(group convolutions)，但注意到这些设计都没有充分考虑到1 x 1卷积(也叫pointwise convolutions)。例如在ResNeXt中，只有3x3的层里使用了组卷积。这样对于ResNeXt里的每个残差单元中，逐点卷积占93.4%的乘加。在小网络里，逐点一般用于限制网络层的通道数以限制网络复杂度，但也可能会导致准确率降低。

   为了处理这个问题，一个简单的方法是使用通道稀疏连接(channel sparse connections),例如组卷积也应用在1x1层上。通过确保每个卷积操作都只作用在对应的输入通道组上，组卷积可以明显减少计算量。然而，如果多个组卷积堆叠在一起，就会有一个问题，那就是每个特定通道的输出都只来源于一小部分的输入通道，在图1(a)中解释了两个堆叠在一起的组卷积的情况。这样会限制通道之间的信息流动并弱化特征表达能力。

   如果我们允许组卷积能从不同组中获取输入数据（如图1b），那么输入和输出就能充分相关联了。具体地说，对于从前面一组层生成的特征图，我们首先在每个组里面将通道分成多个小组，然后打乱送入到下一个组，可由一个channel shuffle操作实现（如图3c），假设一个卷积层有g组，输出g x n个通道，首先将输出通道维度reshape成(g,n)，转置，再flattening回去。通道打乱操作使其可以构建更强的多组卷积层。

<center><img src="{{ site.baseurl }}/images/pdCompress/shufflenet1.png"></center>

### ShuffleNet Unit
 
   以ResNet中提出的如图2(a)的瓶颈单元(bottleneck unit)开始设计，瓶颈单元是一个残差结构，在其残差分支上，对于3x3的层，我们采用计算量较少的3x3 depthwise卷积，然后将第一个1x1层用逐点组卷积进行替换，并接一个channel shuffle操作去组成ShuffleNet单元，如图2(b)所示。其中第二个逐点组卷积是用于恢复通道维度以匹配shortcut分支的维度。为了方便，在第二个逐点组卷积后，我们不再使用额外的通道打乱操作，因为它的结果变化并不大。而对于shufflenet带步长的情况，我们简单做两步调整(如图2c)，1）添加一个3x3均值池化到shortcut路径上；2）用通道串联代替逐点加法，这样能扩大通道维度而仅需要一点点额外计算量。

   归功于逐点组卷积和通道打乱，使ShuffleNet单元可以高效计算，在相同配置下计算复杂度比ResNet和ResNeXt小。例如，输入c x h x w，瓶颈通道数为m，n为卷积组的数量，则计算量对比如下：

* ResNet：hw(2cm + 9m^2)FLOPS.

* ResNeXt：hw(2cm + 9m^2/g)FLOPS.

* ShuffleNet：hw(2cm/g + 9m)FLOPS.

<center><img src="{{ site.baseurl }}/images/pdCompress/shufflenet2.png"></center>

### Network Architecture

   ShuffleNet整体结构如下:
   
<center><img src="{{ site.baseurl }}/images/pdCompress/shufflenet3.png"></center>

### Experiments

   其中0.5X表示卷积滤波器个数减半。
   
<center><img src="{{ site.baseurl }}/images/pdCompress/shufflenet4.png"></center>

<center><img src="{{ site.baseurl }}/images/pdCompress/shufflenet5.png"></center>

<center><img src="{{ site.baseurl }}/images/pdCompress/shufflenet6.png"></center>

<center><img src="{{ site.baseurl }}/images/pdCompress/shufflenet7.png"></center>

<center><img src="{{ site.baseurl }}/images/pdCompress/shufflenet8.png"></center>