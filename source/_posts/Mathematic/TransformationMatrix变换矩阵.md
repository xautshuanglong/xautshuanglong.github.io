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

## 二维旋转

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

