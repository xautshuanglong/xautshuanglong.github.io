---
title: Markdown 语法笔记
date: 2016-04-10 15:46:20
categories: Hexo
tags: Markdown
---

### 概述
>* 宗旨 【实现易读易写】  
>* 兼容 【成为一种适用于网络的语言】  

<!--more-->
### 基本语法
| 效果 | 语法格式 | 示例效果 |
|:----:|:------:|:--------:|
| 加粗 | `**加粗**` | **加粗** |
| 斜体 | `*斜体*` | *斜体* |
|删除线|`~~text~~`|~~text~~|
| 内嵌 | <code>\`内嵌\`</code> | `内嵌` |
| 无序列表 | `* 列表` | 行首（\*后有空格） |
| 无序列表 | `+ 列表` | 行首（+后有空格） |
| 无序列表 | `- 列表` | 行首（-后有空格） |
| 有序列表 | `1. 列表` | 行首（.后有空格） |
| 引用 | `> 引用` | 行首（>后有空格） |
| 标题1 | `# 标题1` |行首（#后有空格）|
| 标题2 | `## 标题2` |行首（#后有空格）|
| 标题3 | `### 标题3` |行首（#后有空格）|
| 链接 |`[百度](http://www.baidu.com "百度")`|[百度](http://www.baidu.com "百度")(标题双引号前有空格)|
| 图片 |`![Alert](/css/images/favicon.ico)`|![Alert](/css/images/favicon.ico)|

### 几点说明
1. 关于有序列表，序号可以不按顺序标记，甚至可以相同。
2. 有序列表和无序列表混杂时，需用换行来区分。
3. 引用区块中可以嵌套列表。
4. 空格可以用 Tab 代替。

### 特殊语法

*   代码区块  
行首4个空格，可用 Tab 代替，要注意 Tab 的长度（4个空格以上）。  
将被解释成以下 html 格式：
        <pre>
            <code>这是代码区块中的文字。</code>
        </pre>
效果如下：
        这是代码区块中的文字。  
*   链接的几种实现  
    1. 内联式  
            This is [an example](/ "home") inline link.
    This is [an example](/ "home") inline link.(可以使用相对路径)
    2. 参考式
	        [id]: / "home".
	        This is [an example][id] reference-style link.
		_注：\[id\]要在行首，可以有字母、数字、空白和标点符号，可以使用相对路径。_  
    This is [an example][id] reference-style link.
[id]: / "home"
*   自动链接  
        <http://www.baidu.com>
    <http://www.baidu.com>  
        <xjshuanglong@126.com>
    <xjshuanglong@126.com>

* 注释：不渲染到 HTML
        // 以下写法有效 Hexo Markdown
        [comment]: <> (This is a comment, it will not be included)
        [comment]: <> (in the output file unless you use it in)
        [comment]: <> (a reference style link.)
        [//]: <> (This is also a comment.)
        [//]: # (This may be the most platform independent comment)
        [^_^]: # (This is also a comment.)
    
        // 以下写法无效 Hexo Markdown
        [^_^]:
            commentted-out contents
            should be shift to right by four spaces (`>>`).

[comment]: <> (This is a comment, it will not be included)
[comment]: <> (in the output file unless you use it in)
[comment]: <> (a reference style link.)
[//]: <> (This is also a comment.)
[//]: # (This may be the most platform independent comment)
[^_^]: # (This is also a comment.)

*   反斜杠
可以利用反斜杠来插入一些在语法中有其它意义的符号。  
例如：不要将文本两侧的`*`转为`<em>`标签
        *需要转换*
        \*不要转换\*

    *需要转换*  
    \*不要转换\*  

__需要转换的特殊字符如下所示：__  

|  符号  |   说明   |
|:------:|:-------:|
|   \\   |  反斜线  |
|   \`   |  反引号  |
|   \*   |   星号   |
|   \_   |   底线   |
|   \{\} |  花括号  |
|   \[\] |  方括号  |
|   \(\) |    括弧  |
|   \#   |   井字号 |
|   \+   |   加号   |
|   \-   |   减号   |
|   \.   | 英文句点 |
|   \!   |  惊叹号  |


__转义解决不了的字符__  
如果碰见特殊字符，可以通过 **Unicode** 编码进行处理：
eg:在表格中插入`|`,可用 `&#124;`.

