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

### 安全通信
1. gRPC 暂未提供读取证书及秘钥文件的接口，需要自行读取，或将证书或秘钥内容拷贝到变量中。（注意换行）
1. gRPC 暂不支持带密码的私钥，需要修改源码，加入秘钥短语参数才行。
1. gRPC 基于 HTTP2，证书验证时要求服务器名称匹配，否则建立链接失败。（关于服务器名称/别名，参见 [OpenSSL学习笔记](/2019/02/22/Libraries/OpenSSL学习笔记/)）

[参考 gRPC双向数据流的交互控制](https://www.jianshu.com/p/5158d6686769)

