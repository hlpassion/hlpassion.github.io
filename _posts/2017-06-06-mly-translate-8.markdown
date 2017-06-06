---
layout:    post
title:    "Machine Learning Yearning 阅读笔记，动手写起来!"
subtitle:    "chapter08"
date:    2017-06-06
author:    "Heron"
header-img:    "img/post-bg-mly.webp"
header-mask:    0.3
catalog:    true
tags:
    - 博客
    - 翻译
    - 机器学习
---
## 8. Establish a single-number evaluation metric for your team to optimize
&emsp;&emsp;分类精度就是single-number evaluation metric(单数字评估指标)的一个例子：在你的分类器上运行开发集(或测试集)，得到一个样本分类正确的数字。通过这个指标，如果分类器A获得97%的正确率，分类器B获得90%的正确率，我们判断分类器A表现更加优越。
&emsp;&emsp;相比之下，Precision(准确率、查准率)和Recall(召回率、查全率)就不是一个单数字评估指标。它给出两个数字来评估你的分类器。使用多数值评估指标使得比较算法变得更加困难。假设你的算法表现如下：
![table](https://cloud.githubusercontent.com/assets/12608255/26774375/91f01aa6-4a02-11e7-98d4-1339d2488b01.png)
从表中数据看来，两个分类器都没有明显的优于另一个，因此无法指导你选择哪一个分类器。
>一个猫的分类器的(Precision)是指在开发集(或测试集)中检测出的所有有猫的图片中有多少比例是真正的有猫。它的查全率(Recall)指在开发集(或测试集)中所有真正有猫的图片有多少比例被检测出来了。在高查准率和高查全率之间通常需要权衡一下。

&emsp;&emsp;在开发中，你的团队将尝试许多关于算法结构、模型参数、特征选择等方面的想法。使用单数字评估指标比如精确度可以将你所有的模型通过它们在这一指标上的表现进行排序，快速决定哪一个Work的最好。
&emsp;&emsp;如果查全率、查准率你都关心的话，我建议使用标准方法之一将它们组合成一个数。比如，可以去两者的平均值，最终得到一个单个数字指标。或者，你可以计算“F1值”，是一种在平均值上做了改善的计算方法，比单纯的取平均值Work的更好。
>想了解更多关于F1值的内容，可以查看[wiki](https://en.wikipedia.org/wiki/F1_score)

![F1](https://cloud.githubusercontent.com/assets/12608255/26775996/fa38bd1e-4a09-11e7-88a6-bdf1bd3e1a9f.png)
  当你面临一堆分类器让你选择的时候，使用单数字评估指标可以帮助更快的做出决策。它可以为算法表现提供一个简洁的排名，从而给出一个清晰的方向。
  最后的例子，假如你已经得到了你的分类器在四个区域(美国、中国、印度、其他)的准确率。给出四个指标。对四个指标取均值或加权平均，最终得到一个指标。均值或加权平均是组合多个指标最常用的方法之一。

