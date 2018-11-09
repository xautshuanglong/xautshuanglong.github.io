---
title: 几种图片格式介绍
date: 2018-11-05 14:11:48
categories: CommonSense
feature: /images/CommonSense/picture_mark.png
toc: true
---

`BMP`
Bitmap 是 Windows 操作系统中的标准图像文件格式：DDB 设备相关位图；DIB 设备无关位图。
`PNG`
Portable Network Graphics 便携式网络图形是一种无损压缩的图形格式。
`JPEG`
Joint Photographic Expert Group 联合图像专家小组的缩写，是第一个国际图像压缩标准。
`GIF`
Graphics Interchange Format 图像互换格式，是一种基于 LZW 算法的连续色调的无损压缩格式。
`XPM`
X PixMap 是一种基于 ASCII 编码的图像格式，在 X Windows 中应用广泛。

<!-- More -->

## BMP

## PNG

## JPEG

## GIF

## XPM
``` c
/* XPM */
static char *xpmFile[] =
{
    "value string", // width height numcolors chars_per_pixel x_hotspot y_hotspot XPMEXT
    "colors string", // #开头表示十六进制RGB  %开头表示十六进制HSV
    "pixels string",
    "other string"
}
```
Colors String 构成：颜色索引`character`，关键字`key`，颜色值`color`。

### 颜色索引
字符串 NONE，表示该颜色是透明色。

### 颜色关键字
- m: 单色
- s: 符号名称
- g4: 4级灰度
- g: 灰度
- c: 彩色

### 颜色值
- \# 开头的十六进制数表示 RGB 空间颜色值
- % 开头的十六进制数表示 HSV 空间颜色值

Pixels 部分表示实际的像素，采用调色板中定义的索引，由等同于图像高度的行组成。
Extension 部分可以自己定义一些图像附加信息，可单行亦可多行。

example:
``` c
/* XPM */ 
static char * plaid[] =
{
    /* plaid pixmap */
    /* width height ncolors chars_per_pixel */
    "22 22 4 2 0 0 XPMEXT",
    
　　/* colors */
    "  c red m white s light_color",
    "Y c green m black s ines_in_mix",
    "+ c yellow m white s lines_in_dark ",
    "x m black s dark_color ",

　  /* pixels */
　  "x x x x x x x x x x x x + x x x x x ",
    " x x x x x x x x x x x x x x x x ",
    "x x x x x x x x x x x x + x x x x x ",
    " x x x x x x x x x x x x x x x x ",
    "x x x x x x x x x x x x + x x x x x ",
    "Y Y Y Y Y x Y Y Y Y Y + x + x + x + x + x + ",
    "x x x x x x x x x x x x + x x x x x ",
    " x x x x x x x x x x x x x x x x ",
    "x x x x x x x x x x x x + x x x x x ",
    " x x x x x x x x x x x x x x x x ",
    "x x x x x x x x x x x x + x x x x x ",
    " x x x x Y x x x ",
    " x x x Y x x ",
    " x x x x Y x x x ",
    " x x x Y x x ",
    " x x x x Y x x x ",
    "x x x x x x x x x x x x x x x x x x x x x x ",
    " x x x x Y x x x ",
    " x x x Y x x ",
    " x x x x Y x x x ",
    " x x x Y x x ",
    " x x x x Y x x x ",
    "XPMEXT ext1 data1",
    "XPMEXT ext2",
    "data2_1",
    "data2_2", 
    "XPMEXT ext3",
    "data3",
    "XPMEXT",
    "data4",
    "XPMENDEXT"
}; 
```

