---
title: Python 小技巧
date: 2016-05-06 17:10:46
categories: Python
tags: Python
description: 小技巧
toc: true
---

## Python 中的小技巧
1. 循环技巧(Looping Techniques)
	* 在字典中循环
	关键字和对应的值可以用 iteritems() 方法同时解读出来。
			dict = {'key0':'value0','key1':'value1','key2':'value2'}
			for key,value in dict.iteritems():
				print key,value
	* 在序列中循环
	索引位置和对应的值可以用 enumerate() 方法同时得到。
			list = ['value0','value1','value2']
			for index,value in enumerate(list):
				print index,value
	* 同时循环多个序列
	同时循环两个或更多的序列，可以使用 zip() 整体打包。
			questions = ['name','quest','favorite color']
			answer = ['eric','the holy grail','blue']
			for q,a in zip(questions,answer):
				print 'What is your {0}? It is {1}.'.format(q,a)
	* 逆向循环序列
	需要逆向循环序列的话，先正向定位序列，然后调用 reversed() 函数。
			for i in reversed(xrange(1,10,2)):
				print i
	* 排序后循环
	sorted() 函数，不改动原序列，生成一个新的已排序的序列。
			list = ['value2','value1','value0']
			for value in sorted(set(list)):
				print value
<!--more-->
2. range() 和 xrange() 的区别
	range()返回的是一个列表 list,而xrange()返回一个对象object。
	除非需要一个List【数据多时，生成list费时】，否则，选择xrange(),效率高【仅在调用时返回所需数字】。
3. 其它技巧
