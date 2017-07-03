---
title: Temporary Record
date: 2016-11-18 21:12:47
description: 临时记录，后续完善，以免忘记。
tags: C++
toc: true
---

值得学习或巩固的知识点
<!-- More -->

### 关于 C++11 新特性
[defaulted 和 deleted 修饰函数](https://www.ibm.com/developerworks/cn/aix/library/1212_lufang_c11new/)
[Deleted functions in C++11](https://www.ibm.com/developerworks/community/blogs/5894415f-be62-4bc0-81c5-3956e82276f3/entry/deleted_functions_in_c_11?lang=zh)

[对象移动](http://www.voidcn.com/blog/chj90220/article/p-6228769.html)
[右值引用与转移语义](http://www.ibm.com/developerworks/cn/aix/library/1307_lisl_c11/)

### C# 相关

#### C# WinForm 应用退出
[C# WinForm程序退出的方法](http://www.cnblogs.com/yugen/archive/2010/08/10/1796864.html)
[C# — WinForm 退出方法总结](http://blog.csdn.net/yl2isoft/article/details/38168681)
[C# Enum,Int,String的互相转换 枚举转换](http://www.cnblogs.com/pato/archive/2011/08/15/2139705.html)

#### C# dllimport 类型转换
[C#调用dll时的类型转换总结](http://blog.chinaunix.net/uid-16685753-id-2738234.html)
[C#调用VC的DLL的接口函数参数类型转换一览表](http://www.cnblogs.com/Huayuan/archive/2012/07/05/2577439.html)

#### C# Windows 服务
[穿透Session 0 隔离（一）](http://www.cnblogs.com/gnielee/archive/2010/04/07/session0-isolation-part1.html)
[穿透Session 0 隔离（二）](http://www.cnblogs.com/gnielee/archive/2010/04/08/session0-isolation-part2.html)

### Windows
#### 查看 Windows 系统版本

命令行
winver
systeminfo

注册表
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion

#### Windows API
* 错误码处理
    GetLastError、FormatMessage
    https://msdn.microsoft.com/zh-cn/library/windows/desktop/ms680582(v=vs.85).aspx

* 文件版本信息
    GetFileVersionInfoSizeEx、GetFileVersionInfoEx、VerQueryValue

* 获取 Windows 特殊目录
    SHGetFolderLocation 
    SHGetFolderPath 
    SHGetSpecialFolderLocation 
    SHGetSpecialFolderPath 

#### COM 编程
Component Object Model
https://www.microsoft.com/com/default.mspx

### 需要总结内容
#### C++ 
* [Google开源项目风格](http://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/classes/)
* 初始化列表
* [STL 元素要求](http://jimmyleeee.blog.163.com/blog/static/930961820097510528758/)
* CComPtr 用法OB总结
* static 用法总结
    [static关键字详解](http://www.cnblogs.com/yc_sunniwell/archive/2010/07/14/1777441.html)
    [static作用](http://www.cnblogs.com/stoneJin/archive/2011/09/21/2183313.html)
* const 查看是否已完善
* extern 用法总结
    [多个源文件共用一个全局变量](http://blog.sina.com.cn/s/blog_74a459380101rjh4.html)
    [extern关键字详解](http://www.cnblogs.com/yc_sunniwell/archive/2010/07/14/1777431.html)
    [全局变量的声明和定义](http://blog.csdn.net/candyliuxj/article/details/7853938)
* inline 用法

#### BAT 批处理
[DOS批处理中%~dp0等扩充变量语法详解](http://www.jb51.net/article/97588.htm)


