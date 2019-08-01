---
title: gRPC学习笔记
date: 2019-08-01 10:44:09
categories: Libraries
feature: /images/Libraries/grpc_logo.png
tags: gRPC
toc: true
---

Google RPC 框架

<!-- More -->

### 工作模式

gRPC 主要有四种 请求/响应 模式：
1. 简单模式（Simple RPC）
1. 服务端数据流模式（Server-side streaming RPC）
1. 客户端数据流模式（Client-side streaming RPC）
1. 双向数据流模式（Bidirectional streaming RPC）

[参考 gRPC双向数据流的交互控制](https://www.jianshu.com/p/5158d6686769)

