---
title: Python 学习笔记
date: 2016-05-12 13:53:27
categories: Python
tags: Python
description: 记录成长的脚印
toc: true
---

>Python 是一门简单易学且功能强大的编程语言：
>拥有高效的高级数据结构；能够用简单而又高效的方式进行面向对象编程。
>Python 优雅的语法和动态类型，再结合它的解释性，使其在大多数平台的许多领域成为编写脚本或开发应用程序的理想语言。

<!--more-->
## Python 模块
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模块是包括 Python 定义和声明的文件。文件名就是模块名加上 .py 后缀。模块名（作为一个字符串）可以由全局变量 __name__ 得到。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;模块可以像函数定义一样包含执行语句，这些语句通常用于初始化模块，它们只在模块第一次导入时执行一次。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;每一个模块拥有自己的私有语义表，因此模块作者可以在模块中使用一些全局变量，不会因为与用户的全局变量冲突而引发错误。
引用形式：`moduleName.itemName`。
注：
    出于性能考虑，每个模块在每个解释器回话中只导入一遍。
	如果修改了模块，需要重启解释权或者用 `reload()` 重新加载，如：`reload(moduleName)`。

### 导入方法
* import moduleName 需要用 `moduleName.itemName` 访问或调用模块中的定义。
* from moduleName import itemName1,itemName2 直接通过 `itemName` 访问或调用模块中定义。
* from moduleName import * 导入模块中的所有定义，除了以下划线 `"_"` 开头的

### 作为脚本来执行
	python moduleName.py <arguments>
模块代码会被执行,就像导入它一样，此时'__name__'被设置为'__main__'。
可以在模块后加入如下代码方便测试：

	if __name__ == "__main__":
		import sys
		someMethod(sys.argv[1])
这段代码只有在模块被作为`main`文件执行时才被调用，通过`import`导入是不会执行的。
### 搜索路径
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;导入`moduleName`时，解释器现在当前目录下搜索名为`moduleName.py`的文件，然后在环境变量`PYTHONPATH`表示的目录列表中搜索，接下来搜索`PATH`路径列表。如果`PYTHONPATH`没有设置，或者文件没有找到，就搜索安装目录。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;实际上，解释器由`sys.path`变量指定的路径搜索模块，该变量初始化时默认包含了输入脚本（或者当前目录），`PYTHONPATH`和安装目录。
_注：由于这些目录中包含运行的脚本，所有脚本不应该和标准模块重名，否则导入模块时Python会尝试把脚本当作模块来加载，通常会引发错误。_
### 已编译的Python文件
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`moduleName.pyc`被视为`moduleName.py`的预编译版本。`moduleName.py`的修改时间被记录在`moduleName.pyc`中，如果两者不匹配，`moduleName.pyc`就被忽略。一旦`moduleName.py`被成功编译，就会尝试生成对应版本的`moduleName.pyc`文件。`moduleName.pyc`是平台无关的，所以`Python`模块可以在不同架构的机器之间共享。
* 以`-O`为参数调用`Python`解释器，生成优化后的代码文件`moduleName.pyo`。
* 以`-OO`为参数调用`Python`解释器，字节码编译器执行完全优化，生成更为紧凑的`moduleName.pyo`文件。

对于同一个模块，可以只有`.pyc`或`.pyo`文件，而没有`.py`文件。可以发布难以逆向的`Python`代码库。
_注：`.pyc`或`.pyo`不会比`.py`文件运行得更快，只是加载的时候更快一些，以命令行形式运行脚本时不会将脚本二进制代码写入`.pyc`或`.pyo`（以模块形式导入即可）。_
### 包
使用`“圆点模块名”`的结构化模块命名空间。`packageName.moduleName`例如：

	A.B 表示名为A的包中名为B的子模块。
用模块来保存不同模块架构可以避免全局变量之间的相互冲突；
用圆点模块名保存不同类库架构可以避免模块之间的命名冲突。
目录中有`__init__.py`文件存在，才能使`Pyhton`视该目录为一个包。
_`__init__.py`可以是一个空文件。_
