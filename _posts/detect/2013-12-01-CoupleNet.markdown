---
title: CoupleNet（ICCV, 2017）
date: 2018-03-22 19:00:00
categories: fDetect
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

论文： Zhu Y, Zhao C, Wang J, et al. CoupleNet: Coupling Global Structure with Local Parts for Object Detection[C]// IEEE International Conference on Computer Vision. IEEE, 2017:4146-4154.

Github：[https://github.com/tshizys/CoupleNet](https://github.com/tshizys/CoupleNet)

### 论文算法概述

       主要基于R-FCN进行改进，提到R-FCN使用了对位置信息敏感的score maps，导致忽略了全局的结构信息。而CoupleNet主要就是处理这个问题，结合局部和全局上下文信息进行目标检测。其中，由RPN输出的候选框会送入到couple模块中，包含了两个分支。一个分支采用对位置信息敏感的ROI池化层（PSROI）去获取局部信息，而另一个分支则使用ROI池化去去对全局和上下文信息进行编码。实验结果；VOC07为82.7%mAP，VOC12为80.4% ，MS COCO为34.4%。
	   
<center><img src="{{ site.baseurl }}/images/pdDetect/couplenet1.png"></center>
	   
### Network architecture

   网络结构如下图2所示，只要分为两个分支，一个对应局部信息，记为local FCN，另一个对应全局和上下文信息，记为global FCN。
   
<center><img src="{{ site.baseurl }}/images/pdDetect/couplenet2.png"></center>

### Local FCN

   通过拼接带k x k x (C+1)个通道数的1x1卷积层来构建一系列的对部件敏感的score maps，其中k表示将目标分成k x k个局部部件(文中实验设为7)，而C+1对应着目标的类别数加上背景一类。对应每个类别都会有k x k个通道数，每个通道负责对目标的一个特定部件进行编码，最后的类别评分由这k x k个通道的输出响应进行投票得到。这里使用对位置信息敏感的ROI池化层[详细介绍看回R-FCN论文]来提取目标的特定部件的特征，并简单使用均值池化进行投票。然后我们可以获得一个C+1维的特征向量，表示这该目标属于每个类别的概率。这个过程等同于将一个强分类器变成由多个分别对应一个部件的弱分类器的组成，这里将这个组合看成是局部的结构表征local structure representation。如下图3(a)所示，对于不完整的人图像，网络可能难以从全局描述上得到较强的响应，因为人像并不完整。相反，这里提出的Local FCN可以有效地获取到几个特定部件的特征，如鼻子嘴巴等在特征图特定区域能够得到较强的响应。但对于那些具有简单的空间结构和包含相当大背景的在候选框内的目标，如图中的饭桌，单一的Local FCN难以满足要求，所以再加了一个针对全局结构信息的Global FCN。
  
<center><img src="{{ site.baseurl }}/images/pdDetect/couplenet3.png"></center>
  
### Global FCN

   Global FCN针对全局信息，首先在ResNet-101最后的卷积层后添加一个1024维的1x1卷积层，用于降维。因为目标的大小不一，所以作者插入了一个ROI池化层去提取固定长度的特征向量作为目标的全局结构描述。然后使用两个卷积核分别为kxk和1x1的卷积层去近一个提取ROI的全局表征。为了提取上下文信息，将context region扩展为原来proposal区域的两倍大小，从原始区域进行ROI池化的输出结果和从context region进行ROI池化的结果进行拼接，如上面图2所示。因为使用的是ROI池化运算，所以Global FCN提取的是目标的整体特征，与Local FCN互补。

### Coupling structure

   为了均衡Local FCN和Global FCN的影响比重，在两个网络的输出进行拼接之前采用了一个归一化操作。这里作者探索了两种不同的方法去做这个归一化，使用L2归一化或使用一个1x1卷积去模拟缩放尺度。同时也研究了三种方式去联合两个网络的输出，分别是element-wise sum、element-wise product和element-wise maximum。作者实验结果显示采用1x1 convolution和element-wise sum的效果最佳。

<center><img src="{{ site.baseurl }}/images/pdDetect/couplenet4.png"></center>

### Experiments

<center><img src="{{ site.baseurl }}/images/pdDetect/couplenet5.png"></center>

<center><img src="{{ site.baseurl }}/images/pdDetect/couplenet6.png"></center>

<center><img src="{{ site.baseurl }}/images/pdDetect/couplenet7.png"></center>

<center><img src="{{ site.baseurl }}/images/pdDetect/couplenet8.png"></center>