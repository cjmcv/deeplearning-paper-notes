---
title: Hopfield神经网络
date: 2015-01-01 11:00:00
categories: fbANNSVM
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<!--<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>
<center><img src="{{ site.baseurl }}/images/pdBase/svm_smo1.png"></center>-->

### 概述

   Hopfield网络为单层对称全反馈网，包含一组神经元和一组相应的单位延迟，构成一个多回路反馈系统。反馈回路的数量等于神经元数量。基本上每个神经元的输出都通过一个单位延迟元素被反馈到网络中另外的每一个神经元。换句话说，网络中没有自反馈。Hopfield网络可用于解决联想记忆和约束优化问题的求解。

   将Lyapunov定理用于分析反馈神经网络的稳定性，它指出在任何出初始条件下，该网络是全局渐进稳定的；吸引子固定点是能量函数的极小值，反之亦然。根据激励函数选取的不同，可分为离散型和连续型两种。DHNN：离散，常使用符号函数sgn(.)，主要用于联想记忆；CHNN：连续，常使用S型函数，主要用于优化计算。

   Hopfield网络有以下三个设定：1）网络中每一神经元的输出都反馈到所有的其他神经元上；    2）网络中没有自反馈，即<img src="http://latex.codecogs.com/gif.latex? w_{ii} = 0"/>；    3）网络权值矩阵是对称的，表示为<img src="http://latex.codecogs.com/gif.latex? W^T  = W"/>；

<center><img src="{{ site.baseurl }}/images/pdBase/ann_hnn1.png"></center>

个人理解：第1步用作学习，相当于平时的模型训练，这里不用训练直接算好权值即可；第2到4步为识别过程，所谓的探针 则为识别的目标，输入到在第1步学习好的网络中，其输出的结果可以认为是对训练该网络权值的输入向量 的联想。即所谓的用于联想记忆。

<center><img src="{{ site.baseurl }}/images/pdBase/ann_hnn2.png"></center>
