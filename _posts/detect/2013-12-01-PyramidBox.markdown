---
title: PyramidBox（Baidu, 2018）
date: 2018-05-03 20:00:00
categories: fDetect
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

论文： Tang X, Du D K, He Z, et al. PyramidBox: A Context-assisted Single Shot Face Detector[J]. 2018.

### 论文算法概述

   提出一个以上下文信息辅助的single shot人脸检测器，PyramidBox。主要有几个方面，1）提出一个基于anchor的用上下文信息辅助的方法，命名为PyramidAnchors，用来提供监督信息去学习小的、模糊的和部分遮挡人脸的上下文特征。2）提出Low-level Feature Pyramid Networks (LFPN)去融合适当的高级的上下文语义特征和低级的脸部特征，这同时也时PyramidBox能够一次过检测所有尺度的人脸；3）引入一种对上下文敏感的预测模块，包含一个混合的网络结构和max-in-out层从融合特征上去学习，增加预测网络的能力以提高最终输出的准确率。4）提出一种针对尺度的Data-anchor-sampling策略去改变训练样本的分布，侧重点在小人脸上，对小人脸训练样本进行扩增。5）PyramidBox在FDDB和WIDER FACE上取得了当前最优结果。
   
<center><img src="{{ site.baseurl }}/images/pdDetect/pyramidbox1.png"></center>
	   
### Network Architecture

   基于anchor的目标检测框架会带有复杂的anchor设计，在不同层级的特征图上进行预测能够有效处理不同尺度的人脸。同时FPN结构也展示了融合高低层级特征的好处。而PyramidBox，如图1所示，使用跟S3FD一样的VGG16骨干扩展和anchor尺度设计，可以生成不同层级的特征图和相同比例间距的anchors。将Low-level FPN添加到这个骨干上，然后将一个上下文敏感的预测模块作为金字塔每个检测层的分支网络去获取最终输出。关键在于我们设计了一个新颖的anchor金字塔方法，为每个人脸从不同层级上生成一系列的anchors。每个模块的细节如下：

* Scale-equitable Backbone Layers.我们使用基本的卷积层和S3FD中的额外卷积层作为骨干层，即留下了VGG16从conv1_1到pool5，将fc6和fc7改成conv_fc层，然后添加更多的卷积层使使网络变得更深。

* Low-level Feature Pyramid Layers.为了提高检测器对多尺度人脸的检测性能，带有高分辨率的低层级特征是很重要的。因此，不少论文提出的算法都是在一个框架内构建多个不同的结构去检测多尺度人脸。为了整合高层、级语义特征到高分辨的低层级特征上，FPN提出了一个top-down的方式去利用所有尺度的高层级语义特征，而最近基于FPN的网络框架在目标检测和人脸检测上都取得了很好的结果。

而这些工作都是从顶层开始构建FPN，这就有个值得争论的问题，那就是不一定所有的高层级特征都有助于小人脸检测。首先，小的、模糊的和部分遮挡的人脸跟大的、清晰的和完整的人脸的纹理特征都会不一样。因此直接使用所有的高层级特征去增强小人脸的检测性能是很粗暴的；其次，高层级特征都是从只有少量上下文信息的区域提取出来的，而且可能还会引入一些噪声信息。例如在PyramidBox的骨干层上，顶端的conv7_2和conv6_2两层的感受野分别是724和468。注意到训练图像输入是640，即意味着顶端两层只包含大尺度人脸的少量上下文特征，所以可能对中小人脸检测贡献不大。

作为另一种方案，我们从中间网络层构建Low-level Feature Pyramid Network (LFPN)，作为top-down结构的开端，则其感受野应该会接近输入图像的一半。另外，LFPN的每个block的结构与FPN一样，如图2(a)所示。

