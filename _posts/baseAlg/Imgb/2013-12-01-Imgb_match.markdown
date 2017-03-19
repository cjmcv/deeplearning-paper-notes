---
title: 模板匹配
date: 2015-01-01 09:00:00
categories: fbImgb
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

### 模版匹配概念

与常用的字符串匹配相类似，如对于简单字符，可将其归一化后，选一相应的开始点后，依次遍历相乘，相同的目标点相乘为1，目标点与非目标点相乘为0，可计算出相似程度。

在OpenCv和EmguCv中支持以下6种对比方式：

1. CV_TM_SQDIFF 平方差匹配法：该方法采用平方差来进行匹配；最好的匹配值为0；匹配越差，匹配值越大。
  
2. CV_TM_CCORR 相关匹配法：该方法采用乘法操作；数值越大表明匹配程度越好。

3. CV_TM_CCOEFF 相关系数匹配法：1表示完美的匹配；-1表示最差的匹配。

4. CV_TM_SQDIFF_NORMED 归一化平方差匹配法

5. CV_TM_CCORR_NORMED 归一化相关匹配法
   			
6. CV_TM_CCOEFF_NORMED 归一化相关系数匹配法

---

### 图像尺寸大小归一化用于模板匹配

   <center><img src="{{ site.baseurl }}/images/pdBase/imgb_match1.png"></center>
   
---

### SSDA-序贯相似性检测

   一种改进的图像模板匹配方法：SSDA算法的相似性判决指标为图像像素灰度差的绝对值之和: <img src="{{ site.baseurl }}/images/pdBase/imgb_match2.png">
   
   步骤：
   
1. 预先设定好一个阈值序列thresh(n)，在匹配的时候，用模板遍历整个图像； 

2. 每当模板到一处地方的时候，随机抽取k个像素点计算相似度e(u,v,k)并与Thresh(k)进行比较，若e(u,v,k)<thresh(k)则继续抽取，k++，直到e(u,v,k)>=thresh(k)时停止，记下k；对不同的待匹配点进行上述匹配计算，最后取最大的 k值对应的待匹配点位置，认为这就是需要找的匹配点。

   SSDA 算法与一般的穷尽搜索算法的最大区别是：它不需要计算所选区域中所有点引起的误差之和，而是计算部分误差和序列 e，将 e 与一个已经定义的误差阈值序列比较，如前 m 个点引起的误差和大于第 m 个误差阈值时，停止计算，换下一个点进行匹配。但该方法的阈值选择较困难，阈值选取的好，处理速度快、跟踪准确。阈值选取过大，处理速度提不高；选取过小，跟踪又不准确。所以实际运用中多采用自动阈值。

---
   
### 基于FFT的快速模版匹配
   
   参考[基于PCA和分块FFT的快速模板匹配算法](http://www.docin.com/p-1243627283.html)
  