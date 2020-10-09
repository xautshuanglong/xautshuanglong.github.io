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

1. WSL 内核升级到 WSL2 （通过 go-delve 调试 golang 代码，无法显示交互提示，VSCODE 也无法使用调试功能）
   参考网址：[wsl2 安装（wsl 升级）](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

1. 从其他主机通过 SSH 连接本机 WSL
   即使 sshd_config 中配置监听地址为`0.0.0.0`，netstat.exe 查看到的也是`127.0.0.1`，因此默认情况下无法通过其他主机进行连接。
   通过 powershell 开启本地代理，并确认防火墙放行相关端口，实战中对 4022 端口添加例外。
``` bash
netsh interface portproxy add v4tov4 listenport=4022 listenaddress=0.0.0.0 connectport=22 connectaddress=127.0.0.1
netsh interface portproxy delete v4tov4 listenport=4022 listenaddress=0.0.0.0
netsh interface portproxy show all
```
   参考网址：[Comparing WSL 1 and WSL 2](https://docs.microsoft.com/en-us/windows/wsl/compare-versions)，[比较 WSL 1 和 WSL 2](https://docs.microsoft.com/zh-cn/windows/wsl/compare-versions)

1. WSL2 使用虚拟硬件磁盘`VHD`来存储 Linux 文件。
   WSL2 VHD 使用 ext4 文件系统，支持扩展硬件磁盘的大小。
   在 Windows 资源管理器导航栏输入：`\\wsl$\Ubuntu-18.04\home\shuanglong`
   也可通过 shell 进入目标文件夹后，调用 Windows 资源管理器：`explorer.exe .`。

        You can access your Linux root file system with Windows apps and tools like File Explorer. Try opening a Linux distribution (like Ubuntu), be sure that you are in the Linux home directory by entering this command: cd ~. Then open your Linux file system in File Explorer by entering (don't forget the period at the end): explorer.exe .

   参考网址：[比较 WSL 1 和 WSL 2](https://docs.microsoft.com/zh-cn/windows/wsl/compare-versions)

