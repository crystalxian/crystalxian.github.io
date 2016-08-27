---
layout: post
title: "Latex+Texstudio+Windows笔记"
categories: Latex
tags: Latex 排版 Texlive
---

* content
{:toc}

### 笔记概述

TexLive+Texstudio
一个学期在渣笔记本上用WinEdt，觉得越用越不好用。~~趁有新的电脑，所以专门在知乎上转了一圈，发现大神推荐了这个配置，所以就毫不犹豫的下手了。这篇笔记主要总结一下自己迈过去的一些配置的坑。~~ 用了一段时间，发现Sublime编译XeLatex实在是有点慢，还是TexStudio带提示功能的专业Latex的软件比较适合这种还不太熟悉Latex的人用。




### 准备工作
~~[sublime 3 官网](http://www.sublimetext.com/blog/articles/sublime-text-3-build-3103)~~


[texlive 2016清华镜像](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet/)

windows用户直接下载一个.exe就好，不用去下源包。安装时间较长。

发现在自己的渣windows7电脑上配置的时候出了一个bug，所以根据提示选择unfold选项，然后在执行命令行，从中找到错误。 

> Can't spawn "cmd.exe": No such file or directory at tlpkg/TeXLive/TLUtils.pm line

解决方法：

> 在系统变量 path 增加一个项：C:\Windows\System32;

~~[sumatrapdf官网](http://www.sumatrapdfreader.org/free-pdf-reader.html)~~

