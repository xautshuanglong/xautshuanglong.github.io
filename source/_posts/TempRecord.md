---
title: Temporary Record
date: 2016-11-18 21:12:47
description: 临时记录，后续完善，以免忘记。
categories: CPP
toc: true
---

值得学习或巩固的知识点
<!-- More -->

## 关于 C++11 新特性
[defaulted 和 deleted 修饰函数](https://www.ibm.com/developerworks/cn/aix/library/1212_lufang_c11new/)
[Deleted functions in C++11](https://www.ibm.com/developerworks/community/blogs/5894415f-be62-4bc0-81c5-3956e82276f3/entry/deleted_functions_in_c_11?lang=zh)
[对象移动](http://www.voidcn.com/blog/chj90220/article/p-6228769.html)
[右值引用与转移语义](http://www.ibm.com/developerworks/cn/aix/library/1307_lisl_c11/)
[Range-based for 循环](https://www.cnblogs.com/h46incon/archive/2013/06/02/3113737.html)

## C# 相关
### C# WinForm 应用退出
[C# WinForm程序退出的方法](http://www.cnblogs.com/yugen/archive/2010/08/10/1796864.html)
[C# — WinForm 退出方法总结](http://blog.csdn.net/yl2isoft/article/details/38168681)
[C# Enum,Int,String的互相转换 枚举转换](http://www.cnblogs.com/pato/archive/2011/08/15/2139705.html)

### C# dllimport 类型转换
[C#调用dll时的类型转换总结](http://blog.chinaunix.net/uid-16685753-id-2738234.html)
[C#调用VC的DLL的接口函数参数类型转换一览表](http://www.cnblogs.com/Huayuan/archive/2012/07/05/2577439.html)

### C# Windows 服务
[穿透Session 0 隔离（一）](http://www.cnblogs.com/gnielee/archive/2010/04/07/session0-isolation-part1.html)
[穿透Session 0 隔离（二）](http://www.cnblogs.com/gnielee/archive/2010/04/08/session0-isolation-part2.html)

## Windows
### 查看 Windows 系统版本

命令行
winver
systeminfo
vmic `windows management instrumentation command-line`
注册表
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion

### Windows 路径
C:\Program Files (x86)\Windows Kits\10\Debuggers\x64\windbg.exe

### Windows 工具
* ProcessExplorer
* ProcessMonitor

### Windows API
* 错误码处理
    GetLastError、FormatMessage
    https://msdn.microsoft.com/zh-cn/library/windows/desktop/ms680582(v=vs.85).aspx
    https://msdn.microsoft.com/en-us/library/ms679351(v=VS.85).aspx
  [HRESULT 与 Windows Error Codes](http://www.cnblogs.com/greenerycn/archive/2010/08/30/hresult_and_win_error_codes.html)
  [C++异常继承关系](http://zh.cppreference.com/w/cpp/error/exception)

* 文件版本信息
    GetFileVersionInfoSizeEx、GetFileVersionInfoEx、VerQueryValue

* 获取 Windows 特殊目录
    SHGetFolderLocation 
    SHGetFolderPath 
    SHGetSpecialFolderLocation 
    SHGetSpecialFolderPath 
* 提高计时精度
    `NtSetTimerResolution` and `NtQueryTimerResolution`. All times are specifified in hundreds of nanoseconds.
* Timer 定时器
    `SetTimer` 利用 Windows 窗口消息 WM_TIMER 实现。要用 `KillTimer` 销毁定时器。注：工作线程（非UI）无法使用。
	`WaitableTimer` 可以跨线程、进程使用。`CreateWaitableTimer`创建定时器对象，`SetWaitableTime`设置回调，`CloseHandle`销毁定时器。
	`TimerQueueTimer` 相当强大，多种工作模式，精度高。
* 键盘按键检测
    GetAsyncKeyState _kbhit
* Microsoft Try-except Statement
  [try-except](https://msdn.microsoft.com/en-us/library/s58ftw19&#40;v=vs.140&#41;.aspx)
  <https://msdn.microsoft.com/en-us/library/s58ftw19(v=vs.140).aspx>
  [try-final](https://msdn.microsoft.com/en-us/library/9xtt5hxz.aspx)
  <https://msdn.microsoft.com/en-us/library/9xtt5hxz.aspx>
* 剪切板相关操作
    `GetClipboardData` `OpenClipboard` `GlobalLock` `GlobalUnlock`
* 系统信息 `sysinfoapi.h`
* 进程操作（权限问题）`OpenProcessToken` `LookupPrivilegeValue` `AdjustTokenPrivileges`
* 网络操作 `InternetAttemptConnect` `InternetCheckConnection` `InternetQueryOption` `InternetGetConnectedState`
  [WinINet Functions](https://docs.microsoft.com/zh-cn/windows/desktop/WinInet/wininet-functions)
  [IP Helper Functions](https://docs.microsoft.com/zh-cn/windows/desktop/IpHlp/ip-helper-functions)
  UDP 也可用 connect 操作，好处多多：绑定 IP，无需频繁建立与断开连接，拒绝第三方数据报干扰。
* `DIA` `Debug Interface Access SDK`，访问存储在PDB（Program Database）文件中信息的库。
  VisualStudio 安装目录下有库及示例：D:\Program Files (x86)\Microsoft Visual Studio 14.0\DIA SDK 。
  [官方参考](https://docs.microsoft.com/zh-cn/visualstudio/debugger/debug-interface-access/debug-interface-access-sdk?view=vs-2015)
* `PDH` `Performance Data Helper` 微软性能数据助手，如同性能监视器记录数据。[PDH header](https://docs.microsoft.com/en-us/windows/desktop/api/pdh/)
* 关于进程退出检测
  `std::atexit` 注册进程退出时调用的函数（可注册多个函数，后注册者先调用。）
* [Windows任务管理器API (博客)](https://blog.csdn.net/zhuyaojun/article/details/7066691)
* [System Information Functions](https://docs.microsoft.com/zh-cn/windows/win32/sysinfo/system-information-functions)
* [Volume Management Functions (官方)](https://docs.microsoft.com/zh-cn/windows/win32/fileio/volume-management-functions)
* [Snapshots of the System (官方)](https://docs.microsoft.com/en-us/windows/win32/toolhelp/snapshots-of-the-system)
* [Taking a Snapshot and Viewing Processes (官方)](https://docs.microsoft.com/en-us/windows/win32/toolhelp/taking-a-snapshot-and-viewing-processes)
* [Editing Drive Letter Assignments](https://docs.microsoft.com/zh-cn/windows/win32/fileio/editing-drive-letter-assignments)
* [Mounted Folders](https://docs.microsoft.com/zh-cn/windows/win32/fileio/volume-mount-points)
* [Mounted Folder Examples](https://docs.microsoft.com/zh-cn/windows/win32/fileio/volume-mount-point-examples)
* [Extensible Authentication Protocol Reference](https://docs.microsoft.com/zh-cn/windows/win32/eap/extensible-authentication-protocol-reference)
* [关于字符串长度](https://docs.microsoft.com/en-us/cpp/c-runtime-library/reference/strlen-wcslen-mbslen-mbslen-l-mbstrlen-mbstrlen-l?view=vs-2019)
* Session 远程桌面相关
  WTSQuerySessionInformationA function
* 文件操作 `SetFileAttributes` `CopyFile` `CopyFileEx`

### Windows Service
* Related API：`OpenSCManager`、`CreateService`、`OpenService`、`ControlService`、`DeleteService`、`RegisterServiceCtrlHandler`、`SetServiceStatus`、`StartServiceCtrlDispatcher`
* [Complete Service Sample](https://msdn.microsoft.com/EN-US/library/windows/desktop/bb540476&#40;v=vs.85&#41;.aspx)

### COM 编程
Component Object Model
https://www.microsoft.com/com/default.mspx

### Microsoft RPC
基于 命名管道 和 tcp/ip
* [RPC 编程](https://www.ibm.com/developerworks/cn/aix/library/au-rpc_programming/)
* [Windows RPC Demo实现](http://www.cnblogs.com/wanghaiyang1930/p/4469222.html)

### Thread Pooling
* [Thread Pools](https://msdn.microsoft.com/EN-US/library/windows/desktop/ms686756&#40;v=vs.85&#41;.aspx)
* [Process and Thread Functions](https://msdn.microsoft.com/EN-US/library/windows/desktop/ms684847&#40;v=vs.85&#41;.aspx)
    `QueueUserWorkItem`

### Synchronization Functions
* [Synchroization Function](https://msdn.microsoft.com/EN-US/library/windows/desktop/ms686360&#40;v=vs.85&#41;.aspx)
	`InterlockedIncrement` `InterlockedIncrement64`

### Unhandled Exception
* SetUnhandledExceptionFilter : Enables an application to supersede(紧接着...) the top-level exception handler of each thread of a process.
* _set_invalid_parameter_handler : Sets a function to be called when the CRT detects an invalid argument.
* _set_purecall_handler : Sets the handler for a pure virtual function call.
`RtlCaptureContext`

## 需要总结内容
### C/C++ 
* [Google开源项目风格](http://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/classes/)
* 初始化列表
* [STL 元素要求](http://jimmyleeee.blog.163.com/blog/static/930961820097510528758/)
* CComPtr 用法OB总结
* static 用法总结
  [static关键字详解](http://www.cnblogs.com/yc_sunniwell/archive/2010/07/14/1777441.html)
  [static作用](http://www.cnblogs.com/stoneJin/archive/2011/09/21/2183313.html)
* const 查看是否已完善
* 类型转换：`static_cast` `dynamic_cast` `const_cast` `reinterpret_cast` [参考](https://www.cnblogs.com/chio/archive/2007/07/18/822389.html)
* extern 用法总结
  [多个源文件共用一个全局变量](http://blog.sina.com.cn/s/blog_74a459380101rjh4.html)
  [extern关键字详解](http://www.cnblogs.com/yc_sunniwell/archive/2010/07/14/1777431.html)
  [全局变量的声明和定义](http://blog.csdn.net/candyliuxj/article/details/7853938)
* inline 用法
* explicit 用法
* #pragma pack([show]|[push|pop][, identifier], n)
* 仿函数 [【C++ STL】深入解析神秘的 --- 仿函数](http://blog.csdn.net/tianshuai1111/article/details/7687983)
* std::transform(retValue.begin(), retValue.end(), retValue.begin(), toupper);
* 类成员函数指针
* 重点 std::packaged_task std::future std::chrono
* WinUI : SkinUI DirectUI DuiEngine("svn checkout http://duiengine.googlecode.com/svn/trunk/ duiengine-read-only")
* C++11 异步编程
  [深入浅出std::async](http://www.cnblogs.com/chengyuanchun/p/5394843.html)
  [用C++11的std::async代替线程的创建](http://www.cnblogs.com/qicosmos/p/3534211.html)
  [协程概念](http://cnodejs.org/topic/58ddd7a303d476b42d34c911)
* 并发编程
  自旋锁：[自旋锁（spin lock）与互斥量的区别](http://blog.csdn.net/a675311/article/details/49096435), [【原创+整理】线程同步之详解自旋锁](http://www.cnblogs.com/cposture/p/SpinLock.html), [C++11实现自旋锁](http://blog.csdn.net/sharemyfree/article/details/47338001)
* 并行计算库 `TBB` 和 `PPL`
* [字符串 Hash 函数](https://www.byvoid.com/zhs/blog/string-hash-compare) `BKDRHash` `APHash` `DJBHash` `JSHash` `RSHash` `SDBMHash` `PJWHash` `ELFHash`
* window 内存泄漏检测：`VLD`、`_CrtDumpMemoryLeaks`、`_CrtMemCheckpoint`、`_CrtMemDifference`。
* 变量存储期：自动、静态、线程、分配。
* 仿 QFile QFileInfo 封装文件操作

### Java
* 高性存储
  [高性能队列——Disruptor](https://tech.meituan.com/disruptor.html)
  [并发框架Disruptor译文](http://ifeve.com/disruptor/)
* 文本格式化
    FOP xml、xslt、xpath、xsl-fo，文档格式化
    FreeMarker 模板引擎
* GC 算法相关
  [GC的三大基础算法](https://segmentfault.com/a/1190000004665100)
* [SLF4j](http://www.slf4j.org/)
  基于外观模式的日志框架，不提供具体日志功能，可与多种日志类库兼容。
  `Log4j + slf4j-log4j12`、`Logback`、`java.util.Logging`、`slf4j-simple` 等
* Spring Boot 相关
  [Spring Boot 中文索引](http://springboot.fun/)
  [纯洁的微笑-Spring Boot 系列完整](http://www.ityouknow.com/spring-boot.html)
* JVM内存结构、Java内存模型、Java对象模型区别。

### Python
* 经典示例
  [CPython解释器支持的线程和进程](https://www.cnblogs.com/wangkc/p/7122120.html)

### XML
* XML 解释器 `DOM` `SAX` `Xerces` `Castor`
  [解析xml文件的几种方法和原理](https://blog.csdn.net/witsmakemen/article/details/6980424)

### PDF
* 几个 PDF 软件库：`xpdf`，`poppler`，`mupdf`。
* [PDF 阅读器开发参考](https://blog.csdn.net/baihacker/article/details/36965841)
* [MuPDF 参考 xiangxw/mupdf-qt](https://github.com/xiangxw/mupdf-qt)

### VisualStudio
* VC 编译器参数设置
  [显示类内存布局](http://www.zyh1690.org/d1reportsingleclasslayout-msvc%E6%B1%87%E7%BC%96/)
* dllimport、dllexport
  [VS下 dllimport与dllexport作用与区别](http://blog.csdn.net/u010055724/article/details/51538686)

### BAT 批处理
  [DOS批处理中%~dp0等扩充变量语法详解](http://www.jb51.net/article/97588.htm)
* 系统应用：`shutdown`、`sc`、`reg`。

### Docker
* [Docker系列](https://www.cnblogs.com/ityouknow/category/1173004.html)

## 算法
* `局部敏感哈希`

## 网络协议

### Tools
* Windows CMD：`ping`、`netstat`、`arp`、`tracert`、`route`、`nbtstat`、`telnet`。

### FTP
* [FTP协议（指令集）](https://my.oschina.net/90888/blog/830379)

### ICMP
* [ICMP协议a](https://www.cnblogs.com/jingmoxukong/p/3811262.html)
* [ICMP Type 对应表](https://www.cnblogs.com/itcomputer/p/4939399.html)

### PXE
* PXE 网络装机环境搭建：`DHCP`，`TFTP`，`HTTP`。

## 性能分析
* VTune
  [Intel VTune Get Started](https://software.intel.com/en-us/vtune/documentation/get-started)
* 值得参考的内容内容
  [动态追踪技术漫谈](https://openresty.org/posts/dynamic-tracing/)
  [内核探测工具 Systemtap 简介](http://www.cnblogs.com/hazir/p/systemtap_introduction.html)
  [Ubuntu官网 wiki-Systemtap](https://wiki.ubuntu.com/Kernel/Systemtap)
  [在 Ubuntu12.04 上安装 Systemtap](http://www.linuxdiyf.com/linux/26714.html)
  [Linux 下的一个全新的性能测量和调试诊断工具 Systemtap](https://www.ibm.com/developerworks/cn/linux/l-cn-systemtap3/index.html)
  [Java 火焰图](https://colobu.com/2016/08/10/Java-Flame-Graphs/)
  [白话火焰图](https://huoding.com/2016/08/18/531)
  [Go 代码调优利器-火焰图](https://lihaoquan.me/2017/1/1/Profiling-and-Optimizing-Go-using-go-torch.html)
  [开源项目之调试 Python 应用生成性能 CPU 火焰图](http://xiaorui.cc/2015/10/22/%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE%E4%B9%8B%E8%B0%83%E8%AF%95python%E5%BA%94%E7%94%A8%E7%94%9F%E6%88%90%E6%80%A7%E8%83%BDcpu%E7%81%AB%E7%84%B0%E5%9B%BE/)
  [使用火焰图做性能分析](http://neoremind.com/2017/09/%E4%BD%BF%E7%94%A8%E7%81%AB%E7%84%B0%E5%9B%BE%E5%81%9A%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90/)
  [如何读懂火焰图](http://www.ruanyifeng.com/blog/2017/09/flame-graph.html)
  [linux 下用火焰图进行性能分析](https://blog.csdn.net/gatieme/article/details/78885908)

## 性能优化
* [高并发编程系列：4大JVM性能分析工具详解，及内存泄漏分析方案](https://youzhixueyuan.com/jvm-performance-analysis-tool.html)
* [阿里P8架构师谈：多线程、架构、异步消息、Redis等性能优化策略](https://youzhixueyuan.com/high-concurrency-performance-optimization-method.html)
* [阿里P8架构师谈：Web前端、应用服务器、数据库SQL等性能优化总结](https://youzhixueyuan.com/web-front-end-application-database-performance-optimization.html)
* [阿里P8架构师谈：MySQL数据库的索引原理、与慢SQL优化的5大原则](https://youzhixueyuan.com/index-principle-and-slow-query-optimization-of-mysql.html)
* [常用的后端性能优化六种方式：缓存化+服务化+异步化等](https://youzhixueyuan.com/six-ways-to-optimize-rear-end-performance.html)
* [阿里P8架构师谈：架构设计之数据库垂直、水平拆分六大原则](https://youzhixueyuan.com/six-principles-of-vertical-and-horizontal-resolution-of-database.html)
* [阿里P8架构师谈：MySQL慢查询优化、索引优化、以及表等优化总结](https://youzhixueyuan.com/mysql-slow-query-optimization-index-optimization.html)

