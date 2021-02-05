---
title: WebSocket学习笔记
date: 2021-01-18 11:15:59
feature: /images/Network/websocket_logo.png
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

## 第三方库

### QWebSocket

### libwebsockets
1. lejp libwebsockets embedded json parser
   可以解析部分目标字符串，若前部分解析没有语法错误，可以继续传入剩下数据，完成后续解析。
   libwebsockets-4.1.6 test-lejp.c 使用同一个 `lejp_ctx` 进行多次解析动作时出问题；首次解析没有问题，在完成解析后或出错后，继续解析新的 json 数据，则会报错，`lejp_parse` 返回 -21。
   经调试排除，`lejp\_ctx.st.s` 中记录了上一次解析时记录下的结束符号，导致新的 json 数据到来时，遇到开始符号与上次的完成符号不匹配，返回错误`LEJP_REJECT_MP_C_OR_E_NEITHER`(-21)。
   规避问题方法：每次完成一次完整解析（成功或失败都行），清除`lejp_ctx`中的信息，然后进行下一次解析动作。
``` c
// 增加清理上下文函数
static void lejp_reset(struct lejp_ctx *ctx)
{
    ctx->st[0].s = 0;
    ctx->st[0].p = 0;
    ctx->st[0].i = 0;
    ctx->st[0].b = 0;
    ctx->sp = 0;
    ctx->ipos = 0;
    ctx->outer_array = 0;
    ctx->path_match = 0;
    ctx->path_stride = 0;
    ctx->path[0] = '\0';
    ctx->line = 1;

    ctx->pst_sp = 0;
    ctx->pst[0].user = NULL;
    ctx->pst[0].ppos = 0;
}

// 在 callback 中适当位置执行上下文清理
static signed char cb(struct lejp_ctx *ctx, char reason)
{
    char buf[1024], *p = buf, *end = &buf[sizeof(buf)];
    int n;

    for (n = 0; n < ctx->sp; n++) { *p++ = ' '; }
    *p = '\0';

    if (reason & LEJP_FLAG_CB_IS_VALUE)
    {
        p += lws_snprintf(p, p - end, "   value '%s' ", ctx->buf);
        if (ctx->ipos)
        {
            int n;
            p += lws_snprintf(p, p - end, "(array indexes: ");
            for (n = 0; n < ctx->ipos; n++)
            {
                p += lws_snprintf(p, p - end, "%d ", ctx->i[n]);
            }
            p += lws_snprintf(p, p - end, ") ");
        }
        lwsl_notice("%s (%s)\r\n", buf, reason_names[(unsigned int)(reason) & (LEJP_FLAG_CB_IS_VALUE - 1)]);
        (void)reason_names;
        return 0;
    }

    switch (reason) {
    case LEJPCB_COMPLETE:
        lwsl_notice("%sParsing Completed (LEJPCB_COMPLETE)\n", buf);
        //lejp_reset(ctx);// Shuanglong
        break;
    case LEJPCB_PAIR_NAME:
        lwsl_notice("%spath: '%s' (LEJPCB_PAIR_NAME)\n", buf, ctx->path);
        break;
    case LEJPCB_FAILED:
        lwsl_notice("Parsing Failed ......\n");
        //lejp_reset(ctx);// Shuanglong
        break;
    }

    return 0;
}
```

2. test-lejp.c 中 `lws_snprintf` 缓冲区大小计算为：当前指针位置减去缓冲区末尾指针，存在大小端问题，导致部分格式化字符串无法正常输出。
   解决方法：判断地址大小，用大的减去小的，确保计算出来的缓冲区大小为非负数，再去做格式化。

