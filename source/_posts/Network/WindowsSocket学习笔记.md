---
title: WindowsSocket学习笔记
date: 2016-10-25 19:23:17
categories: Network
tags: [socket]
toc: true
---

winsock.h  htons函数  将端口号转换为网络字节顺序

## 三次握手与四次挥手
![https://baijiahao.baidu.com/s?id=1618114723935605183&wfr=spider&for=pc](/images/Network/tcp_connect_and_disconnect.jpg)

<!-- More -->

## TCP_IP 通信模型

1. `阻塞 send 和 recv (入门) `
   当 tcp 窗口太小时，send 会阻塞线程，当网络缓冲区没有数据时，recv 也会阻塞线程。
   弊端：性能低，多线程开销大。
1. `IO 多路复用 （中级）`
   通过轮询的方式主动检测是否可以收发：select 或者 Linux 下的 poll。
   弊端：轮询消耗性能，且可监视的套接字数量受操作系统限制。
1. `异步 （高级）`
   基于事件由操作系统主动通知：Windows 下 WSAAsyncSelect，Linux 下 epoll。
1. `完成端口 （锦上添花）`
   不仅通知可读可写，顺便完成收发工作。

