---
title: SqueezeNet（2016）
date: 2017-08-12 19:00:00
categories: fCompress
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

论文： Iandola F N, Han S, Moskewicz M W, et al. SqueezeNet: AlexNet-level accuracy with 50x fewer parameters and <0.5MB model size[J]. 2016.

Github：[https://github.com/DeepScale/SqueezeNet](https://github.com/DeepScale/SqueezeNet)

推荐博客：[http://www.jianshu.com/p/8e269451795d](http://www.jianshu.com/p/8e269451795d)

### 论文算法概述

       提出SqueezeNet，比alexnet的参数少50倍，但在imagenet上具有与alexnet相近的准确率。而论文的主要目标是找到能保持准确率的参数更少的CNN结构。
	   
### Architectural Design Strategies

1. 将部分3x3滤波器改成1x1。参数少9倍。
   
2. 减少3x3滤波器的输入通道数。假设一个卷积层只由3x3卷积组成，则参数数量为（输入维度）x（滤波器数量）x（3x3），这里用下面介绍的squeeze layers来减少输入通道数。

3. 将下采样操作推后，使卷积层能有更大的激活特征图，提高的准确率。

### The Fire Module

   一个Fire module由一个squeeze卷积层（只包含1x1卷积）输入到一个expand层（混合了1x1和3x3卷积），如图1所示。采用1x1卷积是源于策略1，而squeeze卷积层中卷积核数比expand层的少，减少了3x3滤波器的输入通道数，即策略2。
   
<center><img src="{{ site.baseurl }}/images/pdCompress/squeezenet1.png"></center>

### The SqueezeNet architecture

   SqueezeNet网络结构如图2左所示，一头一尾为普通的卷积层，其余主要由fire模块组成，最后使用均值池化代替全连接层（1、conv提取局部信息，fc提取全局进行，而最大的局部即为全局，则可用conv代替fc；2、fc权重与输入大小有关，使输入大小固定，而conv则没这个要求，对于不同的输入大小，卷积可在大尺寸图上滑动进行特征提取）。图2另外两个网络则参考resnet添加了旁路，测试结果如表3所示。其中作者还重点提到了使用Deep Compression对squeezeNet进一步压缩，可降低510倍模型大小且保持准确率，但需要注意的是这类型的压缩只针对模型的大小。而对于不同结构，其参数与计算量并没有必然关系。

<center><img src="{{ site.baseurl }}/images/pdCompress/squeezenet2.png"></center>

<center><img src="{{ site.baseurl }}/images/pdCompress/squeezenet3.png"></center>

   
