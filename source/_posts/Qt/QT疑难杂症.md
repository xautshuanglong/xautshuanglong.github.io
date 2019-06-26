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

1. 连接信号与槽后，不触发槽函数。
   * 确认连接函数返回值是否为真；
   * 类是否继承自 QObject 或其子类，并确保 Q_OBJECT 没有遗漏
   * 信号函数如果含有自定义类型，该类型需要声明为元对象类型，并注册运行时类型。`Q_DECLARE_METATYPE(CustomType)` `qRegisterMetaType<CustomType>("CustomType")`

1. 从 QtCreator 项目导出 VisualStudio 项目，在头文件中加入 `Q_OBJECT` 宏却不执行 moc。
   提示错误 找不到 `qt_metacast` `qt_metacall` 等函数，经确认并没有生成 moc_xxxxx.cpp 文件，及仅替换了 `Q_OBJECT` 宏，却没有执行 moc 编译，所以没有对应的函数实现。
   解决方案是在 QtCreator 中提前加好 `Q_OBJECT` 并执行构建，再次导出 VisualStudio 项目。

1. QObject::moveToThread() 需配合 QObject::thread() 使用，否则可能出现跨线程使用对象的错误。
   移动到主线程： myObject->moveToThread(QApplication::instance()->thread());
   如果 QObject 拥有父类会导致 moveToThread() 失败；

