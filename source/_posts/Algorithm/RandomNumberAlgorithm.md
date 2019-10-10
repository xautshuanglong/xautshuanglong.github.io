---
title: 随机数算法
date: 2019-10-08 10:51:25
categories: Algorithm
feature: /images/Algorithm/random_logo.jpg
tags: Random
mathjax: true
toc: true
---

## 类型分类
1. 数值概率算法：
1. 拉斯维加斯算法：
1. 蒙特卡罗算法：
1. 舍伍德算法：

<!-- More -->

## 算法说明

### 线性同余
最好最朴素的伪随机数生成算法：易理解、易实现、速度快。

### 马特赛特旋转演示法
[马特赛特旋转演算法](https://www.cnblogs.com/shine-lee/p/9516757.html?utm_source=debugrun&utm_medium=referral)

## 几种平台的实现

### Microsoft Visual C/C++
`C/C++ 语言中的实现` 线性同余法

$$
X\_n = (X\_{n-1} \times a)\ mod\ M
$$

测试代码：(Visual Studio 2015)
``` C
void RandomNumberTest::LinearCongruentialGeneratorTest()
{
    srand(1); // 注释掉该语句，输出内容相同，旨在说明默认种子就是 1。
    for (int i = 0; i < 100; ++i)
    {
        std::cout.width(10);
        std::cout << rand();
        if ((i + 1) % 10 == 0)
        {
            std::cout << std::endl;
        }
    }
    std::cout << std::endl << std::endl;

    srand(1);
    for (int i = 0; i < 100; ++i)
    {
        std::cout.width(10);
        std::cout << rand();
        if ((i + 1) % 10 == 0)
        {
            std::cout << std::endl;
        }
    }
    std::cout << std::endl << std::endl;
}
```
输出结果：
![输出结果](\images\Algorithm\RandomNumber_MicrosoftVisualC_Test.png)
试验总结：
* rand() 函数默认种子为 1；
* 不设置初始种子的话，每次重启应用生成的伪随机数序列相同；（默认种子相同）
* 关于线性同余公式，每次生成新的伪随机数，以上一个伪随机数作为种子（因子）。
  并非直接将伪随机数代入公式中的 $ X\_{n-1} $ (通过代码说明)；

``` C
/***
*void srand(seed) - seed the random number generator
*
*Purpose:
*       Seeds the random number generator with the int given.  Adapted from the
*       BASIC random number generator.
*
*Entry:
*       unsigned seed - seed to seed rand # generator with
*
*Exit:
*       None.
*
*Exceptions:
*
*******************************************************************************/

void __cdecl srand ( unsigned int seed )
{
    // 将种子保存到 _getptd()->_holdrand 中
    _getptd()->_holdrand = (unsigned long)seed;
}

/***
*int rand() - returns a random number
*
*Purpose:
*       returns a pseudo-random number 0 through 32767.
*
*Entry:
*       None.
*
*Exit:
*       Returns a pseudo-random number 0 through 32767.
*
*Exceptions:
*
*******************************************************************************/

int __cdecl rand ( void )
{
    _ptiddata ptd = _getptd();

    // 根据种子和线性同余发生器，产生下一个伪随机数
    return( ((ptd->_holdrand = ptd->_holdrand * 214013L + 2531011L) >> 16) & 0x7fff );
}
```

$$
Seed\_n =
\begin{cases}
    Constant & n = 0 \\\\
    Seed\_{n-1} \times a + c & n \geq 1
\end{cases}
$$

$$
X\_n = Seed\_n >> 16\ \&\ 32767 \text{ (0x7fff) }
$$

    可以这样理解：取伪随机数种子的第 16~30 二进制位，共 15 位（从 0 开始）。

