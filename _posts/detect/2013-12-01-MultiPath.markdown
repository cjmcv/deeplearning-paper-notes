---
title: MultiPath（Facebook, 2016）
date: 2017-10-15 22:00:00
categories: fDetect
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

论文： Zagoruyko S, Lerer A, Lin T Y, et al. A MultiPath Network for Object Detection[J]. 2016.

### 论文算法概述

       提出的MultiPath主要对Fast R-CNN做了以下三点修改：1、skip connections，用于融合多层的特征信息；2、foveal structure，用于发掘在各分辨率下物体的上下文信息；3、integral loss，用于提高检测效果。对于region proposals的提取则使用DeepMask替换Fast R-CNN的selective search，而MultiPath网络充当分类器。
	   
<center><img src="{{ site.baseurl }}/images/pdDetect/multipath1.png"></center>

### Foveal Structure

   在Fast R-CNN中，直接使用ROI-pooling对proposal区域进行操作而没有考虑到周围的信息，对于很小的物体上下文信息很重要（也可参考tinyface）。这里为了添加上下文信息，添加了四个裁剪区域，分别以region proposal为中心向外裁剪1、1.5、2和4倍，然后分别输入到ROI Pooling中，将输出结果拼接得到图中的4096x4的特征向量，用于后面的分类和回归。
   
### Skip Connections

   在Fast R-CNN中，ROI-pooling应用在VGG-D conv5层后面，这时的特征已经下采样了16倍，即16x16的图像输入会得到1x1大小的输出，信息大量丢失。文中使用conv3，conv4和conv5如图1中所示进行连接，作为foveal classifier的输入（sp：conv3仅连至1x，conv4连至1x/1.5x/2x），然后使用1 x 1卷积进行降维。
   
### Integral Loss

   Fast R-CNN的loss函数如下：<img src="{{ site.baseurl }}/images/pdDetect/multipath2.png">。在训练时，其IOU阈值设为0.5，即proposal与ground true box的IOU大于50，则为正，计算loss两项；否则为负，并直接忽略回归部分的loss项。但其分类部分的loss（Lcls）对于IOU在50以上的均同等对待，这样并不合理，理应是IOU越高者分数越高。所以作者对分类部分loss作出了修改：<img src="{{ site.baseurl }}/images/pdDetect/multipath3.png">，其中的ku的u对应的是IOU的阈值，在实际计算中，将这连续积分近似成du=5的累加，公式如下：<img src="{{ site.baseurl }}/images/pdDetect/multipath4.png">。作者使用的参数是n=6，阈值为{50，55，60，...，75}。
   
### Experiments

   <center><img src="{{ site.baseurl }}/images/pdDetect/multipath5.png"></center>
   
   <center><img src="{{ site.baseurl }}/images/pdDetect/multipath6.png"></center>
   
   <center><img src="{{ site.baseurl }}/images/pdDetect/multipath7.png"></center>
   
   <center><img src="{{ site.baseurl }}/images/pdDetect/multipath8.png"></center>
   
   <center><img src="{{ site.baseurl }}/images/pdDetect/multipath9.png"></center>
