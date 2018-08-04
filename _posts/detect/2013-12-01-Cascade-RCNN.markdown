---
title: Cascade R-CNN（2017）
date: 2018-08-04 14:00:00
categories: fDetect
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

论文：Cai Z, Vasconcelos N. Cascade R-CNN: Delving into High Quality Object Detection[J]. 2017.

Github: [https://github.com/zhaoweicai/cascade-rcnn](https://github.com/zhaoweicai/cascade-rcnn)

### 论文算法概述

    一个检测器若是以IOU的阈值为0.5进行训练的，通常会产生一些误检且候选框质量较差，但若提高IOU的阈值，检测性能会急剧下降。主要原因有以下两点：一是IOU阈值提高，正样本数量会大量减少，容易导致过拟合；二是训练时用的IOU阈值与预测时的IOU不对应。所以提出了Cascade R-CNN, 包含一系列由递增的IOU阈值分阶段（级联）训练出来的检测器，以更有选择性的防止误报。用重采样来促使所有检测器的的正样本训练集大小接近以减轻过拟合问题。在预测时以同样的级联过程进行，使每个阶段的候选框和检测器质量之间更紧密地匹配。
    
<center><img src="{{ site.baseurl }}/images/pdDetect/cascade_rcnn1.png"></center>
	   
<center><img src="{{ site.baseurl }}/images/pdDetect/cascade_rcnn2.png"></center>
	   
### Detection Quality

   因为一个框会包含目标以及一些背景，一般会使用IOU来决定一个检测是正的还是负的。IOU高于阈值u的为正，否则为负。但无论阈值是多少，检测的设置都是高度相对的。当阈值高时，正例包含少量背景，而当阈值低时，更丰富和多样化的正例会被检测到，但误识也更多。所以一般很难要求一个模型能够对所有IOU阈值都表现很好。一个常用的折中的设置是将阈值定在0.5，检测质量较低，误检测较多。

   一个简单的解决方案是构建一个分类器的集合，如上图3(c)所示，以一个loss对应多个不同质量等级（不同IOU阈值）的分类器，然而每个等级的样本数量都不同，随着阈值的提高，正样本会急剧减少，会使对应高质量（阈值）分类器更容易过拟合。并且高质量的分类器被要求在推理过程中处理低质量分类器的候选框，这是没有被优化过的。基于这些问题，图3(c)的这种结构在大多数的质量等级（IOU阈值）上都难以得到更高的准确率，整体来说只比图3(a)的好一点点。

### Cascaded Bounding Box Regression

   Cascade RCNN是个级联回归的框架，如图3(d)，每一级回归器的训练样本都取自前一级的输出，逐级提升。如图3(b)上的iterative BBox框架中，为了定位准确，采用了级联结构来对Box进行回归，使用的head是一样的，那么就会有两个问题，一个是0.5的阈值无法适用于所有的候选框，如对IOU超过0.85时效果会严重退化；二是每一级都基于原始的数据分布进行优化的，但实际应用时，每一级的数据分布都将不同。而Cascade RCNN与之有以下几点不同，第一点是iterative BBox是以一个后处理过程去提升检测框质量，而级联回归则是个重采样过程，会改变不同阶段处理得到的候选框分布。第二点是级联回归过程在训练和预测上都会使用到，因此训练和预测的分布上是不冲突的。第三点是多个特定回归器都是由不同阶段重采样的分布上进行优化的，有针对性，不会出现不匹配的情况。
   
### Cascaded Detection

   像RPN等得到的初始的候选框的分布中低质量的会占大部分，如图4所示，这样会导致难以充分训练高质量的分类器。这里的Cascade RCNN通过以级联回归作为一个重采样机制来解决该问题。因为基于一个固定IOU阈值u去训练的回归器，会倾向于生成高于该阈值的候选框。再由这些新候选框得到的样本集上，以更高的IOU进行训练，即通过一级后，样本相当于被重采样过得到新的样本分布了。这样可以使从一个样本集开始，对样本的分布以更高的IOU进行连续的重采样，这重采样得到的多个分布做级联回归。因为每经过一级，候选框的IOU都更高质量更好，那么即使下一级的IOU阈值设置高一些，也不会有太多的样本被刷掉，这样就可以保证样本数量从而避免过拟合。同时每过一级检测器都是根据一个更高IOU阈值训练的，具有针对性地优化。
  
<center><img src="{{ site.baseurl }}/images/pdDetect/cascade_rcnn3.png"></center>
  
### Experiments

<center><img src="{{ site.baseurl }}/images/pdDetect/cascade_rcnn4.png"></center>

<center><img src="{{ site.baseurl }}/images/pdDetect/cascade_rcnn5.png"></center>

### 总结

* 一种级联的检测方案，多网络级联，训练时每个网络的输入以前一个网络的输出为基础，且设置的IOU阈值逐级增加。

* 侧重点在于处理：1、单一IOU阈值无法处理各种IOU；2、高IOU阈值部分因样本缺少容易过拟合导致性能差。

* 处理方案是级联并逐级增加IOU阈值，这样首先一点是可以使每级针对一个阈值进行针对性优化，另外是相当于做了重采样，一定程度上维持样本数量（看Cascaded Detection一段）。