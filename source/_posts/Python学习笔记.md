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
## Python 包
使用`“圆点模块名”`的结构化模块命名空间。`packageName.moduleName`
例如：
`A.B`表示名为A的包中名为B的子模块。
用模块来保存不同模块架构可以避免全局变量之间的相互冲突；
用圆点模块名保存不同类库架构可以避免模块之间的命名冲突。
目录中有`__init__.py`文件存在，才能使`Pyhton`视该目录为一个包。
_注：`__init__.py`可以是一个空文件，也可以包含初始化代码，或者设置`__all__`变量。_
### 导入形式

    import package.item.subitem
    中间的子项必须是包，最后的子项可以是包或模块，但不能是前面子项中定义的包、函数或变量。
    package.item.subitem.definition 必须通过完整的名称来引用。

    from package import item
    item 可以是包中的一个子模块或一个子包,也可以是包中的定义，如：函数、类、变量。
    item 可以在没有包前缀的情况下引用。

    from package import *
    如果 __init__.py 定义了 __all__，就会按照列表给出的模块名进行导入。

    from Package.module import *
    可以直接引用包中的所有定义。
## Python 输入输出
### 输出形式
`str()`将值转化为适于人阅读的形式；
`repr()`将值转化为适于解释器读取的形式。
若没有适于人阅读的解释形式的话，`str()`会返回与`repr()`等同的值。
如：数值、链表、字典等，两者有统一的解读形式；字符串和浮点数则有着各自独特的解读方式。

	s = 'Hello,world.'
	s1 = str(s)  # s1 = 'Hello,world.'
	s2 = repr(s) # s2 = "'Hello,world.'"
	print s1,s2  # Hello,world. 'Hello,world.'
	s = 'Hello,\tworld.'
	s1 = str(s)  # s1 = 'Hello,\tworld.'
	s2 = repr(s) # s2 = "'Hello,\\tworld.'"
	print s1,s2  # Hello,   world. 'Hello,\tworld.'
	s1 = str(1.0/7.0)  # s1 = 0.142857142857
	s2 = repr(1.0/7.0) # s2 = 0.142857142857142857
打印平方、立方表：

	for x in range(1,6):
		print  repr(x).rjust(2),repr(x*x).rjust(3),repr(x*x*x).rjust(4)
	
	1   1    1
	2   4    8
	3   9   27
	4  16   64
	5  25  125
	
	for x in range(1,6):
		print '{0:2d} {1:3d} {2:4d}'.format(x,x*x,x*x*x)
	
	1   1    1
	2   4    8
	3   9   27
	4  16   64
	5  25  125
_注：字符串对齐对齐方式：ljust、center、rjust。  
它们把字符串输出到新的字符串序列，不改变原字符串，并通过向左侧、右侧、左右两侧填充空格使其对齐，字符串较长时不会自动截断。  
(如果需要截断，可以使用切割操作，如：`x.ljust(n)[:n]`)_
`zfill()`用于向字符串左侧填充`0`，它可以正确理解正负号。

    print '12'.zfill(5)
    print '-3.14'.zfill(7)

    00012
    -003.14

### 格式化输出
* 旧格式 `%`

|格式化符号|说明|
|:-----:|:-----:|
| %c | 转换成字符（ASCII码值，或长度为1的字符串。） |
| %r | 优先使用`repr()`函数进行字符串转换 |
| %s | 优先使用`str()`函数进行字符串转换 |
| %b | 转换成二进制整数 |
| %d &#124; %i | 转换成有符号十进制整数 |
| %u | 转换成无符号十进制整数 |
| %o | 转换成无符号八进制整数 |
| %x &#124; %X | 转换成无符号十六进制整数（x &#124; X,代表大小写） |
| %e &#124; %E | 转换成科学计数法（e &#124; E控制输出e &#124; E） |
| %f &#124; %F | 转换成浮点数（小数部分自然截断） |
| %g &#124; %G | %e和%f &#124; %E和%F的简写 |
| %% | 输出 % |
<br>

