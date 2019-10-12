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


### Java
测试代码：(Eclipse JEE + JDK 1.8.0_221)
``` java
private void RandomTest()
{
    logger.debug("-----> AttemptRandom.RandomTest() <-----");
    
    int row = 5;
    int col = 5;
    int total = row * col;
    
    for (int i = 0; i < total; i++)
    {
//        System.out.printf("%10s", i);
        double randomNumber = Math.random();
        System.out.printf("%25.18f", randomNumber);
        
        if ((i + 1) % col == 0)
        {
            System.out.println();
        }
    }
    
    System.out.printf("\n\n");
    
    Random randomDouble = new Random(1);
    for (int i = 0; i < total; i++)
    {
        double randomDoubleN = randomDouble.nextDouble();
        System.out.printf("%25.18f", randomDoubleN);
        if ((i + 1) % col == 0)
        {
            System.out.println();
        }
    }
    
    System.out.printf("\n\n");
    
    Random randomDoubleCopy = new Random(1);
    for (int i = 0; i < total; i++)
    {
        double randomDoubleN = randomDoubleCopy.nextDouble();
        System.out.printf("%25.18f", randomDoubleN);
        if ((i + 1) % col == 0)
        {
            System.out.println();
        }
    }
    
    System.out.printf("\n\n");
    
    Random randomInt = new Random(1);
    for (int i = 0; i < total; i++)
    {
        int randomIntN = randomInt.nextInt();
        System.out.printf("%15d", randomIntN);
        if ((i + 1) % col == 0)
        {
            System.out.println();
        }
    }
    
    System.out.printf("\n\n");
    
//    long oldseed = 1L;
//    long oldseed = 1L ^ 25214903917L;
    long oldseed = 25214903916L; // 相当于构造 Random 时初始种子为 1
    long nextseed = 0L;
    for (int i = 0; i < total; i++)
    {
        nextseed = (oldseed * 25214903917L + 11L) & 281474976710655L;
        int randomValue = (int)(nextseed >>> (48 - 32));
        oldseed = nextseed;
        System.out.printf("%15d", randomValue);
        
        if ((i + 1) % col == 0)
        {
            System.out.println();
        }
    }
}
```
输出结果：
![输出结果](\images\Algorithm\RandomNumber_Java_Test.png)
试验总结：
* 采用线性同余算法；
* 种子相同时产生相同的伪随机数序列；
* Random 类默认种子根据时间获得，赋值种子为 1 ，会通过异或运算生成第一个种子。

Java 源码 JDK 1.8.0_221
``` java 
// 十进制 25214903917 [0x5deece66d];
private static final long multiplier = 0x5DEECE66DL;
// 11 [0xb]
private static final long addend = 0xBL;
// 281474976710655 [0xffffffffffff]
private static final long mask = (1L << 48) - 1;

/**
 * Returns the next pseudorandom, uniformly distributed {@code int}
 * value from this random number generator's sequence. The general
 * contract of {@code nextInt} is that one {@code int} value is
 * pseudorandomly generated and returned. All 2<sup>32</sup> possible
 * {@code int} values are produced with (approximately) equal probability.
 *
 * <p>The method {@code nextInt} is implemented by class {@code Random}
 * as if by:
 *  <pre> {@code
 * public int nextInt() {
 *   return next(32);
 * }}</pre>
 *
 * @return the next pseudorandom, uniformly distributed {@code int}
 *         value from this random number generator's sequence
 */
public int nextInt() {
    return next(32);
}

/**
 * Generates the next pseudorandom number. Subclasses should
 * override this, as this is used by all other methods.
 *
 * <p>The general contract of {@code next} is that it returns an
 * {@code int} value and if the argument {@code bits} is between
 * {@code 1} and {@code 32} (inclusive), then that many low-order
 * bits of the returned value will be (approximately) independently
 * chosen bit values, each of which is (approximately) equally
 * likely to be {@code 0} or {@code 1}. The method {@code next} is
 * implemented by class {@code Random} by atomically updating the seed to
 *  <pre>{@code (seed * 0x5DEECE66DL + 0xBL) & ((1L << 48) - 1)}</pre>
 * and returning
 *  <pre>{@code (int)(seed >>> (48 - bits))}.</pre>
 *
 * This is a linear congruential pseudorandom number generator, as
 * defined by D. H. Lehmer and described by Donald E. Knuth in
 * <i>The Art of Computer Programming,</i> Volume 3:
 * <i>Seminumerical Algorithms</i>, section 3.2.1.
 *
 * @param  bits random bits
 * @return the next pseudorandom value from this random number
 *         generator's sequence
 * @since  1.1
 */
protected int next(int bits) {
    long oldseed, nextseed;
    AtomicLong seed = this.seed;
    do {
        oldseed = seed.get();
        nextseed = (oldseed * multiplier + addend) & mask;
    } while (!seed.compareAndSet(oldseed, nextseed));
    return (int)(nextseed >>> (48 - bits));
}
```

