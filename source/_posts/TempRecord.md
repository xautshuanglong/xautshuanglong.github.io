---
title: Temporary Record
date: 2016-11-18 21:12:47
description: 临时记录，后续完善，以免忘记。
categories: CPP
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

#### Windows 路径
C:\Program Files (x86)\Windows Kits\10\Debuggers\x64\windbg.exe


#### Windows API
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

#### Windows Service
* Related API：`OpenSCManager`、`CreateService`、`OpenService`、`ControlService`、`DeleteService`、`RegisterServiceCtrlHandler`、`SetServiceStatus`、`StartServiceCtrlDispatcher`
* [Complete Service Sample](https://msdn.microsoft.com/EN-US/library/windows/desktop/bb540476&#40;v=vs.85&#41;.aspx)

#### COM 编程
Component Object Model
https://www.microsoft.com/com/default.mspx

#### Microsoft RPC
基于 命名管道 和 tcp/ip
* [RPC 编程](https://www.ibm.com/developerworks/cn/aix/library/au-rpc_programming/)
* [Windows RPC Demo实现](http://www.cnblogs.com/wanghaiyang1930/p/4469222.html)

#### Thread Pooling
* [Thread Pools](https://msdn.microsoft.com/EN-US/library/windows/desktop/ms686756&#40;v=vs.85&#41;.aspx)
* [Process and Thread Functions](https://msdn.microsoft.com/EN-US/library/windows/desktop/ms684847&#40;v=vs.85&#41;.aspx)
    `QueueUserWorkItem`

#### Synchronization Functions
* [Synchroization Function](https://msdn.microsoft.com/EN-US/library/windows/desktop/ms686360&#40;v=vs.85&#41;.aspx)
	`InterlockedIncrement` `InterlockedIncrement64`

#### Unhandled Exception
* SetUnhandledExceptionFilter : Enables an application to supersede(紧接着...) the top-level exception handler of each thread of a process.
* _set_invalid_parameter_handler : Sets a function to be called when the CRT detects an invalid argument.
* _set_purecall_handler : Sets the handler for a pure virtual function call.
`RtlCaptureContext`

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
* [字符串 Hash 函数](https://www.byvoid.com/zhs/blog/string-hash-compare) `BKDRHash` `APHash` `DJBHash` `JSHash` `RSHash` `SDBMHash` `PJWHash` `ELFHash`

#### Java
* 高性存储
    [高性能队列——Disruptor](https://tech.meituan.com/disruptor.html)
	[并发框架Disruptor译文](http://ifeve.com/disruptor/)

#### VisualStudio
* VC 编译器参数设置
  [显示类内存布局](http://www.zyh1690.org/d1reportsingleclasslayout-msvc%E6%B1%87%E7%BC%96/)
* dllimport、dllexport
  [VS下 dllimport与dllexport作用与区别](http://blog.csdn.net/u010055724/article/details/51538686)

#### BAT 批处理
  [DOS批处理中%~dp0等扩充变量语法详解](http://www.jb51.net/article/97588.htm)

* 系统应用：`shutdown`、`sc`、`reg`。

### 算法
* `局部敏感哈希`

