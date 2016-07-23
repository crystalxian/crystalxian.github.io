---
layout: post
title:  "R-Cnn算法理解"
categories: Rcnn
tags:  Rcnn caffe 物体识别
---

* content
{:toc}

## Rcnn是使用深度学习进行物体检测的开山之作。




## 基础知识
** IOU的定义
物体检测需要定位出物体的bounding box，不仅需要我们可以分辨出这个物体是车，还需要我们知道这个车的bounding box。IOU是定义bounding box定位精度的。

![](http://img.blog.csdn.net/20160314191626423)

