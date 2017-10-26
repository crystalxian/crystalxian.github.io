---
layout: post
title:  "Rethinking the Inception Architecture for computer Vision"
date:  2017-10-26
categories: Model
tags: DeepLearning
---

* content
{:toc}




在第一版googLeNet的基础上，增大了网络的结构。目标是，通过在合适地分解卷积和进行正则化(标签正则化)，最有效地利用增加的这些计算量。

>Q:如何做到4个model结果的ensemble，会明显提升效果。


## 介绍

本文介绍一些通用的设计准则以及优化思路，来以一种efficient的方式来扩大卷积网络的结构。

>Q:This is enabled by the generous use of dimensional reduction and parallel
>structures of the Inception modules which allows for mitigating the impact of
>structual changes on nearby components. 什么是nearby componets?

## General Design Principles

1.
避免表达瓶颈，特别是在网络靠前的地方。信息流前向传播过程中不能经过高度压缩的层。从input到output，feature
map的宽和高都应该逐渐减小，不能一下子变得很小。另外输出的channel数，一般来说都会逐渐增多，但是维度并不是代表信息的多少，只是一种估计的手段(这种表示丢掉了相关结构)。

2. 在网络中对高维表达进行局部表达，使得网络的训练更快。
>Q: Increasing the activations per tile in a convolutional network allows for
>more disentangled features. activations是指？ 解耦的特征？

3.
在较低维度的输入上进行空间聚合，不会造成表达损失。比如在进行3x3卷积之前，可以对输入先进行降维处理，而不会产生严重后果。原因是，如果对结果进行空间聚合，在降维过程中，临近区域单元的强相关性使得信息量有很少的减少。

4.宽度和深度保持在一个平衡的状态。

## 大卷积的分解

### 分解为小卷积

将5x5的卷积替换成两个3x3的卷积叠加。

![](https://pic4.zhimg.com/v2-114869f84a1057f4575042d3424b42e3_b.jpg)

参数量和计算量都有所下降。

>Q: If we would naivly slide a network without reusing the computation between neighboring grid tiles, we would increase the computational cost. sliding this network
can be represented by two 3x3 convolutional layers which reuses the
activations between 'adjacent tiles'.  This way, we end up with a net 9+9/25 x
reduction of computation, resulting in a relative gain of 28% by this
factorization.  The exact same saving holds for the parameter count as each parame-
ter is used exactly once in the computation of the activation of each unit. 

非线性激活会提高性能，相比较与线性激活。

### 空间分解为不对称的卷积核

![](http://img.blog.csdn.net/20170329151510550?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3NwYmE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

3x3卷积核改为3x1和1x3的卷积核，降低了33%的计算量。Generally，衍生出以下结构：

![](http://img.blog.csdn.net/20170329151851164?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3NwYmE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

但是作者发现，在较前面使用这种结构效果不好，在mxm的中间的feature
map上，其中m在12到20之间，效果比较好。

### 添加附加的分类器

之前GoogLeNet的论文中，提到附加分类器可以加强收敛，防止在深度网络中的梯度消失现象。但是这篇文章认为，附加的分类器起到了正则化的作用。主分类器在辅助分类器有Batch-normalized
或者dropout层时，会有性能的提高。

### 有效降低特征图的大小

为了避免第一条准则中提到的计算瓶颈所造成的的信息损失，一般会通过增加滤波器的数量来保持表达能力，但是计算量会增加。作者提出了一种并行的结构，使用两个并行的步长为2的模块，P和C。P是一个池化层，C是一个卷积层，然后将两个模型的响应组合到一起：

![](http://img.blog.csdn.net/20170329153342989?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3NwYmE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这样既没有造成信息提取上的损失，也能够很好的降低维度。

参考文章

1. [一篇很好的解读](http://blog.csdn.net/wspba/article/details/68065564)

2.  [一篇很不错的总结(包括GoogLeNet_v1)](http://blog.csdn.net/shuzfan/article/details/50738394)

3.  [一篇对卷积和pooling的总结，有对1*1卷积的介绍](http://www.cnblogs.com/zf-blog/p/6075286.html)

