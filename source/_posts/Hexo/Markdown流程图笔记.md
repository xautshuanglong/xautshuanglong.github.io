---
title: Markdown流程图
date: 2018-09-24 16:04:36
categories: Hexo
mermaid: true
tags: Markdown
toc: true
---

## 简单流程图元素表示

<!-- More -->

### 1. Flowchart
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

### 2. Sequence
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

### 3. Mermaid Flowchart
``` mermaid
graph TD
    A --> B
    A --> C
    B --> D
    C --> D
```

``` mermaid
graph TD
    B["fa:fa-twitter for pease"]
    B-->C[fa:fa-ban forbidden]
    B-->D(fa:fa-spinner)
    B-->E(A fa:fa-camera-retro perhaps?)
```

### 4. Mermaid Sequence
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

### 5. Mermaid Gantt
``` mermaid
gantt
    dateFormat  YYYY-MM-DD
    title Adding GANTT diagram functionality to mermaid
    section A section
    Completed task            :done,    des1, 2014-01-06,2014-01-08
    Active task               :active,  des2, 2014-01-09, 3d
    Future task               :         des3, after des2, 5d
    Future task2              :         des4, after des3, 5d
    section Critical tasks
    Completed task in the critical line :crit, done, 2014-01-06,24h
    Implement parser and jison          :crit, done, after des1, 2d
    Create tests for parser             :crit, active, 3d
    Future task in critical line        :crit, 5d
    Create tests for renderer           :2d
    Add to mermaid                      :1d
```

### 6. Mermaid Class
``` mermaid
classDiagram
    Class01 &lt;|-- AVeryLongClass : Cool
    Class03 *-- Class04
    Class05 o-- Class06
    Class07 .. Class08
    Class09 --> C2 : Where am i?
    Class09 --* C3
    Class09 --|> Class07
    Class07 : equals()
    Class07 : Object[] elementData
    Class01 : size()
    Class01 : int chimp
    Class01 : int gorilla
    Class08 &lt;--> C2 : Cool label
```

