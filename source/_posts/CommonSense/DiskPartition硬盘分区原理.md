---
title: 硬盘分区原理
date: 2019-02-18 11:39:41
feature: /images/CommonSense/DiskPartition_logo.png
tags: partition
toc: true
---

记录硬盘分区相关知识：磁盘分区表。

<!-- More -->
https://baike.baidu.com/item/%E7%A1%AC%E7%9B%98%E5%88%86%E5%8C%BA%E8%A1%A8

## 硬盘分区表

### 识别标志
分区表 = 主分区 + 其他分区表
分区表位于硬盘某柱面 0 磁头 1 扇区。
主分区表（第一个分区表）总是位于 0 柱面 0 磁头 1 扇区。
其余分区表可由主分区表依次推导出来。

### 结构含义

