---
title: Markdown流程图
date: 2018-09-24 16:04:36
categories: Hexo
mermaid: true
tags: Markdown
---

### 简单流程图元素表示

<!-- More -->

``` flow
st=>start: 开始
e=>end: 登录
io1=>inputoutput: 输入用户名密码
sub1=>subroutine: 数据库查询子类
cond=>condition: 是否有此用户
cond2=>condition: 密码是否正确
op=>operation: 读入用户信息
st->io1->sub1->cond
cond(yes,right)->cond2
cond(no)->io1(right)
cond2(yes,right)->op->e
cond2(no)->io1
```

``` flow
st=>start: Start
op=>operation: Your Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
```

``` sequence
title:这是一个例子
participant 网页
participant 商品详情服务器 as A
participant 商品价格服务器 as B
Note over 网页:用户选择商品
网页->A:查询
A->B:查询
B->B:查询DB并计算
B-->A:返回
A->A:获取其他信息
A-->网页:返回
```

``` mermaid
graph TD
    A --> B
    A --> C
    B --> D
    C --> D
```

``` mermaid
sequenceDiagram
    participant Alice
    participant Bob
    Alice->John: Hello John, how are you?
    loop Healthcheck
        John->John: Fight against hypochondria
    end
    Note right of John: Rational thoughts <br/>prevail...
    John-->Alice: Great!
    John->Bob: How about you?
    Bob-->John: Jolly good!
```