* Pyramid Detection Layers.我们选择lfpn_2、lfpn_1、lfpn_0、conv_fc7、conv6_2和conv7_2作为检测层，带有的anchor的大小分别为16、32、64、128、256和512。这里lfpn_2、lfpn_1、lfpn_0是基于conv3_3、conv4_3和conv5_3的LFPN的输出层。另外，与SSD风格的方法相似，我们使用L2归一化去对LFPN进行归一化。

* Predict Layers.每个检测层都接有一个对上下文敏感的模块Context-sensitive Predict Module(CPM)，其输出是用于监督金字塔anchors的。而金字塔anchors基本上会覆盖人脸、头部和身体区域。第l-th个CPM的输出大小为w_l x h_l x c_l，其中<img src="{{ site.baseurl }}/images/pdDetect/pyramidbox2.png">是对应的特征图大小，对l=0/1/2/.../5的通道数c_l为20。因此，每个通道的特征都分别用于分类和回归人脸、头部和身体，则人脸分类需要4(=cp_l + cn_l)个通道，其中cp_l和cn_l分别为max-in-out的前景和背景。满足条件：若l=0，cp_l=1,否则cp_l=3。另外，头部和身体的分类都需要两个通道，同时人脸、头部和身体的定位则需要4个通道。

* PyramidBox loss layers.对于每个目标人脸，我们会有一系列的金字塔anchor是去同时监督分类和回归任务。我们还设计了一个PyramidBox Loss，使用softmax loss用于分类和smooth L1 loss用于回归。

<center><img src="{{ site.baseurl }}/images/pdDetect/pyramidbox3.png"></center>

### Context-sensitive Predict Module

* Predict Module.在原始基于anchor的检测器上，如SSD和YOLO，目标函数都直接应用在选择的特征图上。如MS-CNN,扩大了每个任务的自网络去提高准确率；如SSH通过以不同的步长在网络层顶部使用更大的卷积预测模块来加大感受野；如DSSD在每个预测模块上添加残差blocks。确实，SSH和DSSD分别使预测模块变得更深和更宽，因此预测模块能得到更好的特征。受Inception-ResNet启发，设计了Context-sensitive Predict Module (CPM)，可以将DSSD中的残差预测模块去替换SSH中上下文模块的卷积层，这样使CPM可以获得DSSD方法的好处，同时保持SSH上下文模块的丰富的上下文信息。

* Max-in-out.Maxout的概念首次被Goodfellow在13年的Maxout networks中提出，S3FD使用了max-out背景标签去减少小负样本的false positive rate。这篇论文里，作者同时在正负样本上使用这种策略，表示为max-in-out。如图2c所示。首次提出在每个预测模块预测c_p + c_n分数，然后选择最大的c_p作为正例分数。类似地选择最大的c_n作为负例分数。在实验中，为第一个预测模块设置c_p=1和c_n=3，因为小anchor会有更复杂的背景，而其他预测模块则设为c_p=3和c_n=1去提高召回率。

### Context-reinforced PyramidAnchors

   最近提出的基于anchor的检测器效果都很好，但仍忽略了每个尺度的上下文特征，因为anchors都是为了人脸区域而设计的。为了解决这个问题，作者提出了使用anchor的另一种方式，PyramidAnchors。

   对于每个目标人脸，PyramidAnchors都生成一系列anchors，这些anchors都对应着相对与人脸更大的区域，即包含了更丰富的上下文信息，如头部、肩膀和身体。我们通过匹配区域大小和anchor大小来设置一层中的anchors，这样可以监督到更高层级的网络层去学习对于较低层级尺度的人脸更具表达性的特征。对头部、肩膀或身体给定额外的标签，我们可以准确地将anchors匹配到GT去生成loss。因为添加额外标签并不公平，所以我们先假定不同人脸目标区域有相同的比例和偏移量的具有类似的上下文特征,在这个假设下以半监督的方式实现。就是说，我们可以使用一系列的统一的boxes去近似头部/肩膀/身体的实际位置，如果在不同人脸上取自这些boxes的特征都相似的话。

