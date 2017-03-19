---
title: Apriori
date: 2015-01-01 09:00:00
categories: fbDM
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

### 概念

   一种挖掘布尔关联规则频繁项集的算法；算法意义主要是为了提高数据访问效率，提升发现频繁项集的速度；算法使用了一种称为逐层搜索的迭代方法，“K-1项集”用于搜索“K项集”；

   <strong>Apriori性质：</strong>频繁项集的所有非空子集也必须是频繁的。

   <strong>Apriori算法分为两个阶段：</strong>1）寻找频繁项集（大于等于最小支持度的项集）；  2）由频繁项集产生强关联规则（大于等于最小支持度和最小置信度）。

---

### 例子

如下图，寻找频繁项集，支持度都从数据库D中算出，产生强关联规则：

<center><img src="{{ site.baseurl }}/images/pdBase/dm_gbdt1.png"></center>

---

* <strong>优点：</strong>易于编程实现；
   
* <strong>缺点：</strong>在大数据集上可能较慢：可能产生大量的候选集，可能需要重复扫描数据库。

---

ps：

支持度: P(A∪B)，即A和B这两个项集在事务集D中同时出现的概率。例子： support((apple,banana)->cherry) = 1/7 = 14.29% （7个交易中有一个交易是同时购买了三件商品）

置信度: P(B｜A)，即在出现项集A的事务集D中，项集B也同时出现的概率。	confidence((apple,banana)->cherry) = 1/2 = 50% (同时购买了apple 和banana的有两个交易，其中一个交易也购买了cherry，所以置信度是50%)
