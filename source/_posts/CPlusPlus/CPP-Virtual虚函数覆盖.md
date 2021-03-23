---
title: C++ 虚函数覆盖
date: 2021-03-23 16:01:05
categories: CPP
description: 精益求精
tags: override
toc: true
---

* 覆盖失败？
* 如何避免覆盖失败？
* 缺省参数谁说了算？

<!-- More -->

## 虚函数表

## 缺省参数

``` cpp
#include <iostream>

class Base
{
public:
    virtual void display(const std::string &strShow = "I am Base class !")
    {
        std::cout << std::endl;
        std::cout << strShow << std::endl;
        std::cout << "Base display" << std::endl;
    }
    virtual ~Base()
    {
    }
};

class Derive : public Base
{
public:
    virtual void display(const std::string &strShow = "I am Derive class !")
    {
        std::cout << std::endl;
        std::cout << strShow << std::endl;
        std::cout << "Derive display" << std::endl;
    }
    virtual ~Derive()
    {
    }
};

class Third : public Derive
{
public:
    virtual void display(const std::string &strShow = "I am Third class !")
    {
        std::cout << std::endl;
        std::cout << strShow << std::endl;
        std::cout << "Third display" << std::endl;
    }
    virtual ~Third()
    {
    }
};

int main()
{
    Base *pBase = new Third();
    Derive *pDerive = new Third();
    Third *pThird = new Third();
    
    Third *pThirdBase = static_cast<Third *>(pBase);
    Third *pThirdDerive = static_cast<Third *>(pDerive);

    Base *pBaseThird = (Base *)pThird;
    Base *pDeriveThird = (Derive *)pThird;

    pBase->display();
    pDerive->display();
    pThird->display();
    pThirdBase->display();
    pThirdDerive->display();
    pBaseThird->display();
    pDeriveThird->display();
}

=================================================================================

输出结果：
I am Base class !
Third display

I am Derive class !
Third display

I am Third class !
Third display

I am Third class !
Third display

I am Third class !
Third display

I am Base class !
Third display

I am Base class !
Third display
```

*注：* **根据汇编显示，虚函数调用时，压栈的缺省实参与指针类型（非对象实际类型）保持一致。**