<center><img src="{{ site.baseurl }}/images/pdDetect/pyramidbox4.png"></center>
   
   一个人脸会3个连续的预测模块中生成3个目标，分别代表这人脸本身以及对应该人脸的头部和身体。例子如图3所示，最大的紫色人脸框大小为128，在P3/P4和P5上有pyramid-anchors，其中P3是由conv_fc7生成的anchors，标签是人脸本身。P4则由conv6_2生成，以目标人脸的头部(大小约为256)作为标签。P5由conv7_2生成，以目标人脸对应的身体（大小约为512）作为标签。类似的，为检测最小的蓝绿色人脸，大小为16，可以从P0的以原始人脸为标签的pyramid-anchors上得到监督特征，P1对应大小为32的头部，P2对应大小为64的身体。
   
<center><img src="{{ site.baseurl }}/images/pdDetect/pyramidbox5.png"></center>

   需要注意的是pyramid anchors是自动生成的，而不需要额外的标签，半监督的训练方法使PyramidAnchor能提取到近似的上下文特征。在预测过程中，我们只需要使用人脸分支的输出，因此相对与其他标准的基于anchor的人脸检测器并没有额外的计算消耗。
   
### Data-anchor-sampling

   首先，随机在样本中选择一个人脸的大小s_face，如前面提到的PyramidBox的anchors的尺度设置如下：<img src="{{ site.baseurl }}/images/pdDetect/pyramidbox6.png">。令<img src="{{ site.baseurl }}/images/pdDetect/pyramidbox7.png">为离选择的人脸的最近的anchor尺度的下标，然后在集合<img src="{{ site.baseurl }}/images/pdDetect/pyramidbox8.png">选择一个随机下标i_target，最后缩放这个人脸大小s_face到<img src="{{ site.baseurl }}/images/pdDetect/pyramidbox9.png">。例如，我们随机选择一个人脸，假设其大小为140，那么最近的anchor-size为128，然后我们需要从16、32、64、128和256上选择一个目标大小。总之，假设选择的是32，那么我们将原始图像以缩放比例30/140=0.2285进行缩放。最后在缩放后的图像上裁剪640x640的包含有选择的人脸的图像块作为训练样本。如图4所示，data-anchor-sampling改变了训练样本的分布：1）小人脸的比例比大人脸的多；2）通过大人脸去生成小人脸以提高小人脸的多样性。

<center><img src="{{ site.baseurl }}/images/pdDetect/pyramidbox10.png"></center>
   
### PyramidBox loss

<center><img src="{{ site.baseurl }}/images/pdDetect/pyramidbox11.png"></center>

   例如，当k=0，则GT的标签与Fast-RCNN一样。而k大于等于1时，则可以通过GT人脸和下采样anchors之间的匹配来决定标签。此外，t为一个向量，代表预测框的4个参数，而t*则对应着带正anchor的GT框，定义为
   
<center><img src="{{ site.baseurl }}/images/pdDetect/pyramidbox12.png"></center>

### Experiments

<center><img src="{{ site.baseurl }}/images/pdDetect/pyramidbox13.png"></center>

<center><img src="{{ site.baseurl }}/images/pdDetect/pyramidbox14.png"></center>

<center><img src="{{ site.baseurl }}/images/pdDetect/pyramidbox15.png"></center>

<center><img src="{{ site.baseurl }}/images/pdDetect/pyramidbox16.png"></center>

   FDDB，2000 False positives下，Discrete：0.987；Continuous：0.860

<center><img src="{{ site.baseurl }}/images/pdDetect/pyramidbox17.png"></center>

   WiderFace，Val：0.961 / 0.95 / 0.889； Test：0.956 / 0.946 / 0.887。

<center><img src="{{ site.baseurl }}/images/pdDetect/pyramidbox18.png"></center>
   
   