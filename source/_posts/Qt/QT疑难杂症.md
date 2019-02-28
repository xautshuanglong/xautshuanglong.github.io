---
title: QT疑难杂症
date: 2019-02-28 15:58:52
feature: /images/Qt/Qt_logo.png
categories: QT
tags: qt
toc: true
---

即使伤痕累累，也要负重前行。

<!-- More -->

## 运行调试
1. 通过 QtCreator 运行时总是 `Crash`，调试时总是报 `the cdb process terminated`。
   请确认直接运行 EXE 是否正常，通常会缺失各种 DLL，除 Qt 依赖模块外是否存在其他依赖的 DLL。
   如果能正常运行（不缺失 DLL 的情况下）通过 QtCreator 也应该能运行。
   缺失 DLL 或 DLL 的 Debug / Release 不匹配都会导致调试异常。其次，要保证 CDB 调试器配置正确。

