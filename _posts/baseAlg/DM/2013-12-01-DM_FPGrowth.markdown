---
title: FP-Growth
date: 2015-01-01 09:00:00
categories: fbDM
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

### 概念

   详细参看[http://blog.csdn.net/javastart/article/details/50521453](http://blog.csdn.net/javastart/article/details/50521453)

   Frequent Pattern Growth，频繁模式增长。一种关联分析算法，它采取如下分治策略：首先将提供频繁项集的数据库压缩到一棵频繁模式树（FP-Tree），但仍保留项集关联信息；然后将这种压缩后的数据库划分成一组条件数据库，每个数据库关联一个频繁项，并分别挖掘每个条件数据库。

   <strong>FP-Growth和Apriori的区别：</strong>

* 优点：第一，不产生候选集，第二，只需要两次遍历数据库，大大提高了效率； 

* 缺点：实现比较困难，在某些数据集上性能会下降。
