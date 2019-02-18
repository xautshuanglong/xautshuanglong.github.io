---
title: TransformationMatrix变换矩阵
date: 2018-11-16 15:02:55
categories: Mathematic
description: 用数学描述世界
feature: /images/Mathematic/TransformMatrix_logo.png
mathjax: true
tags: matrix
toc: true
---

奇妙数学世界

<!-- More -->

[旋转变换](https://blog.csdn.net/csxiaoshui/article/details/65446125)
[旋转矩阵 Wiki](https://zh.wikipedia.org/wiki/%E6%97%8B%E8%BD%AC%E7%9F%A9%E9%98%B5)

## 二维平移
$$
\begin{cases}
x' = x + t_x \\\\
y' = y + t_y \\\\
\end{cases}
$$

$$
\begin {bmatrix}
    x' \\\\
    y' \\\\
    1  \\\\
\end {bmatrix}
=
\begin {bmatrix}
    1 & 0 & t_x \\\\
    0 & 1 & t_y \\\\
    0 & 0 & 1  \\\\
\end {bmatrix}
\cdot
\begin {bmatrix}
    x \\\\
    y \\\\
    1 \\\\
\end {bmatrix}
$$

## 二维缩放
$$
\begin{cases}
x' = x \cdot s_x \\\\
y' = y \cdot s_y \\\\
\end{cases}
$$

$$
\begin {bmatrix}
    x' \\\\
    y' \\\\
    1  \\\\
\end {bmatrix}
=
\begin {bmatrix}
    s_x & 0   & 0 \\\\
    0   & s_y & 0 \\\\
    0   & 0   & 1 \\\\
\end {bmatrix}
\cdot
\begin {bmatrix}
    x \\\\
    y \\\\
    1 \\\\
\end {bmatrix}
$$

## 二维旋转

### 绕原点旋转

$$
\begin {bmatrix}
    x' \\\\
    y' \\\\
\end {bmatrix}
=
\begin {bmatrix}
    cos\theta & -sin\theta \\\\
    sin\theta &  cos\theta \\\\
\end {bmatrix}
\cdot
\begin {bmatrix}
    x \\\\
    y \\\\
\end {bmatrix}
$$

$$
\begin {bmatrix}
    x' \\\\
    y' \\\\
    1  \\\\
\end {bmatrix}
=
\begin {bmatrix}
    cos\theta & -sin\theta & 0 \\\\
    sin\theta &  cos\theta & 0 \\\\
    0         &  0         & 1 \\\\
\end {bmatrix}
\cdot
\begin {bmatrix}
    x \\\\
    y \\\\
    1 \\\\
\end {bmatrix}
$$

### 绕任意点旋转

1. 将旋转点移动到原点处
1. 执行绕原点的旋转
1. 再将旋转点移回原来的位置

$$
\begin{align}
M & = 
\begin {bmatrix}
    1 & 0 & tx \\\\
    0 & 1 & ty \\\\
    0 & 0 & 1  \\\\
\end {bmatrix}
\cdot 
\begin {bmatrix}
    cos\theta & -sin\theta & 0 \\\\
    sin\theta &  cos\theta & 0 \\\\
    0         &  0         & 1 \\\\
\end {bmatrix}
\cdot
\begin {bmatrix}
    1 & 0 & -tx \\\\
    0 & 1 & -ty \\\\
    0 & 0 &  1  \\\\
\end {bmatrix} \\\\
& = 
\begin {bmatrix}
    cos\theta & -sin\theta & (1-cos\theta) \cdot tx + ty \cdot sin\theta \\\\
    sin\theta &  cos\theta & (1-cos\theta) \cdot ty - tx \cdot sin\theta \\\\
    0         &  0         & 1 \\\\
\end {bmatrix}
\end{align}
$$

## 二维复合变换
变换顺序：缩放 -> 旋转 -> 平移。

1. 缩放不改变原点和轴向；
1. 旋转不改变原点，但改变轴向；
1. 平移不改变轴向，但改变原点。

缩放不能再旋转之后，且缩放和旋转不能在平移之后。
https://blog.csdn.net/zsq306650083/article/details/50561857

## 三维旋转

### 绕 x 轴的旋转
$$
\begin {bmatrix}
    x' \\\\
    y' \\\\
    z' \\\\
    1  \\\\
\end {bmatrix}
=
\begin {bmatrix}
    1 & 0         &  0         & 0 \\\\
    0 & cos\theta & -sin\theta & 0 \\\\
    0 & sin\theta &  cos\theta & 0 \\\\
    0 & 0         &  0         & 1 \\\\
\end {bmatrix}
\cdot
\begin {bmatrix}
    x \\\\
    y \\\\
    z \\\\
    1 \\\\
\end {bmatrix}
$$

### 绕 y 轴的旋转
$$
\begin {bmatrix}
    x' \\\\
    y' \\\\
    z' \\\\
    1  \\\\
\end {bmatrix}
=
\begin {bmatrix}
     cos\theta & 0 & sin\theta & 0 \\\\
     0         & 1 & 0         & 0 \\\\
    -sin\theta & 0 & cos\theta & 0 \\\\
     0         & 0 & 0         & 1 \\\\
\end {bmatrix}
\cdot
\begin {bmatrix}
    x \\\\
    y \\\\
    z \\\\
    1 \\\\
\end {bmatrix}
$$

### 绕 z 轴的旋转
$$
\begin {bmatrix}
    x' \\\\
    y' \\\\
    z' \\\\
    1  \\\\
\end {bmatrix}
=
\begin {bmatrix}
    cos\theta & -sin\theta & 0 & 0 \\\\
    sin\theta &  cos\theta & 0 & 0 \\\\
    0         &  0         & 1 & 0 \\\\
    0         &  0         & 0 & 1 \\\\
\end {bmatrix}
\cdot
\begin {bmatrix}
    x \\\\
    y \\\\
    z \\\\
    1 \\\\
\end {bmatrix}
$$

## Mathjax 矩阵语法

$$
\begin {matrix}
    1 & x & x^2 \\\\
    1 & y & y^2 \\\\
    1 & z & z^2 \\\\
\end {matrix}
$$


$$
\begin {bmatrix}
    1 & x & x^2 \\\\
    1 & y & y^2 \\\\
    1 & z & z^2 \\\\
\end {bmatrix}
$$

$$
\begin {Bmatrix}
    1 & x & x^2 \\\\
    1 & y & y^2 \\\\
    1 & z & z^2 \\\\
\end {Bmatrix}
$$


$$
\begin {vmatrix}
    1 & x & x^2 \\\\
    1 & y & y^2 \\\\
    1 & z & z^2 \\\\
\end {vmatrix}
$$


$$
\begin {Vmatrix}
    1 & x & x^2 \\\\
    1 & y & y^2 \\\\
    1 & z & z^2 \\\\
\end {Vmatrix}
$$

$$
\begin {bmatrix}
     1 & a_1 & a_1^2 & \cdots & a_1^n \\\\
     1 & a_2 & a_2^2 & \cdots & a_2^n \\\\
     \vdots  & \vdots& \vdots & \ddots & \vdots \\\\
     1 & a_m & a_m^2 & \cdots & a_m^n
\end {bmatrix}
$$

$$
\left [
    \begin {array} {cc|c}
      1 & 2 & 3 \\\\
      4 & 5 & 6 \\\\
    \end {array}
\right ]
$$

行内小矩阵测试： $\bigl( \begin{smallmatrix} a & b \\\\ c & d \end{smallmatrix} \bigr)$ $\bigl[ \begin{smallmatrix} a & b \\\\ c & d \end{smallmatrix} \bigr]$ $\bigl\\{ \begin{smallmatrix} a & b \\\\ c & d \end{smallmatrix} \bigr\\}$

