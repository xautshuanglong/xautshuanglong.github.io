---
title: SQLite3 学习笔记
date: 2020-01-14 09:27:39
categories: Database
description: SQLite 命令行用法，Sqlcipher 数据库加解密。
feature: /images/Libraries/SQLite_logo.jpg
tags: sqlite
toc: true
---

SQLite：是一个进程内的库，实现了自给自足的、无服务器的、零配置的、事务性的 SQL 数据库引擎。它是一个零配置的数据库，这意味着与其他数据库一样，您不需要在系统中配置。

SQLCipher is an SQLite extension that provides 256 bit AES encryption of database files.

[SQL Cipher](https://www.zetetic.net/sqlcipher/)
[Open Source Library](https://github.com/sqlcipher/sqlcipher)
[Open Source Android](https://github.com/sqlcipher/android-database-sqlcipher)


<!-- More -->

## SQLite 常用命令

## SQLCipher 编译

### 编译环境

开发工具：`Visual Studio 2015`
辅助工具：[ActiveTCL](https://www.activestate.com/products/tcl/) `SourceInsight`
相关依赖：`OpenSSL` `ZLib`

### 编译报错

1. 无法识别标识符 `CRITICAL_SECTION`

    {% label 问题描述 danger %}
    sqlite3.c(30924): error C2061: 语法错误: 标识符“CRITICAL_SECTION”
    {% label 问题根源 primary %}
    CRITICAL_SECTION 在 winnt.h 中有定义，是平台而定。
    包含 windows.h 可正常识别，SQLITE_HAS_CODEC 导致 windows.h 没有被包含到源文件。
    {% label 解决方案 success %}
    增加 -DSQLITE_HAS_CODEC 编译参数，参考自 [issue 206](https://github.com/sqlcipher/sqlcipher/issues/206)

1. 缺少 OpenSSL 依赖

    {% label 问题描述 danger %}
    sqlite3.c(23821): fatal error C1083: 无法打开包括文件: “openssl/rand.h”: No such file or directory
    {% label 问题根源 primary %}
    加解密功能依赖于 OpenSSL 库，需要下载或编译并正确配置依赖库路径。
    {% label 解决方案 success %}
    模仿 TCL、ZLIB 库配置 OpenSSL 依赖库。

1. 缺少 TCL 依赖

    {% label 问题描述 danger %}
    测试程序编译，找不到 TCL 依赖库
    {% label 问题根源 primary %}
    编译前需要 TCL 脚本生成相应源码，以及对中间生成对象进行重组。
    {% label 解决方案 success %}
    配置 TCL 依赖库，安装 ActiveTCL 的位置。

1. 链接不到 OpenSSL

    {% label 问题描述 danger %}
    部分exe, 如 sqldiff.exe 链接失败，找不到 OpenSSL 中的函数。
    {% label 问题根源 primary %}
    这部分 EXE 链接时，没有依赖 OpenSSL 相关 *.lib。
    {% label 解决方案 success %}
    OpenSSL 为新增依赖库，需要配置依赖它的所有可执行文件编译时能链接库该库。

1. 替换 Navicat SQLite 插件报错

    {% label 问题描述 danger %}
    用 sqlcipher 编译出来的 sqlite3.dll 替换 Navicat Premium 12.0.28 中的 sqlite3.dll 启动时报错：
    SQLite function sqlite3_user_authenticate cannot linked. 查看 sqlite3.dll 导出函数的确没有 sqlite3_user_authenticate 函数。
    {% label 问题根源 primary %}
    没有开启用户授权功能模块的控制宏。
    {% label 解决方案 success %}
    编译参数添加 -DSQLITE_USER_AUTHENTICATION 并且将 userauth.c 加入到编源文件列表。

1. Navicat 启动提示缺失部分函数

    {% label 问题描述 danger %}
    用 sqlcipher 编译出来的 sqlite3.dll 替换 Navicat Premium 12.0.28 中的 sqlite3.dll 启动时报错：
    SQLite function sqlite3_libversion cannot linked.
    查看 sqlite3.dll 导出函数的确没有 sqlite3_libversion 函数，但是 libsqlite3.lib 中有 sqlite3_libversion 而 sqlite3.def 没有该函数。
    手动补全 sqlite3.def 并单独执行如下命令：
        link.exe /DEBUG   /NOLOGO /MACHINE:X64  /LIBPATH:..\sqlcipher-4.2.0\..\OpenSSL_x64\Release /DLL /DEF:sqlite3.def /OUT:sqlite3.dll sqlite3.lo sqlite3res.lo  libeay32.lib ssleay32.lib  userauth.lo
    可正常导出且 Navicat 不报错。
    {% label 问题根源 primary %}
    sqlite3.dll 是由静态库 libsqlite3.lib 生成，命令行参数中通过 sqlite3.def 指定导出函数。
    因为 sqlite3.def 没有指定相应函数名，所以 sqlite3.dll 中就没有相应导出函数。
    对比不同方式下生成的 libsqlite3.lib 中的函数名，发现:不加入 userauth.lo 时，函数名为 1 sqlite3_xxxx;加入 userauth.lo 时，函数名为 2 sqlite3_xxxx。
    再看 sqlite3.def 的生成规则，其中正则表达式以 1？（0/1个1）开头，忽略了以 2 开头的函数名。
    {% label 解决方案 success %}
    修改正则表达式为以 2？ 开头的函数：
        dumpbin /all libsqlite3.lib | $(TCLSH_CMD) $(TOP)\tool\replace.tcl include "^\s+[12]? _?(sqlite3(?:session|changeset|changegroup|rebaser)?_[^@]*)(?:@\d+)?$$" \1 | sort >> sqlite3.def


## 参考网址

[SQLite3 SqlCipher windows下的编](https://www.zetetic.net/sqlcipher/)
[Github issues CRITICAL_SECTION](https://github.com/sqlcipher/sqlcipher/issues/206)

