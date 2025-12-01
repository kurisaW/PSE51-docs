# asctime — 将日期时间转换为字符串

## 概要

```c
#include <time.h>

char *asctime(const struct tm *timeptr);
```

## 描述

`asctime()` 函数应将 `timeptr` 指向的结构体中的分解时间（broken-down time）转换为以下形式的字符串：

```
Sun Sep 16 01:03:52 1973\n\0
```

使用等效于以下算法的方式：

```c
char *asctime(const struct tm *timeptr)
{
    static char wday_name[7][3] = {
        "Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"
    };
    static char mon_name[12][3] = {
        "Jan", "Feb", "Mar", "Apr", "May", "Jun",
        "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
    };
    static char result[26];

    sprintf(result, "%.3s %.3s%3d %.2d:%.2d:%.2d %d\n",
        wday_name[timeptr->tm_wday],
        mon_name[timeptr->tm_mon],
        timeptr->tm_mday, timeptr->tm_hour,
        timeptr->tm_min, timeptr->tm_sec,
        1900 + timeptr->tm_year);
    return result;
}
```

如果分解时间的任何成员包含超出其正常范围的值（参见 XBD `<time.h>`），`asctime()` 函数的行为是未定义的。同样，如果计算出的年份超过四位数字或小于年份 1000，行为也是未定义的。

`tm` 结构体在 `<time.h>` 头文件中定义。

`asctime()`、`ctime()`、`gmtime()` 和 `localtime()` 函数应在两个静态对象之一中返回值：一个分解时间结构体和一个 `char` 类型的数组。执行任何返回指向这些对象类型之一的指针的函数可能会覆盖从之前调用其中任何函数所返回的值指向的任何同类型对象中的信息。

`asctime()` 函数不需要是线程安全的；但是，`asctime()` 应避免与除自身、`ctime()`、`gmtime()` 和 `localtime()` 之外的所有函数发生数据竞争。

## 返回值

成功完成时，`asctime()` 应返回指向字符串的指针。如果函数不成功，应返回 NULL。

## 错误

未定义错误。

## 应用程序用法

此函数仅用于与较旧的实现保持兼容性。如果结果字符串过长，它具有未定义行为，因此应避免使用此函数。在未检测到输出字符串长度溢出的实现上，可能会以导致应用程序失败或可能违反系统安全的方式溢出输出缓冲区。此外，此函数不支持本地化的日期时间格式。为避免这些问题，应用程序应使用 `strftime()` 从分解时间生成字符串。

分解时间结构体的值可以通过调用 `gmtime()` 或 `localtime()` 获得。

## 基本原理

标准开发者决定将 `asctime()` 函数标记为过时的（obsolescent），尽管它在 ISO C 标准中，但存在缓冲区溢出的可能性。ISO C 标准也提供了 `strftime()` 函数，可用于避免这些问题。

## 未来方向

此函数可能在将来的版本中被移除，但要在从 ISO C 标准中移除之后才能移除。

## 参见

- `clock()`
- `ctime()`
- `difftime()`
- `futimens()`
- `gmtime()`
- `localtime()`
- `mktime()`
- `strftime()`
- `strptime()`
- `time()`
- XBD `<time.h>`

## 更改历史

### 首次发布于 Issue 1
源自 SVID 的 Issue 1。

### Issue 5
- 先前在 APPLICATION USAGE 部分中的规范性文本移至 DESCRIPTION。
- 为与 POSIX 线程扩展对齐，包含了 `asctime_r()` 函数。
- 在 DESCRIPTION 中添加了说明 `asctime()` 函数不需要是可重入的注释。

### Issue 6
- `asctime_r()` 函数被标记为线程安全函数选项的一部分。
- 超出 ISO C 标准的扩展被标记。
- APPLICATION USAGE 部分更新为包含线程安全函数的注释及其避免可能使用静态数据区域的说明。
- `asctime_r()` 的 DESCRIPTION 更新为描述返回字符串的格式。
- 为与 ISO/IEC 9899:1999 标准对齐，向 `asctime_r()` 原型添加了 `restrict` 关键字。
- 应用 IEEE Std 1003.1-2001/Cor 2-2004, 项目 XSH/TC2/D6/17，在 RETURN VALUE 部分添加了 CX 扩展，要求如果 `asctime()` 函数不成功则返回 NULL。

### Issue 7
- 应用 Austin Group Interpretation 1003.1-2001 #053，将这些函数标记为过时的。
- 应用 Austin Group Interpretation 1003.1-2001 #156。
- `asctime_r()` 函数从线程安全函数选项移至 Base。
- 应用 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0033 [86,429]。

### Issue 8
- 应用 Austin Group Defect 469，阐明 `asctime()` 行为未定义的条件。
- 应用 Austin Group Defect 1302，使此函数与 ISO/IEC 9899:2018 标准对齐。
- 应用 Austin Group Defect 1330，更改 FUTURE DIRECTIONS 部分。
- 应用 Austin Group Defect 1376，从源自 ISO C 标准的某些文本中移除 CX 着色并更新以匹配 ISO C 标准。
- 应用 Austin Group Defect 1410，移除 `asctime_r()` 函数。