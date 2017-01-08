---
title: 计算机中的时间
date: 2017-01-08 13:00:15
categories: CommonSense
description: 做专业的程序员
tags: Time
toc: true
---

## 计算机计时原理
`GMT`时间(`Greenwich Mean Time`)，即`格林威治标准时间`，正午太阳横穿格林威治子午线的时间。
`UTC`时间(`Universal Time Coordinated`)，即`世界调整时间`，由原子钟计时，根据`GMT`校准后得到的时间。

自1924年2月5日开始，格林威治天文台每隔一小时会向全世界发放调时信息。

`原子钟`是一种时钟，以原子共振频率标准来计算及保持时间的准确【电子转变能级时释放的精确微波信号】。原子钟是世界上已知最准确的时间测量和频率标准，也是国际时间和频率转换的标准。

<!-- More -->

## FILETIME与UNIX时间转换
不同操作系统的计时起点：
* Linux/Unix&nbsp;&nbsp;1970-1-1 00:00:00 【考虑时区，北京时间：1970-1-1 08:00:00】
* Windows&nbsp;&nbsp;&nbsp;&nbsp;1601-1-1 00:00:00 【11644473600LL seconds before Linux/Unix epoch】
```c
typedef struct _FILETIME
{
	DWORD dwLowDateTime;	// low 32 bits
	DWORD dwHighDateTime;	// high 32 bits
}FILETIME, *PFILETIME, *LPFILETIME;

typedef union _ULARGE_INTEGER
{
	struct
	{
		DWORD LowPart;
		DWORD HighPart;
	};
	struct
	{
		DWORD LowPart;
		DWORD HighPart;
	}u;
	ULONGLONG QuadPart;
}ULARGE_INTEGER, *PULARGE_INTEGER;

FILETIME fileTime; // 时钟嘀嗒，100ns，开始于 1601-01-01 00:00:00

// 移位操作
unsigned long long ulongTime = fileTime.dwHighDateTime;
ulongTime = ulongTime << 32 | fileTime.dwLowDateTime;

// 官方推荐
ULARGE_INTEGER ulargeTime;
ulargeTime.HighPart = fileTime.dwHighDateTime;
ulargeTime.LowPart = fileTime.dwLowDateTime;

// 转换
#define WINDOWS_TICK 10000000 // 100ns=10^-7s
// 1601-01-01 00:00:00 与 1970-01-01 00:00:00 间隔的秒数
#define SEC_TO_UNIX_EPOCH 11644473600LL 
time_t unixTime1 = ulongTime / WINDOWS_TICK - SEC_TO_UNIX_EPOCH;
time_t unixTime2 = ulargeTime.QuadPart / WINDOWS_TICK - SEC_TO_UNIX_EPOCH;
```

## 获取当前系统时间
* tm
* time_t
* SYSTEMTIME

## 精确计算时间差
* QueryPerformanceFrequency
* QueryPerformanceCounter

