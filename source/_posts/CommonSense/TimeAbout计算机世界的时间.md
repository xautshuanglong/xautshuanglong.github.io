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
`CST`时间(`Central Standard Time`)，即`中央标准时间`，可视为`美国`，`澳大利亚`，`古巴`或`中国`的标准时间:

* 美国中部时间：Central Standard Time (USA) UT-6:00
* 澳大利亚中部时间：Central Standar Time (Australia) UT+9:30
* 中国标准时间：Chian Standard Time UT+8:00
* 古巴标准时间：Cuba Standar Time UT -4:00

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
* time_t 时间戳，距离 1900-01-01 00:00:00 的秒数。
```cpp
time_t time(time_t *timer);
timer --> Pointer to the storage location for time.
Return Value --> Return the time as seconds elapsed since midnight, January 1,1970,
or -1 in the case of an error.
```
* tm 精确到秒的时间结构体，需注意各成员的取值范围。
```cpp
struct tm
{
    int tm_sec;     // the number of seconds [0-59]
    int tm_min;     // the number of minutes [0-59]
    int tm_hour;    // the number of hours since midnight [0-23]
    int tm_mday;    // indicate the day of the month [1-31]
    int tm_mon;     // the number of months since January [0-11]
    int tm_year;    // the number of years since 1900 [117 --> 1900+117=2017]
    int tm_wday;    // the number of days since Sundary [0-6]
    int tm_yday;    // the number of days since January 1 [0-365]
    int tm_isdst;   // daylight savings time [TRUE] and normal time [FALSE]
};

tm m_tmCurTime;
time_t m_tCurTime;
time(&m_tCurTime);// 当前时间戳
// 根据当前所在时区(系统设置)转换为信息全面的结构 
localtime_s(&m_tmCurTime, &m_tCurTime);

long timeZoneOffset;
time_t tempTime;
_tzset();
_get_timezone(&timeZoneOffset);
tempTime = m_tCurTime - timeZoneOffset;
gmtime_s(&m_tmCurTime, &tempTimeOB);// 标准时间，需要根据时区自行调整(参见后文 时区)
```
* SYSTEMTIME
``` cpp
typedef struct _SYSTEMTIME
{
    WORD wYear;         // The current year.
    WORD wMonth;        // The current month; January is 1.
    WORD wDayOfWeek;    // The current day of the week; Sunday is 0, Monday is 1, and so on.
    WORD wDay;          // The current day of the month.
    WORD wHour;         // The current hour.
    WORD wMinute;       // The current minute.
    WORD wSecond;       // The current second.
    WORD wMilliseconds; // The current millisecond.
} SYSTEMTIME, *PSYSTEMTIME, *LPSYSTEMTIME;
SYSTEMTIME m_systemTime;
::GetLocalTime(&m_systemTime);
char timeBuffer[64];
sprintf_s(timeBuffer, "%4d-%02d-%02d %02d:%02d:%02d.%03d",
    m_systemTime.wYear, m_systemTime.wMonth, m_systemTime.wDay,
    m_systemTime.wHour, m_systemTime.wMinute, m_systemTime.wSecond, m_systemTime.wMilliseconds);
```

## 时区与时间差
* 时差 UTC = LocalTime + Bias
```cpp
time_t timeTest;
time(&timeTest);

tm gmTime;
tm loTime;
gmtime_s(&gmTime, &timeTest);
time_t gmt = mktime(&gmTime);
localtime_s(&loTime, &timeTest);
time_t lot = mktime(&loTime);
double diff = difftime(gmt, lot);// -28800.000000

long timeZone;
// The _get_timezone function retrieves the difference in seconds
// between UTC and local time as an integer.
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
* 获取时区相关信息
```cpp
// set time environment variables:
// assign values to three global variables:_daylignt,_timezone and _tzname
_tzset();

