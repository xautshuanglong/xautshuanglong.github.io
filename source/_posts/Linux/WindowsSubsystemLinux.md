---
title: WSL Windows子系统安装Linux
date: 2020-03-04 11:20:01
categories: Linux
description: Windows Subsystem Linux
feature: /images/Linux/WindowsSubsystemLinux_logo.png
tags: wsl
toc: true
---

**WSL 学习路上：**
* 配置说明
  `访问主机代理软件`
* 问题解决
  `文本显示`、`局域网 SSH`、`文件系统资源共享`、`版本升级 WSL2`

<!-- More -->

## 配置说明

代理那些事

[//]: # (借助 v2ray 下载墙外代码，端口号根据代理软件而定)

git config core.longpaths true
git config http.proxy http://127.0.0.1:10809
git config https.proxy https://127.0.0.1:10809

CMD 管理员权限系统环境

netsh winhttp show proxy
netsh winhttp set proxy
netsh winhttp reset proxy

CMD 运行环境

set http_proxy=http://127.0.0.1:10809
set https_proxy=https://127.0.0.1:10809
set socks5_proxy=socks5://127.0.0.1:10808

特殊情况下无法 TLS 协议 https://xxx.xxx.xxx 下载资源，尝试将 https 替换为 socks5
set https_proxy=socks5://127.0.0.1:10808


## 问题解决

[//]: # (控制台字体设置)
{% alert success %}
控制台字体设置
{% endalert %}
{% label 问题描述 danger %}
wsl 控制台窗口设置字体为 consolas，但是通过 vim 看源码时，字体又变回原默认字体，奇丑无比。
{% label 解决方案 success %}
修改注册表：`\HKEY_CURRENT_USER\Console\C:_Program Files_WindowsApps_CanonicalGroupLimited.Ubuntu18.04onWindows_1804.2018.817.0_x64__79rhkp1fndgsc_ubuntu1804.exe`
添加：`CodePage` 子项，数据类型`DWORD`，值`0x1b5`
参考网址：[WSL中字体意外变化的解决方法](https://blog.csdn.net/MobiuX/article/details/82194028)

[//]: # (go-delve 调试时不显示交互提示)
{% alert success %}
WSL 内核升级到 WSL2
{% endalert %}
{% label 问题描述 danger %}
通过 go-delve 调试 golang 代码，无法显示交互提示，VSCODE 也无法使用调试功能。
{% label 解决方案 success %}
将 WSL1 升级到 WSL2
参考网址：[wsl2 安装（wsl 升级）](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

[//]: # (无法从其他主机访问本机 WSL)
{% alert success %}
从其他主机通过 SSH 连接本机 WSL
{% endalert %}
{% label 问题描述 danger %}
即使 sshd_config 中配置监听地址为`0.0.0.0`，netstat.exe 查看到的也是`127.0.0.1`，因此默认情况下无法通过其他主机进行连接。
wsl 中 ifconfig eth0 为随机分配地址。
{% label 解决方案 success %}
通过 powershell 开启本地代理，并确认防火墙放行相关端口，实战中对 `4022` 端口添加例外。
``` bash
netsh interface portproxy add v4tov4 listenport=4022 listenaddress=0.0.0.0 connectport=22 connectaddress=127.0.0.1
netsh interface portproxy delete v4tov4 listenport=4022 listenaddress=0.0.0.0
netsh interface portproxy show all
```
参考网址：[Comparing WSL 1 and WSL 2](https://docs.microsoft.com/en-us/windows/wsl/compare-versions)，[比较 WSL 1 和 WSL 2](https://docs.microsoft.com/zh-cn/windows/wsl/compare-versions)

[//]: # (从 WSL 访问主机)
{% alert success %}
从 WSL 访问主机
{% endalert %}
{% label 问题描述 danger %}
无法从 WSL 中访问主机应用（代理软件）
127.0.0.1 不能访问宿主机，通过宿主机内网 IP 也不能访问宿主机。
通过 windows 网络适配器管理可以看到名为 vEthernet(WSL) 的适配器 `Hyper-V Virtrual Ethernet Adapter`，查看属性得知使用了静态 IP 和特定的子网掩码，使用该 IP 可实现从 WSL 访问主机。也可在 WSL 中通过命令行查看：`cat /etc/resolve.conf`。
{% label 解决方案 success %}
找对IP，配置防火墙规则。
参考网址：[比较 WSL 1 和 WSL 2 —— 访问网络应用程序](https://docs.microsoft.com/zh-cn/windows/wsl/compare-versions)

[//]: # (Windows 资源管理器访问 WSL 文件系统)
{% alert success %}
WSL2 使用虚拟硬件磁盘`VHD`来存储 Linux 文件。
{% endalert %}
{% label 问题描述 danger %}
不能从 windows 资源管理器浏览到 wsl 安装目录下的文件系统，出现`ext4.vhdx`。
{% label 解决方案 success %}
WSL2 VHD 使用 ext4 文件系统，支持扩展硬件磁盘的大小。
在 Windows 资源管理器导航栏输入：`\\wsl$\Ubuntu-18.04\home\shuanglong`
也可通过 shell 进入目标文件夹后，调用 Windows 资源管理器：`explorer.exe .`。

    You can access your Linux root file system with Windows apps and tools like File Explorer. Try opening a Linux distribution (like Ubuntu), be sure that you are in the Linux home directory by entering this command: cd ~. Then open your Linux file system in File Explorer by entering (don't forget the period at the end): explorer.exe .

参考网址：[比较 WSL 1 和 WSL 2](https://docs.microsoft.com/zh-cn/windows/wsl/compare-versions)

