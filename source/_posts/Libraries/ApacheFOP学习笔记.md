---
title: Apache FOP 学习笔记
date: 2018-10-10 10:45:38
categories: Libraries
feature: /images/Libraries/apache-fop-logo.jpg
tags: FOP
toc: true
---

> Apache™ FOP (Formatting Objects Processor) is a print formatter driven by XSL formatting objects (XSL-FO) and an output independent formatter. It is a Java application that reads a formatting object (FO) tree and renders the resulting pages to a specified output. Output formats currently supported include PDF, PS, PCL, AFP, XML (area tree representation), Print, AWT and PNG, and to a lesser extent, RTF and TXT. The primary output target is PDF.

<!-- More -->

## 问题处理

### 中文显示
1. 自动检测系统字体库
   `fop.xconf`默认配置为自动检测。
   注意使用正确的字体家族名称。（可使用 Windows 自带字体查看器查询）
![WinFontViewer.png](/images/Libraries/WinFontViewer.png)
``` xml
<fo:block color="green" font-family="华文行楷">
    Hello, <xsl:value-of select="name"/>!
    <fo:external-graphic src="asf-logo.png"/>
    中文测试
</fo:block>
```
1. 添加自定义字体库
在配置文件中注册字体`fop.xconf`
可以任意定义字体名称`font-triplet name="xxx"` ，使用时与其保持一致即可。`font-family="xxx"`
``` xml
<font kerning="yes" embed-url="HanBiaoLiXuKeShuFa.ttf">
    <font-triplet name="LiXuKeShuFa" style="normal" weight="normal"/>
</font>
```

[Apache™ FOP: Fonts](https://xmlgraphics.apache.org/fop/2.3/fonts.html)

## 推荐用法

### 应用集成
[Apache™ FOP: Embedding](https://xmlgraphics.apache.org/fop/2.3/embedding.html "How to Embed FOP in a Java application")

### 事件处理
[Apache™ FOP: Events/Processing Feedback](https://xmlgraphics.apache.org/fop/2.3/events.html)