| 辅助符号 | 说明 |
|:-----:|:-----:|
| * | 定义宽度或者小数点精度 |
| - | 用作左对齐 |
| + | 在正数前面显示(+) |
| # | 在八进制数前面显示零(0)，十六进制前面显示“0x”或“0X” |
| 0 | 显示的数字前面填充“0”而不是默认的空格 |
| (var) | 映射变量（通常用来处理字段类型的参数） |
| m.n | m是显示的最小宽度，n是小数点后的位数（如果可用的话） |
`%[(name)][flags][width].[precision]typecode`
`(name)`为命名
`flags`可以有：`+`，`-`，`' '`，`0`。
`width`表示显示宽度
`precision`表示小数点后的精度

	num = 100
	print "%d to hex is %x" %(num, num)
	print "%d to hex is %X" %(num, num)
	print "%d to hex is %#x" %(num, num)
	print "%d to hex is %#X" %(num, num) 
	
	f = 3.1415926
	print "value of f is: %.4f" % f
	
	lists = [{"name":"name1", "age":27}, {"name":"name2", "age":28}, {"name":"name3", "age":26}]
	print "name: %10s, age: %10d" % (lists[0]["name"], lists[0]["age"])
	print "name: %-10s, age: %-10d" % (lists[1]["name"], lists[1]["age"])
	print "name: %*s, age: %0*d" % (10, lists[2]["name"], 10, lists[2]["age"])
	
	for item in lists:
	    print "%(name)s is %(age)d years old" % item

    输出结果：
	100 to hex is 64
	100 to hex is 64
	100 to hex is 0x64
	100 to hex is 0X64
	value of f is: 3.1416
	name:      name1, age:         27
	name: name2     , age: 28
	name:      name3, age: 0000000026
	name1 is 27 years old
	name2 is 28 years old
	name3 is 26 years old

* 新格式 `format`
在`format()`函数中，使用`{}`符号充当格式化操作符。


    table = {'key1':111,'key2':222,'key3':333}
    print('key1:{key1:d},key2:{key2:#d},key3:{key3:#x}'.format(**table))
    # 位置参数
    print "{0} is {1} years old".format("Wilber", 28)
    print "{} is {} years old".format("Wilber", 28)
    print "Hi, {1}! {1} is {0} years old".format(28,"Wilber")

    # 关键字参数
    print "{name} is {age} years old".format(name = "Wilber", age = 28)

    # 下标参数
    li = ["Wilber", 28]
    print "{0[0]} is {0[1]} years old".format(li)

    # 填充与对齐
    # ^、<、>分别是居中、左对齐、右对齐，后面带宽度
    # :号后面带填充的字符，只能是一个字符，不指定的话默认是用空格填充
    print '{:>8}'.format('3.14')
    print '{:<8}'.format('3.14')
    print '{:^8}'.format('3.14')
    print '{:0>8}'.format('3.14')
    print '{:a>8}'.format('3.14')

    # 浮点数精度
    print '{:.4f}'.format(3.1415926)
    print '{:0>10.4f}'.format(3.1415926)

    # 进制
    # b、d、o、x分别是二进制、十进制、八进制、十六进制
    print '{:b}'.format(11)
    print '{:d}'.format(11)
    print '{:o}'.format(11)
    print '{:x}'.format(11)
    print '{:#x}'.format(11)
    print '{:#X}'.format(11)

    # 千位分隔符
    print '{:,}'.format(1570000000)

    输出结果：
	key1:111,key2:222,key3:0x14d
	Wilber is 28 years old
	Wilber is 28 years old
	Hi, Wilber! Wilber is 28 years old
	Wilber is 28 years old
	Wilber is 28 years old
	    3.14
	3.14
	  3.14
	00003.14
	aaaa3.14
	3.1416
	00003.1416
	1011
	11
	13
	b
	0xb
	0XB
	1,570,000,000

