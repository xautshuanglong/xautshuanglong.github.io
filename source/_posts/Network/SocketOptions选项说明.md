---
title: Socket 选项说明
date: 2018-12-01 21:45:47
categories: Network
feature: /images/Network/socket_logo.png
tags: socket
toc: true
---

Socket 是应用层和传输层之间的一个抽象层，
有很多选项可对其进行控制：

|       |       |
|:------|:------|
| `SO_DEBUG` | `SO_OOBINLINE` |
| `SO_REUSEADDR` | `SO_LINGER` |
| `SO_TYPE` | `SO_RCVLOWAT` |
| `SO_ERROR` | `SO_SNDLOWAT` |
| `SO_DONTROUTE` | `SO_RCVTIMEO` |
| `SO_RCVBUF` | `SO_SNDtIMEO` |
| `SO_SNDBUF` | `TCP_MAXSEG` |
| `SO_KEEPALIVE` | `TCP_NODELAY` |

<!-- More -->

## Socket 选项简要列表
|     Level    |  Option Name  |  数据类型  |  说明  |
|:------------:|:--------------|:-----------|:-------|
|                         | SO_DEBUG         | int  |  打开调试信息  |
|                         | SO_REUSEADDR     | int  |  重用本地地址  |
|                         | SO_TYPE          | int  |  获取 socket 类型  |
|                         | SO_ERROR         | int  |  获取并清除 socket 错误状态  |
|                         | SO_DONTROUTE     | int  |  不查看路由表，直接发送给局域网内主机。  |
|                         | SO_RCVBUF        | int  |  TCP 接受缓冲区大小  |
|  SOL_SOCKET 通用选项    | SO_SNDBUF        | int  |  TCP 发送缓冲区大小  |
|                         | SO_KEEPALIVE     | int  |  发送周期性保活报文以维持连接  |
|                         | SO_OOBINLINE     | int  |    |
|                         | SO_LINGER        | linger|  若有数据待发送。则延迟关闭。  |
|                         | SO_RCVLOWAT      | int   |  TCP 接受缓冲区低水位标记  |
|                         | SO_SNDLOWAT      | int   |  TCP 发送缓冲区高水位标记  |
|                         | SO_RCVTIMEO      | timeval |  接收数据超时  |
|                         | SO_SNDTIMEO      | timeval |  发送数据超时  |
|  IPPROTO_IP IPV4选项    | IP_TOS           | int  |  服务类型  |
|                         | IP_TTL           | int  |  存活时间  |
|                         | IPV6_NEXTHOP     | socketaddr  |  下一跳 IP 地址  |
|  IPPROTO_IPV6 IPV6选项  | IPV6_RECVPKTINFO | int  |  接收分组信息  |
|                         | IPV6_DONTFRAGi   | int  |  禁止分片  |
|                         | IPV6_RECVTCLASS  | int  |  接收通信类型  |
|  IPPROTO_TCP TCP选项    | TCP_MAXSEG       | int  |  TCP 最大报文段大小  |
|                         | TCP_NODELAY      | int  |  禁止 Nagle 算法  |

## Socket 选项详细说明
### SO_DEBUG
### SO_REUSEADDR
### SO_TYPE
### SO_ERROR
### SO_DONTROUTE
### SO_RCVBUF
### SO_SNDBUF
### SO_KEEPALIVE
### SO_OOBINLINE
### SO_LINGER
### SO_RCVLOWAT
### SO_SNDLOWAT
### SO_RCVTIMEO
### SO_SNDTIMEO
### IP_TOS
### IP_TTL
### IPV6_NEXTHOP
### IPV6_RECVPKTINFO
### IPV6_DONTFRAG
### IPV6_RECVTCLASS
### TCP_MAXSEG
### TCP_NODELAY

