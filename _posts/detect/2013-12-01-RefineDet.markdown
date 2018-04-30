---
title: RefineDet（2017）
date: 2018-04-30 20:00:00
categories: fDetect
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

论文： Zhang S, Wen L, Bian X, et al. Single-Shot Refinement Neural Network for Object Detection[J]. 2017.

Github：[https://github.com/sfzhang15/RefineDet](https://github.com/sfzhang15/RefineDet)

### 论文算法概述

       提出的是一个single-stage检测器，号称达到two-stage检测器的精度和one-stage检测器的效率，名为RefineDet,。该网络包含anchor精炼模块(anchor refinement module, ARM)和目标检测模块(object detection module, ODM)。anchor精炼模块的作用有两点，1）过滤负的anchors以减少分类器的搜索空间；2）粗略调整anchors的位置和大小为随后的回归器提供更好的初始化。而目标检测模块则将精炼后得到的anchors作为输入去提升回归和多标签预测的精度。同时作者还设计了一个transfer connection block(TCB)去转移anchor精炼模块中的特征到目标检测模块去预测位置、大小和类别。该网络在VOC和COCO上达到最优， VOC07为85.8%%mAP，VOC12为86.8% ，MS COCO为41.8%。

<center><img src="{{ site.baseurl }}/images/pdDetect/refinedet1.png"></center>
   
### Network Architecture

* Transfer Connection Block：为了连接ARM和ODM，引入TCB去将ARM里不同层的特征转换城ODM需要的，这样ODM可以共享ARM上的特征。TCB的另一个作用是整合大尺度上下文信息，通过添加高级特征到转移的特征上去提高检测准确率。为了匹配它们之间的维度，使用反卷积去加大高级特征并逐位相加。在相加后面添加一个卷积层以确保特征的检测区分能力。TCB结构如图2所示。

<center><img src="{{ site.baseurl }}/images/pdDetect/refinedet2.png"></center>

* Two-Step Cascaded Regression： 当前的one-stage方法一般都依赖与一个one-step回归，而这个回归是基于多个不同尺度的特征层去预测目标的位置和大小，但这对于某些情况，准确率并不高，特别是对于小目标的检测。所以这里提出了一种two-step的级联回归策略。首先使用ARM去调整anchors的位置和大小，为ODM中的回归提供更好的初始化。更具体地，将n个anchor boxes与特征图上每一个被切分的cell相关联。相对于与之对应的cell，每个anchor box的初始位置都是固定的。在每个特征图的cell上，我们为每个精炼的anchor boxes预测一个相对于原始anchor位置的偏移量以及是否存在目标的两个评分。因此，我们可以在每个特征图cell上得到n个精炼anchors。  在得到精炼的anchor boxes后，将它们通过在ODM上对应的特征图去进一步生成目标类别和准确的目标位置和大小，如图1所示。在ARM和ODM上的对应的特征图的维度是一致的。我们计算c个类别分数，和4个相对于精炼anchor boxes的偏移量，即为每个精炼anchor box生成c+4维度的输出以完成整个检测任务。这个过程跟SSD中的default boxes相似，不同点是，SSD直接使用均匀平铺默认框，而RefineDet则使用两步策略，ARM生成精炼过的anchor boxes，而ODM以精炼anchor boxes作为输入进一步检测。

* Negative Anchor Filtering：为了早一步排除容易区分的负anchor并减轻样本不均衡的问题，作者设计了一个负anchor过滤机制。在训练阶段，对于一个精炼anchor box，如果它的负类评分高于预先设置的阈值，则在ODM的训练中被排除掉。就是说，在ODM训练时只用了了精炼过的困难的负anchor和精炼过的正anchor。同时在前向推理阶段，如果一个精炼anchor box预测到的负类评分高于阈值，在检测时也同样会被排除掉。

* Loss Functio：包含ARM的和ODM的两部分。

<center><img src="{{ site.baseurl }}/images/pdDetect/refinedet3.png"></center>
	
### Experiments

<center><img src="{{ site.baseurl }}/images/pdDetect/refinedet4.png"></center>

<center><img src="{{ site.baseurl }}/images/pdDetect/refinedet5.png"></center>

<center><img src="{{ site.baseurl }}/images/pdDetect/refinedet6.png"></center>