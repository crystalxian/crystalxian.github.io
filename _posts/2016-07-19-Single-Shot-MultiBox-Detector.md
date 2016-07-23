---
layout: post
title:  "SSD learning Note"
date:   2016-07-19
categories: Object-Detection
tags: SSD CNN Detection
excerpt: 记录SSD的学习笔记。
---

* content
{:toc}

SSD的学习笔记。

---

## 综述

* Our approach, named SSD, discretizes the output space of bounding boxes into a set of bounding box priors over different aspect ratios and scales per feature map location.

    理解：diescretizes？priors？
* It completely discards the proposal generation step and encapsulates all the computation in a single network.

	亮点：makes SSD easy to train and straightforward to intergrate into systems that require a detection component.

---

## 介绍
* Region-based Convolutional Neural Networks(R-CNN) or its faster variants approach detection as a classification problem over object proposals, followed by (optional) regression of the bounding box coordinates.

	理解：R-CNN根据selective search选出region proposals，在利用CNN进行分类后，得到一系列的有类别（类别概率）的region proposals。通过nms的方法得到想要的bounding box（？）

* Given a set of fixed bounding box priors of different aspect ratios and scales, we train a network to select which priors contain objects of interest and adjust their coordinates to better match the object's shape.

* In our method, the offset adjustment and confidences for multiple categories of each prior are predicted from the underlying 1x1 feature at each location on a feature map, as opposed to the whole feature map as done in MultiBox and Yolo.
	
	主要的实现方式如下图所示：

 	![](/assets/ssd_intro.png)
	
	理解:？
* The set of bounding box priors we define is in the same spirit as the anchor boxes used in the Faster R-CNN.

	分析：

* SSD is the first work to combine the idea of convolutional bounding box priors with the ability to predict multiple object categories.

---

## 相关工作

* Faster-RCNN replaces selective search proposals by ones learned from a region proposal network(RPN), and introduces a method to integrate the RPN with Fast R-CNN by alternating between fine-tuning shared convolutional layers and prediction layers for these two networks.
	借鉴了anchor boxes的观点。

* If we only use one prior per location freom the topmost feature map,our SSD has similar architecture as OverFeat; if we use the whole topmost feature map for predictions instead of using the convolutional priors, we can approximately reproduce YOLO.

---

## 实现
* Training

> 用bi来代表每一个priors.每一个prior都包含bounding box和一系列物体类别的置信度。

> 预测box的坐标是通过把网络计算出的offsets加到相应的priors上，所有priors和ground truth boxes都是相对于全图的归一化坐标。'解释' 归一化的坐标使得我们不必担心输入图片的大小问题。

* 匹配问题

本文不定期更新。
