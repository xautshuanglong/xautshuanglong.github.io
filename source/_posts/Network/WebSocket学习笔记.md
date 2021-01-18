---
title: WebSocket学习笔记
date: 2021-01-18 11:15:59
feature: /images/Network/ll.png
categories: Network
tags: websocket
toc: true
---

WebSocket

<!-- More -->

## 握手加密原理
Sec-WebSocket-Key: lNnmFaCQ0C8d0BGOeA5RGA==
Sec-WebSocket-Accept: jd5qokdDimNb4Vvtu5guseRRObw=
258EAFA5-E914-47DA-95CA-C5AB0DC85B11 GUID 魔数[RFC 6455](https://tools.ietf.org/html/rfc6455)

``` bash
// Sec-WebSocket-Accept = base64(sha1(Sec-WebSocket-Key + 258EAFA5-E914-47DA-95CA-C5AB0DC85B11)) // 纯字符串拼接
// 计算中间值保持原有数值（字节数组），切勿转成可见字符，避免计算错误。

import hashlib
import base64
base64.b64encode(hashlib.sha1('lNnmFaCQ0C8d0BGOeA5RGA==258EAFA5-E914-47DA-95CA-C5AB0DC85B11'.encode('utf-8')).digest())

// b'jd5qokdDimNb4Vvtu5guseRRObw=' // 输出内容
```

