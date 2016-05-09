---
title: stringstream 陷阱
date: 2016-05-09 12:35:45
categories: C++
tags: stringstream
---

## stringstream 陷阱
stringstream 对象的构造和析构是非常耗 CPU 时间的。
如果打算在多次转换中使用同一个 stringstream 对象（效率高），那么最好每次转换前使用 clear() 方法。
注意：
* clear() 清空的是 stream 的状态，如：错误状态。
* .str(""),才能正确清空 stringstream。
