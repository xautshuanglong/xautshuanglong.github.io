---
title: CPlusPlus中的坑
date: 2016-11-04 10:21:15
categories: CPP
description: 避免重复跳坑
toc: true
tags:
---

> 吃一堑 长一智
不要在同一个坑里挣扎
<!--More-->

### Visual Studio 运行时库(`Runtime Library`)
|类型|简称|含义|对应的库|备注|
|:---:|:---:|:---:|:---:|:---:|
| Single-Thread | /ML | Release版的单线程静态库 | libc.lib | VS2003后被废弃 |
| Single-Thread Debug | /MLd | Debug版的单线程静态库 | libcd.lib | VS2003后被废弃 |
| Multi-Thread | /MT | Release版的多线程静态库 | libcmt.lib |  |
| Multi-Thread Debug | /MTd | Debug版的多线程静态库 | libcmtd.lib |  |
| Multi-Thread DLL | /MD | Release版的多线程动态库 | msvcrt.lib<br>msvcrtxx.dll |  |
| Multi-Thread DLL Debug | /MDd | Debug版的多线程动态库 | msvcrtd.lib<br>msvcrtxxd.dll | &nbsp; |