_注：`print`会自动在行末尾加上回车，如果不需要换行，只需在末尾添加一个逗号`,`即可。_

## Python 面向对象(类)
### 类 概述
`Python`的类机制通过最小的新语法和语义在语言中实现了类。它是`C++`或者`Modula-3`语言中类机制的混合。就像模块一样，`Python`的类并没有在用户和定义之间设立绝对的屏障，而是依赖于用户不去“强行闯入定义”的优雅。
类的大多数重要特性都被完整的保留下来：
* 类继承机制允许多重继承
* 派生类可以覆盖(override)基类中的任何方法或类
* 可以使用相同的方法名称调用基类的方法
* 对象可以包含任意数量的私有数据。

用`C++`术语来讲：
* 所有的类成员(包括数据成员)都是公有( public )的
* 所有的成员函数都是虚( virtual )的

用`Modula-3`的术语来讲：
* 成员方法中没有简便的方式引用对象的成员：方法函数在定义时需要以引用的对象做为第一个参数，调用时则会隐式引用对象。

与`C++`和`Modula-3`不同：
* 大多数带有特殊语法的内置操作符(算法运算符、下标等)都可以针对类的需要重新定义。

### 作用域 和 命名空间
`命名空间`是从命名到对象的映射。当前(`Python 2.7`)命名空间主要是通过`Python`字典实现的。不同命名空间中的命名没有任何联系。模块的属性和模块中的全局命名有直接的映射关系：它们共享同一命名空间！

不同的命名空间在不同的时刻创建，有不同的生存期：
* 包含内置命名的命名空间在`Python`解释器启动时创建，会一直保留，不被删除。
* 模块的全局命名空间在模块定义被读入时创建，通常，模块命名空间也会一直保存到解释器退出。
* 由解释器在最高层调用执行的语句，不管它是从脚本文件中读入还是来自交互式输入，都是`__main__`模块的一部分，所以它们也拥有自己的命名空间(内置命名也同样被包含在一个模块中，它被称作`__builtin__`)。

当函数被调用时，就会为它创建一个局部命名空间，并且在函数返回或抛出一个并没有在函数内部处理的异常时删除。每个递归调用都有自己的局部命名空间。

尽管作用域是静态定义，在使用时他们都是动态的。每次执行时，至少有三个命名空间可以直接访问的作用域嵌套在一起：
* 包含局部命名的作用域在最里面，首先被搜索；其次搜索的是中层的作用域，这里包含了同级的函数；最后搜索最外面的作用域，它包含内置命名。
* 首先搜索最内层的作用域，它包含局部命名任意函数包含的作用域，是内层嵌套作用域搜索起点，包含非局部，但是也非全局的命名。
* 接下来的作用域包含当前模块的全局命名。
* 最外层的作用域(最后搜索)是包含内置命名的命名空间。

通常，局部作用域引用当前函数的命名。在函数之外，局部作用域与全局作用域引用同一命名空间：模块命名空间。类定义也是局部作用域中的另一个命名空间。

作用域决定于源程序的意义：定义于某模块中函数的全局作用域是该模块的命名空间，而不是改函数的别名被定义或调用的位置；命名的实际搜索过程是动态的，在运行时确定。(`Python 2.7`)

`Python`的一个特别之处：如果没有使用`global`语法，其赋值操作总是在最里层的作用域。赋值不会复制数据，只是将命名绑定到对象。删除也是如此：`del x`只是从局部作用域的命名空间中删除命名`x`。事实上，所有引入新命名的操作都作用于局部作用域。特别是`import`语句和函数定将模块名或函数绑定于局部作用域(可以使用`global`语句将变量引入到全局作用域)。

### 类 定义语法
类定义最简单的形式如下：

	class ClassName:
		<statement-1>
		<statement-2>
		...
		<statement-N>

类的定义就像函数定义(def 语句)，要执行的时候才能生效，可以把它放到 if 分支，或者函数内部。
