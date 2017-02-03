---
title: 正则化
date: 2015-01-01 12:00:00
categories: fbDnn
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<!--<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>
<center><img src="{{ site.baseurl }}/images/pdBase/svm_smo1.png"></center>-->

需要通过正则化来控制神经网络，使得它不那么容易过拟合。有几种正则化的类型供选择：

### L2正则化

   在损失函数里，加入对每个参数的惩罚度。对于每个权重w，我们在损失函数里加入一项<img src="http://latex.codecogs.com/gif.latex? (1/2) \lambda w^2 "/>，其中<img src="http://latex.codecogs.com/gif.latex? \lambda "/>是我们可调整的正则化强度。前面加上1/2的原因是，求导梯度的时候，刚好变成<img src="http://latex.codecogs.com/gif.latex? \lambda w"/>而不是<img src="http://latex.codecogs.com/gif.latex? 2 \lambda w"/>。L2正则化理解起来也很简单，它对于特别大的权重有很高的惩罚度，以求让权重的分配均匀一些，而不是集中在某一小部分的维度上。加入L2正则化项，其实意味着，在梯度下降参数更新的时候，每个权重以W += -lambda*W的程度被拉向0。

---

### L1正则化
 
   对于每个权重 w 的惩罚项为<img src="http://latex.codecogs.com/gif.latex? (1/2) \lambda |w| "/>。有时候，你甚至可以看到大神们混着L1和L2正则化用，也就是说加入惩罚项<img src="http://latex.codecogs.com/gif.latex? \lambda |w| + \frac{1}{2}\lambda w^2  "/>，L1正则化有其独特的特性，它会让模型训练过程中，权重特征向量逐渐地稀疏化，这意味着到最后，我们只留下了对结果影响最大的一部分权重，而其他不相关的输入(例如『噪声』)因为得不到权重被抑制。所以通常L2正则化后的特征向量是一组很分散的小值，而L1正则化只留下影响较大的权重。在实际应用中，如果你不是特别要求只保留部分特征，那么L2正则化通常能得到比L1正则化更好的效果。

---

### 最大范数约束
 
   它直接限制了一个上行的权重边界，然后约束每个神经元上的权重都要满足这个约束。实际应用中是这样实现的，我们不添加任何的惩罚项，就按照正常的损失函数计算，只不过在得到每个神经元的权重向量w之后约束它满足<img src="http://latex.codecogs.com/gif.latex? |w||^2  < c"/>。有些人提到这种正则化方式帮助他们提高最后的模型效果。另外，这种正则化方式倒是有一点很吸引人：在神经网络训练学习率设定很高的时候，它也能很好地约束住权重更新变化，不至于直接挂掉。

---

### Dropout
 
   在神经网络中的最常用正则化方法！

   训练神经网络模型时，如果训练样本较少，为了防止模型过拟合，Dropout可以作为一种trikc供选择。Dropout是hintion最近2年提出的，源于其文章Improving neural networks by preventing co-adaptation of feature detectors.中文大意为：通过阻止特征检测器的共同作用来提高神经网络的性能。
    
   Dropout是指在模型训练时随机让网络某些隐含层节点的权重不工作，不工作的那些节点可以暂时认为不是网络结构的一部分，但是它的权重得保留下来（只是暂时不更新而已），因为下次样本输入时它可能又得工作了。
    
---

   按照hinton的文章，他使用Dropout时训练阶段和测试阶段做了如下操作：

* 在样本的训练阶段，在没有采用pre-training的网络时（Dropout当然可以结合pre-training一起使用），hintion并不是像通常那样对权值采用L2范数惩罚，而是对每个隐含节点的权值L2范数设置一个上限bound，当训练过程中如果该节点不满足bound约束，则用该bound值对权值进行一个规范化操作（即同时除以该L2范数值），说是这样可以让权值更新初始的时候有个大的学习率供衰减，并且可以搜索更多的权值空间。
　　
* 在模型的测试阶段，使用”mean network(均值网络)”来得到隐含层的输出，其实就是在网络前向传播到输出层前时隐含层节点的输出值都要减半（如果dropout的比例为50%），其理由文章说了一些，可以去查看。

---

  关于Dropout，文章中没有给出任何数学解释，Hintion的直观解释和理由如下：

1. 由于每次用输入网络的样本进行权值更新时，隐含节点都是以一定概率随机出现，因此不能保证每2个隐含节点每次都同时出现，这样权值的更新不再依赖于有固定关系隐含节点的共同作用，阻止了某些特征仅仅在其它特定特征下才有效果的情况。
　 
2. 可以将dropout看作是模型平均的一种。对于每次输入到网络中的样本（可能是一个样本，也可能是一个batch的样本），其对应的网络结构都是不同的，但所有的这些不同的网络结构又同时share隐含节点的权值。这样不同的样本就对应不同的模型，是bagging的一种极端情况。个人感觉这个解释稍微靠谱些，和bagging，boosting理论有点像，但又不完全相同。

3. native bayes是dropout的一个特例。Native bayes有个错误的前提，即假设各个特征之间相互独立，这样在训练样本比较少的情况下，单独对每个特征进行学习，测试时将所有的特征都相乘，且在实际应用时效果还不错。而Droput每次不是训练一个特征，而是一部分隐含层特征。

4. 还有一个比较有意思的解释是，Dropout类似于性别在生物进化中的角色，物种为了使适应不断变化的环境，性别的出现有效的阻止了过拟合，即避免环境改变时物种可能面临的灭亡。

<center><img src="{{ site.baseurl }}/images/pdBase/dnn_regular1.png"></center>

