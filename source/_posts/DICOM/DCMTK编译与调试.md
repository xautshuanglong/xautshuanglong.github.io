---
title: DCMTK 编译与调试
date: 2018-12-25 14:33:27
categories: DICOM
feature: /images/DICOM/DICOM_logo.png
tags:
toc: true
---

CMake 生成 VS 解决方案使用绝对路径留下的痛

<!-- More -->

## CMake DCMTK
* 指定安装路径，打包头文件及依赖库；
* 支持库：xml、png、tiff、openssl、iconv、zlib；
* 绝对路径问题。

## 疑难杂症
1. VLD 检查出 log4cplus 内存泄漏
   现象描述：单线模式（主线程）下通过 DCMTK 中的 log4cplus 输出日志没有检测出内存泄漏，但在多线程环境下，就会出现内存泄漏，大致如下：

``` C
---------- Block 936 at 0x00000000002672F0: 522 bytes ----------
  CRT Alloc ID: 381092
  Leak Hash: 0xE89898E0, Count: 1, Total 522 bytes
  Call Stack (TID 5948):
    ucrtbased.dll!malloc()
    operator new[]()
    OFVector<char>::reserve() + 0xA bytes
    OFVector<char>::resize()
    OFVector<char>::OFVector<char>()
    dcmtk::log4cplus::helpers::snprintf_buf::snprintf_buf()
    dcmtk::log4cplus::internal::per_thread_data::per_thread_data() + 0x13 bytes
    dcmtk::log4cplus::internal::alloc_ptd() + 0x21 bytes
    dcmtk::log4cplus::internal::get_ptd() + 0x5 bytes
    dcmtk::log4cplus::detail::get_macro_body_oss() + 0x7 bytes
    DicomSCU::InitNetwork() + 0x36 bytes
    DicomSCU::PerformEcho() + 0x15 bytes
    DicomSCU::ExcuteOperation() + 0x12 bytes
    DicomExecutor::run() + 0x1C bytes
    Qt5Cored.dll!QBitArray::size() + 0x99D40 bytes
    Qt5Cored.dll!QBitArray::size() + 0xA2BBB bytes
    kernel32.dll!BaseThreadInitThunk() + 0xD bytes
    ntdll.dll!RtlUserThreadStart() + 0x1D bytes
```
   解决方案：log4cplus 采用线程局部变量存储多线程中的日志记录，局部变量只在线程中起作用，所以在线程退出时需要主动清理线程局部变量。`dcmtk::log4cplus::threadCleanup()`

