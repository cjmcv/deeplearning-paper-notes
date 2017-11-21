---
title: CLandmark（ICCV, 2015）
date: 2016-01-23 19:00:00
categories: fAlign
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

论文： Facial Landmark Tracking by Tree-Based Deformable Part Model Based Detector[C]// The IEEE International Conference on Computer Vision. IEEE, 2015:963-970.

项目主页(含代码): [http://cmp.felk.cvut.cz/%CB%9Curicamic/clandmark/](http://cmp.felk.cvut.cz/%CB%9Curicamic/clandmark/)

### 论文算法概述

       该跟踪器在视频每帧中独立使用静态图像关键点检测器，并采用卡尔曼滤波器去平滑人脸区域位置和降低人脸检测的失败率。而其中的静态关键点检测器使用的是由Structured Output SVMs训练而来的基于树的可变型部件模型DPM。基于树的DPM可由动态规划来得到全局推理过程，从而不会遇到在复杂形状模型中常遇到的问题陷入局部最优解； 基于树的DPM的主要使用问题是由组合推理造成的运算时间长，可以使用由粗到细的搜索策略来减轻。

1. 静态图像的关键点检测器：基于DPM，构建一个无向图模型，特征描述子由多尺度金字塔的稀疏局部二值模型（S-LBP）得到，使用SLBP主要是精度和速度之间的权衡。

2. 关键点检测器训练：由于是线性参数化，评分结果也是线性的，需要训练一个线性分类器，使用全监督的tructured output SVM (SOSVM)。使用Bundle Methods for Regularized Risk Minimization (BMRM)算法进行求解。

3. 粗到细的搜索策略：训练了两个分类器，（粗）一个输入图像为80 x 80，关键点的patchsize为13 x 13，用于限制人脸检测与得到脸部检测子； （细）检测器被使用160 x 160的图片，150 x 150的patchsize。具体如下图2所示。

4. 用kalman滤波器稳定人脸检测：前面定义的基于静态图像的处检测器，为了可扩展到视频流处理，我们需要解决一些人脸检测器可能存在的问题。特别地，使用了应用于商业的Waldboost face detector。然后使用kalman滤波器去估计人脸检测框。

<center><img src="{{ site.baseurl }}/images/pdAlign/clandmark1.png"></center>

### 使用感受：

1. 每帧都检测，检测速度较快，但不稳定,关键点跳动大；

2. 需要训练多个角度的不同模型，适合用于获取左右偏转情况，做正脸提取，而不太适合用于包含上下角度的姿态估计，可能是模型训练问题导致脸部倾斜无法检测。
