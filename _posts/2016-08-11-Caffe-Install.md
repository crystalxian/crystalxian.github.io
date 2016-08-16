---
layout: post
title:  "Caffe安装"
date:   2016-08-11 
categories: Caffe
tags: Caffe 
---

* content
{:toc}

## 安装两点记录

> CPU的安装Makefile.config

> GPU的安装过程中

1. 出现的boost版本过高的问题

2. cmake出错的问题




---
CPU安装在虚拟机上的[Makefile.config](https://drive.google.com/open?id=0B1zyWUhQhBdWWkFxcG5KNS1WSDA)

1. 加上（位置无所谓）

```
LIBRARIES += glog gflags protobuf leveldb snappy \
             lmdb boost_system hdf5_hl hdf5 m \
             opencv_core opencv_highgui opencv_imgproc opencv_imgcodecs
```

2. 去掉'CPU\_ONLY :=1'前面的注释

3. 如果要用PYTHON\_LAYER，去掉相应行前的注释

4.  [hdf5的错误](http://blog.csdn.net/striker_v/article/details/51615197)

```
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial /usr/include/hdf5/serial
```

---

GPU安装[Makefile.config](https://drive.google.com/open?id=0B1zyWUhQhBdWdDR5ek5pTUNtcVE)

1. boost版本需要<1.6，否则会有问题

2. cmake的问题，SSD-caffe的安装也出现了这个问题。

```
error: ‘memcpy’ was not declared in this scope
   return (char *) memcpy (__dest, __src, __n) + __n;
```

解决方案

```
cd cmake->cuda.cmake
   if(UNIX OR APPLE)
    	list(APPEND CUDA_NVCC_FLAGS -Xcompiler -fPIC -D_FORCE_INLINES)
   endif()
```   
![compare](https://cl.ly/2J1a1r103u2B/CUDA.png)

错误+解决方案

```
error: make mistake  -lhdf5 cannot find
use cmake -DCMAKE-PREFIX-INSTALL=/anywhere you want 
```

