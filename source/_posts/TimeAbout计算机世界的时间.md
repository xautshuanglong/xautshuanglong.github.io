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
```cpp
struct tmi
{
	int tm_sec;	// the number of seconds [0-59]
	int tm_min;	// the number of minutes [0-59]
	int tm_hour;	// the number of hours since midnight [0-23]
	int tm_mday;	// indicate the day of the month [1-31]
	int tm_mon;	// the number of months since January [0-11]
	int tm_year;	// the number of years since 1900 [117 --> 1900+117=2017]
	int tm_wday;	// the number of days since Sundary [0-6]
	int tm_yday;	// the number of days since January 1 [0-365]
	int tm_isdst;	// daylight savings time [TRUE] and normal time [FALSE]
};
```
* time_t
* SYSTEMTIME
``` c
SYSTEMTIME m_systemTime;
::GetLocalTime(&m_systemTime);
char timeBuffer[64];
sprintf_s(timeBuffer, "%4d-%02d-%02d %02d:%02d:%02d.%03d",
	m_systemTime.wYear, m_systemTime.wMonth, m_systemTime.wDay,
	m_systemTime.wHour, m_systemTime.wMinute, m_systemTime.wSecond, m_systemTime.wMilliseconds);
```

### 时区
```cpp
tm gmTime;
tm loTime;
gmtime_s(&gmTime, &timeTest);
time_t gmt = mktime(&gmTime);
localtime_s(&loTime, &timeTest);
time_t lot = mktime(&loTime);
double diff = difftime(gmt, lot);// -28800.000000

long timeZone;
// The _get_timezone function retrieves the difference in seconds between UTC and local time as an integer.
// The default value is 28,800 seconds, for Pacific Standard Time (eight hours behind UTC).
int errorCode = _get_timezone(&timeZone);// -28800(28800)
char timeZoneName[64];
size_t t;
_get_tzname(&t, timeZoneName, sizeof(timeZoneName), 0);// 中国标准时间(太平洋标准时间)
// 通过调整系统时间可知：
// 中国标准时间   = -28800
// 太平洋标准时间 =  28800
// _get_timezone  =  格林威治时间 - 本地时间
```

## 精确计算时间差
* QueryPerformanceFrequency
* QueryPerformanceCounter

