---
title: 内存池技术
date: 2018-04-17 16:24:33
categories: CommonSense
description: 内存宝贵 杜绝浪费
tags: MemPool
toc: true
---

## 简介

<!-- More -->

## 优势 及 缺陷

### 优势
* 原理上比`malloc`及`new`快
* 分配内存时不存在额外开销`overhead`
* 几乎不存在内存碎片
* 无须逐个释放对象（释放整个池）

### 缺陷
* 需要事先知道对象大小
* 需要根据实际情况调节

