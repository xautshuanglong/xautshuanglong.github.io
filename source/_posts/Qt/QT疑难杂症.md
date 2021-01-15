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

1. 信号与槽使用直连方式`Qt::DirectConnection`，在槽函数中使用 sender() 无法获取信号发送者，发送者指针为空，为什么为空？
``` bash
QT 帮助文档关于 QObject::sender() 的说明

Returns a pointer to the object that sent the signal, if called in a slot activated by a signal; otherwise it returns nullptr. The pointer is valid only during the execution of the slot that calls this function from this object's thread context.
The pointer returned by this function becomes invalid if the sender is destroyed, or if the slot is disconnected from the sender's signal.
Warning: This function violates the object-oriented principle of modularity. However, getting access to the sender might be useful when many signals are connected to a single slot.
Warning: As mentioned above, the return value of this function is not valid when the slot is called via a Qt::DirectConnection from a thread different from this object's thread. Do not use this function in this type of scenario.
```
    几点规律总结：
    * 直连模式无法获取发送者对象，即使发送者与接收者在同一线程也无法获取发送者对象。（Qt5.15.0）
    * 与 QTcpSocket disconnected 信号相对应的自定义槽函数无法获取发送者对象，进而无法调用 deleteLater()，此时，将 QTcpSocket::disconnected 信号连接到 QTcpSocket::deleteLater，如需自定义槽函数，多连接一个即可。
    * 子类化 QThread 重载 run 函数并在其中执行 exec 函数，此时将子类化的类的实例通过 moveToThread 移动到自身线程中，可以让子类化后的类的槽函数工作在这个子线程中。

1. 备用

