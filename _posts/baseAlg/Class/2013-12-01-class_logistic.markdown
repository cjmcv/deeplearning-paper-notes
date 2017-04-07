---
title: Logistic回归
date: 2015-01-01 09:00:00
categories: fbClass
---

### 概述

       Logistic回归分析的基本原理就是利用一组数据拟合一个Logistic回归模型，然后借助这个模型揭示总体中若干个自变量与一个因变量取某个值的概率之间的关系。可以从统计意义上估计出在其它自变量固定不变的情况下，每个自变量对因变量取某个值的概率的数值影响大小。

1. Logistic回归是一种用于多因素分析的模型，特别适用于因变量为离散型多项分类的资料;

2. 同时具有判别和预测功能；

3. 它对因变量的分布没有要求，从数学角度看，Logistic回归模型非常巧妙地避开了分类型变量的分布问题，补充完善了线性回归模型和广义线性回归模型的缺陷。

### 推导过程

<center><img src="{{ site.baseurl }}/images/pdBase/class_logi1.png"></center>

### 训练过程

<center><img src="{{ site.baseurl }}/images/pdBase/class_logi2.png"></center>

### 算法特点

<center><img src="{{ site.baseurl }}/images/pdBase/class_logi3.png"></center>

---

### Softmax

   该模型是logistic回归模型在多分类问题上的推广。Softmax降为二分类时即为logistic回归。
   
### Softmax回归 和 k个二元分类器 之间的选择（softmax在互斥时选择，多个logistic在不互斥时选择）

   例子，将图像分到三个不同类别中。(i) 假设这三个类别分别是：室内场景、户外城区场景、户外荒野场景。你会使用sofmax回归还是 3个logistic 回归分类器呢？ (ii) 现在假设这三个类别分别是室内场景、黑白图片、包含人物的图片，你又会选择 softmax 回归还是多个 logistic 回归分类器呢？在第一个例子中，三个类别是互斥的，因此更适于选择softmax回归分类器 。而在第二个例子中，三个类别不互斥，如室内场景图也可以是黑白图片，也可以同时包含任务，建立三个独立的 logistic回归分类器更加合适。

---

### PS

* 这只是特点，对求解参数无影响；但这个特点可表示LR可用于进行参数的筛选（特征选择），如w1影响小，可舍去，只保留影响大的特征，简化模型。

* Logistic回归对变量的要求：

1. 因变量为二分类或多分类变量； 

2. 自变量为数值变量、等级或二分类变量；  

3. 多分类的计数资料需进行变量转换，形成一组哑变量（dummy variable，0/1）；

* 这里使用sigmoid函数作为激活函数，是为了使输出的范围限制在0~1之间，且中间对比度拉伸使决策点处的对比度明显。

