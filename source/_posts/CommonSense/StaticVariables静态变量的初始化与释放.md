---
title: 静态变量的初始化与释放
date: 2018-12-13 15:22:46
categories: CommonSense
description: 由 VLD 引发的对静态变量初始化顺序的思考
tags: static
toc: true
---

> **前情提要：**
> VLD要记录每次的内存分配，它通过Windows提供的分配钩子allocation hooks来监视调试堆内存的分配。它是一个用户自定义的回调函数，在每次从堆中分配内存之前被调用，在初始化是VLD使用_CrtSetAllocation注册这个钩子函数。 全局变量在程序初始化时就初始化，如果将VLD作为一个全局变量就可以与程序一起启动，但是C/C++并没有约定全局变量初始化的顺序，如果其它全局变量的构造函数中有内存分配则可能无法检测到。
> https://blog.csdn.net/fan_hai_ping/article/details/8023433 

``` c
#pragma init_seg({ compiler | lib | user | "section-name" [, func-name]} )
```
https://docs.microsoft.com/en-us/cpp/preprocessor/init-seg?view=vs-2017

http://www.cnblogs.com/hgy413/archive/2011/10/15/3693581.html

