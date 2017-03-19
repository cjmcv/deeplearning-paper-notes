---
title: 背景建模
date: 2015-01-01 09:00:00
categories: fbImgb
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

### 常用的传统背景建模算法

1. 单高斯模型

2. 混合高斯模型

3. 码本

4. 自组织背景检测

5. 样本一致性背景建模算法

6. 基于颜色信息的背景建模方法

7. 本征背景法

---

### 混合高斯背景模型

   混合高斯背景建模是基于像素样本统计信息的背景表示方法，利用像素在较长时间内大量样本值的概率密度等统计信息(如模式数量、每个模式的均值和标准差)表示背景，然后使用统计差分(如3σ原则)进行目标像素判断，可以对复杂动态背景进行建模，计算量较大。

   在混合高斯背景模型中，认为像素之间的颜色信息互不相关，对各像素点的处理都是相互独立的。对于视频图像中的每一个像素点，其值在序列图像中的变化可看作是不断产生像素值的随机过程，即用高斯分布来描述每个像素点的颜色呈现规律【单模态(单峰)，多模态(多峰)】。

   对于多峰高斯分布模型，图像的每一个像素点按不同权值的多个高斯分布的叠加来建模，每种高斯分布对应一个可能产生像素点所呈现颜色的状态，各个高斯分布的权值和分布参数随时间更新。当处理彩色图像时，假定图像像素点R、G、B三色通道相互独立并具有相同的方差。对于随机变量X的观测数据集{x1,x2,…,xN}，xt=(rt,gt,bt)为t时刻像素的样本，则单个采样点xt其服从的混合高斯分布概率密度函数：

   <center><img src="{{ site.baseurl }}/images/pdBase/imgb_background1.png"></center>

   <center><img src="{{ site.baseurl }}/images/pdBase/imgb_background2.png"></center>
   
### 码本

   为当前图像每个像素建立一个codebook结构。基本思想是得到每个像素的时间序列模型。这种模型能很好处理时间起伏。缺点是消耗大量的内存。参看[http://blog.csdn.net/tiemaxiaosu/article/details/51615262](http://blog.csdn.net/tiemaxiaosu/article/details/51615262)

### 自组织背景检测

   基于自组织神经网络，参看[http://blog.csdn.net/kezunhai/article/details/9499895](http://blog.csdn.net/kezunhai/article/details/9499895)

### 样本一致性

   参看[http://www.cnblogs.com/dwdxdy/p/3530862.html](http://www.cnblogs.com/dwdxdy/p/3530862.html)

   