---
title: Ninja 学习笔记
date: 2021-03-25 13:52:28
categories: Tools
feature: /
tags: ninja
toc: true
---

高效构建工具

<!-- More -->

## MSVC

``` bash

# ninja -t clean -----> 删除生成的目标文件仅 build 指定的目标。
# ninja clean    -----> 显示调用 clean 目标，删除指定的文件。适用于 windows，基于 cmd.exe /c del 删除文件。

ninja_required_version = 1.10.2

CC = cl.exe
cflags = /w
builddir = Debug
standard = /std:c++latest

rule compile_HelloWorld
    command = $CC $standard /EHsc /c $cflags -MD $in /Fo $out
    description = --------> Compiling $in to $out
    depfile = $out.d
    deps = msvc

build TestFunc.obj : compile_HelloWorld HelloWorld/TestFunc.cpp
build HelloWorld.obj : compile_HelloWorld HelloWorld/HelloWorld.cpp

rule link_HelloWorld
   command = $CC $DEFINES $INCLUDES $cflags $in
   description = --------> Linking $in to $out

build HelloWorld.exe : link_HelloWorld HelloWorld.obj TestFunc.obj


rule clean_cmd
    command = cmd.exe /c $
        if exist HelloWorld.lib ( del /S HelloWorld.lib ) & $
        if exist HelloWorld.exp ( del /S HelloWorld.exp )
    description = --------> Deleting generated files

build clean: clean_cmd

default HelloWorld.exe

```

