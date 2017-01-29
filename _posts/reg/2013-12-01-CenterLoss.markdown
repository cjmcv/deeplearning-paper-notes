---
title: Center Loss（ECCV，2016）
date: 2016-10-29 19:30:00
categories: fReg
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

论文： Wen Y, Zhang K, Li Z, et al. A Discriminative Feature Learning Approach for Deep Face Recognition[M]// Computer Vision – ECCV 2016. Springer International Publishing, 2016.

Github: [https://github.com/ydwen/caffe-face](https://github.com/ydwen/caffe-face)

### 论文算法概述

       在训练深度网络时选择一个好的损失函数很重要，因为SGD用于CNN训练是基于mini-batch的，并不能很好的反映出深度特征的全局分布。而将所有图片一起输入训练不切实际，因此contrastive loss和triplet loss作为可选的方法，分别为两张图像和三张图片构造损失函数，并且相对与单张图片的效果得到显著提升，但不可避免地使收敛速度慢且不稳定。

       在大多数可用的CNN中，softmax损失函数被用作训练深度模型的监视信号。为了增强深度学习特征的识别能力，文中提出中心损失函数（center loss）。中心损失函数学习每类数据的深层特征的中心，惩罚深层特征和它们对应的类中心的距离，即训练时在更新特征中心的同时最小化深度特征与其对应中心的距离（减小类内距离）。这种中心损失函数是可训练的，在CNN中非常容易优化。在用于训练CNN时，使用softmax loss和center loss进行联合监督，使用超参数对两个监督信号进行平衡。从直观上看，softmax loss使各类的深度特征分离，而center loss将深度特征推向各自的中心。这样联合监督可以使类间差异加大，类内差异减小。实验中通过Softmax损失函数和中心损失函数的联合监视的CNN在几个重要人脸识别基准上取得了最高的准确率。

### Center Loss

  设c为类深度特征中心，x为各特征向量，L为梯度。则center loss的公式为:

<center><img src="{{ site.baseurl }}/images/pdReg/centerloss1.png"></center>

  在训练时，中心损失由SGD进行优化，类特征中心c应随着特征的改变而更新，即需要输入整个训练集并将每次迭代得到的特征取平均，这样的做法不切实际。文中采用的方式是基于mini-batch来更新类特征中心，在每次迭代中，类中心通过类特征取平均得到（即有些类中心可能会不更新）；为了避免错误标签带来大扰动，文中使用一个标量来控制类中心的学习率。梯度L和类中心c的计算如下：

<center><img src="{{ site.baseurl }}/images/pdReg/centerloss2.png"></center>

<center><img src="{{ site.baseurl }}/images/pdReg/centerloss3.png"></center>

<center><img src="{{ site.baseurl }}/images/pdReg/centerloss4.png"></center>

### 实验结果

  model A仅以softmax loss作为监督信号； model B以softmax loss和contrastive loss作为监督信号； model C以softmax loss和center loss作为监督信号。结果对比如下：

<center><img src="{{ site.baseurl }}/images/pdReg/centerloss5.png"></center>


