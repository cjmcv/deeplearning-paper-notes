---
title: ScaleFace（CVPR, 2017）
date: 2017-10-16 23:00:00
categories: fDetect
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

论文： Yang S, Xiong Y, Loy C C, et al. Face Detection through Scale-Friendly Deep Convolutional Networks[J]. 2017.

### 论文算法概述

       Scaleface主要针对大尺度范围的人脸检测问题，将一个大的尺度范围分成多个小范围，各个小尺度范围的人脸通过特定的网络结构进行各自建模，然后合并到一个网络的进行端到端训练。且该模型对于大范围尺度的人脸检测不需要以图像金字塔作为输入。
	   
<center><img src="{{ site.baseurl }}/images/pdDetect/scaleface1.png"></center>

### Finding a network for specific scale range

   网络层级应与图像尺度相对应才能取得最佳效果，如图所示，小的人脸在stride=8的Res2+Res3层的特征中能取得最优效果，而大人脸则在stride=32的Res4+Res5层中取得最优。直观地看，更高层特征图上的物体ROI就会越小。
   
<center><img src="{{ site.baseurl }}/images/pdDetect/scaleface2.png"></center>
  
<center><img src="{{ site.baseurl }}/images/pdDetect/scaleface3.png"></center>

### How many scale variant detectors

   尺度范围的划分情况实验
   
<center><img src="{{ site.baseurl }}/images/pdDetect/scaleface4.png"></center>

### How to combine the scalevariant detectors

  图中navie对应的是使用三个独立训练的检测器联合在一起检测的结果，这三个检测器拥有文中各检测器一样的网络结构而没有共享特征表达；而Joint对应的是文中方式的结果，三个检测器并入一个网络中，前级网络的特征表达共享。
 
<center><img src="{{ site.baseurl }}/images/pdDetect/scaleface5.png"></center>

### Training and implementation details
   
  训练集按尺度范围划分，包含正负样本，分别输入对应的检测器，各个检测器的训练集互斥。不同的尺度范围使用不同的ROIs和ground-true，而ground-truth regions尺度超过对应范围时，该检测器将其忽略。
   
<center><img src="{{ site.baseurl }}/images/pdDetect/scaleface6.png"></center>
   
### Results on benchmark datasets
   
  在FDDB中，200 false positives时达到94.55%召回率，而2000 false positives时为96.00%。
   
<center><img src="{{ site.baseurl }}/images/pdDetect/scaleface7.png"></center>

### 总结

   针对大尺度范围的人脸检测问题，将一个大的尺度范围分成多个小范围，各个小尺度范围的人脸通过特定的网络结构进行各自建模，然后合并到一个网络的进行端到端训练。以类似特征金字塔的方式取不同层对不同的尺度做处理，如主干网络从底往上有从res2x到res5x共4个block，则res2x和res3x的特征输出联合处理小人脸，res3x和res4x的特征输出联合处理中人脸，同理res4x和res5x处理大人脸。每个尺度范围都有一个对应的检测分支，由Fast RCNN和RPN组成。最后会将所有检测分支的输出汇总为结果。

   注意训练样本也要做尺度划分，且划分后每个集的训练样本互斥，每个分支只处理自己对应的范围而忽略其他的。
   