---
title: (Face-Detection) - WiderFace
date: 2016-10-01 19:00:00
categories: fDataset
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

地址：[http://mmlab.ie.cuhk.edu.hk/projects/WIDERFace/index.html](http://mmlab.ie.cuhk.edu.hk/projects/WIDERFace/index.html)

### 简介

       WiderFace为人脸检测基准数据集，含32,203张图像和393,703个人脸标签，标签涵盖遮挡、姿态、事件类别和人脸框。因为样本量多标签丰富，所以也常被用于训练任务。WiderFace以60个事件类别（如交通、节日、游行等）为基础进行划分，每个事件类别中随机选择40% / 10% / 50%的数据分别作为训练集/验证集/测试集。在评估算法时分两种情形，一种是不限制训练集的，另一种是要在WiderFace提供的training/validation部分样本进行训练。评估机制与PASCAL VOC中的一致，测试图片不提供标签。
	   
<center><img src="{{ site.baseurl }}/images/pdDataset/widerface1.png"></center>

### 样本属性

* Scale. 以图像的高将人脸分成三个尺度：small（10-50个像素），medium（50-300个像素），large（300以上个像素）。这样的划分主要是考虑到通用目标的检测率和人眼的辨别能力。

* Occlusion. 对于评估一个人脸检测器来说，遮挡是一个很重要的因素。这里将遮挡看成是一个属性，并将人脸划分为三类，无遮挡、部分遮挡和严重遮挡，其中遮挡1-30%的为部分遮挡，30%以上的为严重遮挡。

* Pose. 与遮挡相似，定义两个等级，分成典型的和非典型的。roll或pitch角度大于30度，或yaw大于90度的认为是非典型的。

* Event. 不同事件通常对应着不同的场景。WiderFace包含60个事件类别，涵盖现实场景中的大量场景。为了评估事件对人脸检测的影响，用三个因素对每个事件进行描述：尺度、遮挡和姿态。对于每个因素，我们计算特定事件类型的检测率，然后进行排序，将事件分成三部分：easy(41-60类)，medium(21-40类)，hard(1-20类)。

<center><img src="{{ site.baseurl }}/images/pdDataset/widerface2.png"></center>

### 标签格式

<center><img src="{{ site.baseurl }}/images/pdDataset/widerface3.png"></center>
   
### 结果权衡指标

   这里所画曲线与FDDB的有所不同，以准确率为纵坐标，召回率为横坐标。
   
* 检测准确率P = 检测到的真人脸数/所有检测到的算法认为是人脸的数目，对应误检。

* 检测召回率R = 检测到的真人脸数/测试图片上的所有人脸的数目，对应漏检。

### 近期榜单数据（2018.04.25）

* Scenario-Ext（使用任意额外的训练数据），Faceness：0.704 / 0.573 / 0.273 - Easy / Medium / Hard。

<center><img src="{{ site.baseurl }}/images/pdDataset/widerface4.png"></center>

* Scenario-Int（使用WiderFace的训练/验证数据集，这一项为主），PyramidBox：0.956 / 0.946 / 0.887 - Easy / Medium / Hard。

<center><img src="{{ site.baseurl }}/images/pdDataset/widerface5.png"></center>