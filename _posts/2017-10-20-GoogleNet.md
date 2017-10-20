---
layout: post
title:  "Going Deeper with convolutions"
date:  2017-10-20
categories: Model
tags: DeepLearning
---

* content
{:toc}




>imageNet2014的比赛，论文中的方法是比赛的第一名，包括task1分类任务和task2检测任务。本文主要关注针对计算机视觉的高效深度神经网络结构，通过改进神经网络的结构达到不增加计算资源需求的前提下提高网络的深度，从而达到提高效果的目的。

## Main Contribution


*    Improve utilization of the computing resources inside the network, which is achieved by carefully crafted design and allows for increasing the
*    depth and width of the network while keeping the computational budget constant.

*    Architecture decisions are based on the Hebbian principle and the intuition of multi-scale processing.

*    A 22 layers deep network is assessed in the competition.

##  Ralated Work

*    本文提出的网络结构为Inception，得名于论文参考文献12（network in  network）。

*    Recent trend of CNN is to increase the number of layers and layer size, while using dropout to address the problem of overfitting。

*    论文参考文献15使用不同尺度的Gabor过滤器来处理多尺度问题，同本文的Inception Model类似。

*    本文借鉴参考论文12，使用了很多1×1的卷积核。卷积核在本文中的作用主要在于降维，以此来去除计算瓶颈。

*    Detection task’s leading approach is Regions with Convolutional Neural Networks(R-CNN) (参考文献6)。该方法分为两步：

*   First utilize low-level cues such as color and superpixel consistency for potential object proposals in a category-agnostic fashion.

*   Then use CNN classifiers to identity object categories at those locations.

## Motivation and High level considerations

### Drawback of increasing CNN size directly:

* More prone to overfitting.

* Dramatically increase use of computational resources. (for example, if most weights end up to be close to zero,then lots of computations is wasted.)

### How to solve it?

*   The fundamental way would be by ultimately moving from fully connected to sparsely connected architectures.

*   论文的参考文献2表明，考虑到统计相关性，一个稀疏网络结构可以重新构建出最优结构。并产生了Hebbian principle——neurons that fire together, wire together。

解决以上问题的根本方法就是把全连接的网络变为稀疏连接（卷积层其实就是一个稀疏连接），当某个数据集的分布可以用一个稀疏网络表达的时候就可以通过分析某些激活值的相关性，将相关度高的神经元聚合，来获得一个稀疏的表示。这种方法也呼应了Hebbian
	principle，一个很通俗的现象，先摇铃铛，之后给一只狗喂食，久而久之，狗听到铃铛就会口水连连。这也就是狗的“听到”铃铛的神经元与“控制”流口水的神经元之间的链接被加强了，而Hebbian principle的精确表达就是如果两个神经元常常同时产生动作电位，或者说同时激动（fire），这两个神经元之间的连接就会变强，反之则变弱（neurons that fire together, wire together）
