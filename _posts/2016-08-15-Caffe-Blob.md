---
layout: post
title:  "Caffe Blob源码解读"
date:   2016-08-15 
categories: Caffe 
tags: Caffe Blob
---

* content
{:toc}

## Blob源码解读

补充Update()函数解读




---

有一篇较为完整的解析见[Blob解读](http://www.cnblogs.com/louyihang-loves-baiyan/p/5149628.html)

其中有些补充和更正

### Update

计算的是$$data=-diff+data$$

```
	caffe_axpy<Dtype>(count_, Dtype(-1),
	        static_cast<const Dtype*>(diff_->cpu_data()),
			        static_cast<Dtype*>(data_->mutable_cpu_data()));
	void caffe_axpy<float>(const int N, const float alpha, const float* X,
		      float* Y) { cblas_saxpy(N, alpha, X, 1, Y, 1); }
	\\其中两个1代表的是stride。
```

---