int dayLightHours;     // 以小时为单位的日光节约时间
long dayLightSeconds;  // 以秒为单位的日光节约时间
long timeZone;         // 秒数 = 格林威治时间 - 本地时间
char timeZoneName[64]; // 时区名称
size_t nameLen;        // 时区名称长度，包括终结符

_get_daylight(&dayLightHours);// daylight saving time offset in hours
_get_dstbias(&dayLightSeconds);// daylight saving time offset in seconds 
int errorCode = _get_timezone(&timeZone);
_get_tzname(&nameLen, timeZoneName, sizeof(timeZoneName), 0);

// This structure specifies information specific to the time zone.
typedef struct _TIME_ZONE_INFORMATION
{
    LONG Bias;
    WCHAR StandardName[32];
    SYSTEMTIME StandardDate;
    LONG StandardBias;
    WCHAR DaylightName[32];
    SYSTEMTIME DaylightDate;
    LONG DaylightBias;
} TIME_ZONE_INFORMATION;
TIME_ZONE_INFORMATION timeZoneInfo;
GetTimeZoneInformation(&timeZoneInfo);
```

## 精确计算时间差
* QueryPerformanceFrequency
```cpp
BOOL QueryPerformanceFrequency(LARGE_INTEGER* lpFrequency);
This function retrieves the frequency of the high-resolution performance counter
if one is provided by the OEM.
lpFrequency --> 
[out] Pointer to a variable that the function sets, in counts per second,
to the current performance-counter frequency.
If the installed hardware does not support a high-resolution performance counter,
the value passed back through this pointer can be zero.
Return Value -->
TRUE indicates that a performance frequency value was successfully filed in.
False indicates failure.
```
* QueryPerformanceCounter
```cpp
BOOL QueryPerformanceCounter(LARGE_INTEGER* lpPerformanceCount);
This function retrieves the current value of the high-resolution performance counter 
if one is provided by the OEM.
lpPerformanceCount --> 
[in] Pointer to a variable that the function sets, in counts, 
to the current performance-counter value.
If the installed hardware does not support a high-resolution performance counter, 
this parameter can be set to zero.
Return Value -->
TRUE indicates that a performance counter value was successfully filled in.
FALSE indicates failure.
```
* GetTickCount
```c
DWORD WINAPI GetTickCount(void);
Retrieves the number of milliseconds that have elapsed since the system was started,
up to 49.7 days.
Return Value -->
The return value is the number of milliseconds that have elapsed since the system was started.
```
* clock
```cpp
clock_t clock( void );
Calculates the wall-clock time used by the calling process.
Return Value -->
The elapsed wall-clock time since the start of the process
(elapsed time in seconds times CLOCKS_PER_SEC).
If the amount of elapsed time is unavailable, the function returns –1, cast as a clock_t.
```

## CPP 标准库时间
 
```cpp
typedef ratio<1, 1000000000000000000000000LL> yocto;  // 有条件的支持
typedef ratio<1, 1000000000000000000000LL> zepto;     // 有条件的支持
typedef ratio<1, 1000000000000000000LL> atto;
typedef ratio<1, 1000000000000000LL> femto;
typedef ratio<1, 1000000000000LL> pico;
typedef ratio<1, 1000000000> nano;
typedef ratio<1, 1000000> micro;
typedef ratio<1, 1000> milli;
typedef ratio<1, 100> centi;
typedef ratio<1, 10> deci;
typedef ratio<10, 1> deca;
typedef ratio<100, 1> hecto;
typedef ratio<1000, 1> kilo;
typedef ratio<1000000, 1> mega;
typedef ratio<1000000000, 1> giga;
typedef ratio<1000000000000LL, 1> tera;
typedef ratio<1000000000000000LL, 1> peta;
typedef ratio<1000000000000000000LL, 1> exa;
typedef ratio<1000000000000000000000LL, 1> zetta;     //有条件的支持
typedef ratio<1000000000000000000000000LL, 1> yotta;  //有条件的支持
```
