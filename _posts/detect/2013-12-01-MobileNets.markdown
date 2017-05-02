---
title: MobileNets（Google, 2017）
date: 2017-04-28 19:00:00
categories: fDetect
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

论文： MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications

github: [https://github.com/pby5/MobileNet_Caffe](https://github.com/pby5/MobileNet_Caffe)

[https://github.com/Zehaos/MobileNet](https://github.com/Zehaos/MobileNet)

### 论文算法概述

       MobileNets基于流线型结构，使用depthwise separable convolution（出自《Xception: Deep learning with depthwise separable convolutions》）构建而成的深度神经网络，并引入两个简单的全局超参数去平衡速度和精度。构建小的高效的模型是常被关注的问题，常见的处理方法可分为两类，压缩预训练模型或直接训练小模型，很多论文中主要关注模型大小，参数变少了，但前向时间却变长了。而MobileNets主要关注速度问题，同时兼顾模型大小。
	   
### Depthwise Separable Convolution

   MobileNet模型基于depthwise separable convolutions，这种卷积可以分解为一个depthwise convolution和1x1的pointwise convolution。depthwise convolution对每个输入通道采用单一滤波器，pointwise convolution则使用1x1卷积去线性融合depthwise convolution的输出，共两步操作。而标准的卷积中，滤波和信息融合都会在一步中完成。这样分解能够较大程度上减少网络计算量和模型大小。如图2中表示由标准卷积2(a)分解成一个depthwise convolution 2(b)和一个1x1 pointwise convolution 2(c).
   
<center><img src="{{ site.baseurl }}/images/pdDetect/mobilenet1.png"></center>

   一个标准的卷积层以Df x Df x M的特征图F输入，得到Df x Df x N的特征图G输出，Df表示特征图空间上的高度和宽度，M/N为通道数。设Dk为卷积核大小。则参数数量为Dk x Dk x M x N，计算量可以表示为Dk x Dk x M x Df x Df x N。MobileNets模型中，通过使用depthwise separable convolution来将输出通道数N和卷积核大小Dk隔离开。MobileNets中depthwise convolution采用的是单一的滤波器，计算量为Dk x Dk x M x Df x Df，pointwise convolution用于线性的信息融合，其计算量为1x 1x M x N x Df x Df，所以depthwise separable convolution的整体计算量为Dk x Dk x M x Df x Df + M x N x Df x Df。MobileNets中使用3x3的depthwise separable convolution可以减少8~9倍的运算量，而准确率下降不多。
   
### Depthwise Separable Convolution

   MobileNet除了第一层卷积为普通卷积外，其他卷积均由depthwise separable convolution堆叠而成，网络结构如table1所示，除了softmax分类层外，所有层都接着batchnorm和ReLU。其中最后一层卷积后添加一个均值池化将特征图大小变为1x1后输入全连接层。若depthwise separable convolution按两层算，则MobileNet有28个网络层。
   
<center><img src="{{ site.baseurl }}/images/pdDetect/mobilenet2.png"></center>

### Width Multiplier: Thinner Models

   为使模型更轻量化，引入宽度因子alpha，作用在输入和输出的通道数上，则一层depthwise separable convolution的计算量为Dk x Dk x (alpha x M) x Df x Df + (alpha x M) x (alpha x N) x Df x Df，alpha的取值范围是0到1。
   
### Resolution Multiplier: Reduced Representation

   为使模型更轻量化，引入分辨率因子p，作用在输入大小Df上，则一层depthwise separable convolution的计算量为Dk x Dk x (alpha x M) x (p x Df) x (p x Df) + (alpha x M) x (alpha x N) x (p x Df) x (p x Df)，p的取值范围是0到1。
   
### Experiments

<center><img src="{{ site.baseurl }}/images/pdDetect/mobilenet3.png"></center>

<center><img src="{{ site.baseurl }}/images/pdDetect/mobilenet4.png"></center>
