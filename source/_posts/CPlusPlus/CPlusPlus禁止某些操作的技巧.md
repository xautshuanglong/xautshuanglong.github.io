---
title: C++ 禁止某些操作的技巧
date: 2016-12-23 14:31:40
categories: CPP
description: 做专业的程序员
toc: true
tags:
---

> 禁止构造：用于单例
> 禁止拷贝：保持唯一性
> 禁止继承：防止泛化
<!--More-->

## 禁止构造
	深拷贝与浅拷贝的区别
	两个对象能够完全独立 --> 深拷贝
	两个对象存在某种联系 --> 浅拷贝
+ 将构造函数声明为 `private`；
+ 用 `delete` 标记，编译器不会合成默认成员。参考[Defaulted & Deleted](https://xautshuanglong.github.io/2016/12/26/CPlusPlus/CPP11新特性/)

## 禁止拷贝/赋值
+ 将拷贝构造函数 与 operator= 均声明为 `private`；
+ 用 `delete` 标记，编译器不会合成默认成员，参考[Defaulted & Deleted](https://xautshuanglong.github.io/2016/12/26/CPlusPlus/CPP11新特性/)；
+ 不提供方法的实现，防止`friend`。


	空类的默认成员有6个：

``` cpp
class Empty{};

Empty();                          //默认构造函数
~Empty();                         //默认析构函数
Empty(const Empty &);             //默认拷贝构造
Empty & operator=(const Empty &); //默认拷贝赋值运算符
Empty * operator&();              //取地址运算符
const Empty * operator&() const;  //取地址运算符 const
```
	禁止拷贝/赋值示例   (可用于派生屏蔽拷贝构造和赋值操作的子类)
``` cpp
#define DISABLE_COPY(Class) \
Class(const Class &); \
Class &operator=(const Class &)
 
class Widget  
{  
public:  
    int* pi;  
private:  
DISABLE_COPY(Widget);
};
```

## 禁止继承
+ C++11 关键字 `sealed`；
+ C++11 关键字 `final`；
+ 虚继承一个拥有私有构造函数的基类。

``` c
class SealedClass
{
private:
	SealedClass() { };
	friend class SealedClassTest;
};

class SealedClassTest:private virtual SealedClass
{
public:
	SealedClassTest();
	~SealedClassTest();
};

// 企图继承 SealedClassTest 的类（通常不是 SealedClass 的有缘类），
// 在实例化时会因为无法访问 SealedClass 的默认构造函数而实例化失败，达到阻止继承的目的。
```

