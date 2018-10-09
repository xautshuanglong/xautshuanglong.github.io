---
title: XSL学习笔记
date: 2018-10-09 11:29:06
categories: Web
tags: xsl
toc: true
---

> 起始于 XSL，结束于 XSLT、XPath 以及 XSL-FO

<!-- more -->

## Chrome XSLT转换无效
xml xslt 参见 [菜鸟教程](http://www.runoob.com/xsl/xsl-transformation.html)

教程示例显示正常（文件存于web服务器端）；
使用 IE 浏览器转换成功正常显示页面。

解决方案：命令行启动`chrome`并加上`--allow-file-access-from-files`参数。
***注意：终止所有已运行的`chrome`进程。***

