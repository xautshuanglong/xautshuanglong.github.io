---
title: Python 小技巧
date: 2016-05-06 17:10:46
categories: Python
tags: Python
description: 小技巧
toc: true
---

## Python 中的小技巧
### 1. 循环技巧(Looping Techniques)
* #### 在字典中循环
关键字和对应的值可以用`iteritems()`方法同时解读出来。
		dict = {'key0':'value0','key1':'value1','key2':'value2'}
		for key,value in dict.iteritems():
			print key,value
* #### 在序列中循环
索引位置和对应的值可以用`enumerate()`方法同时得到。
		list = ['value0','value1','value2']
		for index,value in enumerate(list):
			print index,value
* #### 同时循环多个序列
同时循环两个或更多的序列，可以使用`zip()`整体打包。
		questions = ['name','quest','favorite color']
		answer = ['eric','the holy grail','blue']
		for q,a in zip(questions,answer):
			print 'What is your {0}? It is {1}.'.format(q,a)
* #### 逆向循环序列
需要逆向循环序列的话，先正向定位序列，然后调用`reversed()`函数。
		for i in reversed(xrange(1,10,2)):
			print i
* #### 排序后循环
`sorted()`函数，不改动原序列，生成一个新的已排序的序列。
		list = ['value2','value1','value0']
		for value in sorted(set(list)):
			print value
<!--more-->

### 2. `range()`和`xrange()`的区别
`range()`返回的是一个列表`List`,而`xrange()`返回一个对象`Object`。
除非需要一个`List`【数据多时，生成`list`费时】，否则，选择`xrange()`,效率高【仅在调用时返回所需数字】。
### 3. `dir()`函数
内置函数`dir()`用于按模块名搜索模块定义，返回一个字符串类型的存储列表。

	import Fibo
	print dir('Fibo')
	['__builtins__', '__doc__', '__file__', '__name__', '__package__', 'fibonacci']
### 4. `str()`和`repr()`
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
	print s1,s2  # Hello,	world. 'Hello,\tworld.'

	s1 = str(1.0/7.0)  # s1 = 0.142857142857
	s2 = repr(1.0/7.0) # s2 = 0.142857142857142857
### 5. 其它技巧
