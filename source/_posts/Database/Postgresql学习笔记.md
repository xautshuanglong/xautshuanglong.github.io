---
title: Postgresql 学习笔记
date: 2020-02-08 02:22:28
categories: Database
description: Postgresql 数据库使用
feature: /images/Database/Postgresql_logo.jpg
tags: postgresql
toc: true
---

Postgresql 介绍

<!-- More -->

## Postgresql 客户端

### C API
### C++ API
qpxx 编译问题

1. runner 工程编译失败，取消 PQXX_SHARED 宏定义
2. unit_runner 工程编译失败，test_stream_from.cxx 找不到 pqxx::stream_from::extract_value<std::nullptr_t> 定义
   实际 stream_from.hxx 中声明，stream_from.cxx 中定义。解决方法：在 test_stream_from.cxx 中重新定义，可能未导出成功（模板没有实例化，懒）

