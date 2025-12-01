# tzset - 设置时区转换信息

## 概要

```c
#include <time.h>

[XSI] extern int daylight;
[XSI] extern long timezone;

[CX] extern char *tzname[2];
[CX] void tzset(void);
```

## 描述

`tzset()` 函数应使用环境变量 `TZ` 的值来设置 `ctime()`、`localtime()`、`mktime()` 和 `strftime()` 函数使用的时间转换信息。如果环境中没有 `TZ`，则应使用实现定义的默认时区信息。

`tzset()` 函数应按如下方式设置外部变量 `tzname`：

```c
tzname[0] = "std";
tzname[1] = "dst";
```

其中 `std` 和 `dst` 的描述如 XBD 第 8 章《环境变量》中所述。

[XSI] 如果所使用的时区永远不应应用夏令时转换，`tzset()` 函数还应将外部变量 `daylight` 设置为 0；否则设置为非零值。外部变量 `timezone` 应设置为协调世界时 (UTC) 与本地标准时间之间的差值，以秒为单位。

如果一个线程在另一个线程正在调用 `tzset()` 或任何被要求或允许像调用 `tzset()` 一样设置时区信息的函数时，直接访问 `tzname`、[XSI] `daylight` 或 `timezone`，则其行为未定义。

## 返回值

`tzset()` 函数不应返回任何值。

## 错误

未定义任何错误。

## 示例

下表给出了示例 `TZ` 变量及其时区差值：

| TZ | timezone |
|----|----------|
| EST5EDT | 5\*60\*60 |
| GMT0 | 0\*60\*60 |
| JST-9 | -9\*60\*60 |
| MET-1MEST | -1\*60\*60 |
| MST7MDT | 7\*60\*60 |
| PST8PDT | 8\*60\*60 |

## 应用用法

由于 `ctime()`、`localtime()`、`mktime()`、`strftime()` 和 `strftime_l()` 函数被要求像调用 `tzset()` 一样设置时区信息，因此在使用这些函数之前不需要显式调用 `tzset()`。但是，可移植应用程序在使用 `localtime_r()` 之前应显式调用 `tzset()`，因为该函数设置时区信息是可选的。

## 原理

无。

## 未来方向

无。

## 参见

- `ctime()`
- `localtime()`
- `mktime()`
- `strftime()`
- XBD 8. 环境变量
- `<time.h>`

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

更正了示例。

### Issue 7

应用了 POSIX.1-2008 Technical Corrigendum 2，XSH/TC2-2008/0377 [880]。

### Issue 8

应用了 Austin Group Defect 1253，将"Daylight Savings"更改为"Daylight Saving"。

应用了 Austin Group Defect 1410，移除了 `ctime_r()` 函数。