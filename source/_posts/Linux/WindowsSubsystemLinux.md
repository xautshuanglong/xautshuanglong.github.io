---
title: WSL Windows子系统安装Linux
date: 2020-03-04 11:20:01
categories: Linux
description: Windows Subsystem Linux
feature: /images/Linux/WindowsSubsystemLinux_logo.png
tags: wsl
toc: true
---

<!-- More -->

## 配置说明
1. 控制台字体设置
   wsl 控制台窗口设置字体为 consolas，但是通过 vim 看源码时，字体又变回原默认字体，奇丑无比。
   修改注册表：`\HKEY_CURRENT_USER\Console\C:_Program Files_WindowsApps_CanonicalGroupLimited.Ubuntu18.04onWindows_1804.2018.817.0_x64__79rhkp1fndgsc_ubuntu1804.exe`
   添加：`CodePage` 子项，数据类型`DWORD`，值`0x1b5`
参考[WSL中字体意外变化的解决方法](https://blog.csdn.net/MobiuX/article/details/82194028)

