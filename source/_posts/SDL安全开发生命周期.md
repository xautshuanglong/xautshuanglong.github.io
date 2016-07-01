---
title: SDL 安全开发生命周期
date: 2016-07-01 09:29:07
categories: VisualStudio
tags: VC
---

## 一、关于SDL
安全开发生命周期`Security Development Lifecycle`

早期版本中对于不安全的函数会给出警告提示，通常在函数后加上`_s`表示该函数的安全版本。

忽略安全检查：
* 方法一：使用安全版本的函数`_s`，如`scanf_s`、`strncopy_s`等。
* 方法二：\_CRT_SECURE_NO_WARNINGS。源码顶端添加`#define _CRT_SECURE_NO_WARNINGS`或
项目属性 -> 配置属性 -> c/c++ -> 预处理器 -> 预处理器定义 -> `_CRT_SECURE_NO_WARNINGS`
* 方法三：关闭`SDL`检查(VS2015)
项目属性 -> 配置属性 -> C/C++ -> SDL检查 -> 否
