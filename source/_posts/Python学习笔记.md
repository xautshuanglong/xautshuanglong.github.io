---
title: Python 学习笔记
date: 2016-05-12 13:53:27
categories: Python
tags: Python
description: 记录成长的脚印
toc: true
---

>Python 是一门简单易学且功能强大的编程语言：
>拥有高效的高级数据结构；能够用简单而又高效的方式进行面向对象编程。
>Python 优雅的语法和动态类型，再结合它的解释性，使其在大多数平台的许多领域成为编写脚本或开发应用程序的理想语言。

<!--more-->
## Python 模块
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模块是包括 Python 定义和声明的文件。文件名就是模块名加上 .py 后缀。模块名（作为一个字符串）可以由全局变量 __name__ 得到。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模块可以像函数定义一样包含执行语句，这些语句通常用于初始化模块，它们只在模块第一次导入时执行一次。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;每一个模块拥有自己的私有语义表，因此模块作者可以在模块中使用一些全局变量，不会因为与用户的全局变量冲突而引发错误。
引用形式：`moduleName.itemName`。
注：
    出于性能考虑，每个模块在每个解释器回话中只导入一遍。
	如果修改了模块，需要重启解释权或者用 `reload()` 重新加载，如：`reload(moduleName)`。

### 导入方法
* import moduleName 需要用 `moduleName.itemName` 访问或调用模块中的定义。
* from moduleName import itemName1,itemName2 直接通过 `itemName` 访问或调用模块中定义。
* from moduleName import * 导入模块中的所有定义，除了以下划线 `"_"` 开头的
