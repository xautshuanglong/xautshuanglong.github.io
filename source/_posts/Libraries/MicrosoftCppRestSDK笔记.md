---
title: MicrosoftCppRestSDK笔记
date: 2019-03-31 10:46:22
categories: Libraries
feature: /images/Libraries/CppRestSDK_logo.png
tags: cpprest
toc: true
---

REST SDK

<!-- More -->

### 注意事项
1. CMake 如何识别 vcpkg 安装的库路径？
   命令行添加参数 `-DCMAKE_TOOLCHAIN_FILE=/path/to/vcpkg/scripts/buildsystems/vcpkg.cmake`
   GUI 点击 “Add Entry”:
   名称为：`CMAKE_TOOLCHAIN_FILE`
   值为：`/path/to/vcpkg/scripts/buildsystems/vcpkg.cmake`
   参考：[Microsoft/cpprestsdk WIKI](https://github.com/Microsoft/cpprestsdk/wiki/How-to-build-for-Windows)

2. vcpkg 安装的依赖库如何指定平台工具集？
   默认情况下 VS2017 会使用 visual studio 2017(v141)，一种可行的修改途径是，在 `{VCPKG_ROOT}\triplets\x64-windows.cmake` 中设置环境变量：
   `set(VCPKG_TARGET_ARCHITECTURE x64)`
   `set(VCPKG_CRT_LINKAGE dynamic)`
   `set(VCPKG_LIBRARY_LINKAGE dynamic)`
   `set(VCPKG_PLATFORM_TOOLSET v140)`
