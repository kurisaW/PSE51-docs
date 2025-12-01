# localtime, localtime_r - 将时间值转换为分解的本地时间

## 概要

```c
#include <time.h>

struct tm *localtime(const time_t *timer);

[CX] struct tm *localtime_r(const time_t *restrict timer,
                           struct tm *restrict result);
```

## 描述

对于 `localtime()`：
[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本 POSIX.1-2024 卷遵循 ISO C 标准。

`localtime()` 函数应将自纪元以来以秒为单位的时间（由 `timer` 指向）转换为分解的时间，表示为本地时间。该函数会校正时区和任何季节性时间调整。[CX] 本地时区信息的设置应如同 `localtime()` 调用 `tzset()` 一样。

作为 `localtime()` 参数使用的自纪元以来的时间（以秒为单位）与 `tm` 结构体（在 `<time.h>` 头文件中定义）之间的关系是：结果应按照自纪元以来的秒数的定义中给出的表达式指定（见 XBD 4.19 自纪元以来的秒数），校正时区和任何季节性时间调整，其中结构体和表达式中的名称相对应。

同样的关系适用于 `localtime_r()`。

`localtime()` 函数不需要是线程安全的；但是，`localtime()` 应避免与除自身、`asctime()`、`ctime()` 和 `gmtime()` 之外的所有函数发生数据竞争。

`asctime()`、`ctime()`、`gmtime()` 和 `localtime()` 函数应在两个静态对象之一中返回值：一个分解的时间结构体和一个 `char` 类型的数组。执行返回指向这些对象类型之一的指针的任何函数可能会覆盖任何先前调用它们中任何一个所返回的值指向的相同类型的任何对象中的信息。

[CX] `localtime_r()` 函数应将自纪元以来以秒为单位的时间（由 `timer` 指向）转换为存储在 `result` 指向的结构体中的分解时间。`localtime_r()` 函数还应返回指向同一结构体的指针。

与 `localtime()` 不同，`localtime_r()` 函数不需要设置 `tzname`。如果 `localtime_r()` 设置了 `tzname`，它还应设置 `daylight` 和 `timezone`。如果 `localtime_r()` 没有设置 `tzname`，它不应设置 `daylight` 也不应设置 `timezone`。如果在 `TZ` 的值随后被修改后访问 `tm` 结构体成员 `tm_zone`，则行为未定义。

## 返回值

成功完成时，`localtime()` 函数应返回指向分解时间结构体的指针。如果检测到错误，`localtime()` 应返回空指针 [CX] 并设置 `errno` 以指示错误。

成功完成时，`localtime_r()` 应返回指向参数 `result` 指向的结构体的指针。如果检测到错误，`localtime_r()` 应返回空指针并设置 `errno` 以指示错误。

## 错误

`localtime()` [CX] 和 `localtime_r()` 函数在以下情况下可能失败：

- **EOVERFLOW** [CX] 结果无法表示。

## 示例

### 获取本地日期和时间

以下示例使用 `time()` 函数计算自 1970 年 1 月 1 日 0:00 UTC（纪元）以来经过的时间（以秒为单位），使用 `localtime()` 将该值转换为分解的时间，并使用 `asctime()` 将分解的时间值转换为可打印的字符串。

```c
#include <stdio.h>
#include <time.h>

int main(void)
{
    time_t result;

    result = time(NULL);
    printf("%s%ju secs since the Epoch\n",
        asctime(localtime(&result)),
            (uintmax_t)result);
    return(0);
}
```

此示例将当前时间写入 `stdout`，格式如下：

```
Wed Jun 26 10:32:15 1996
835810335 secs since the Epoch
```

### 获取文件的修改时间

以下示例在本地时区中打印给定文件的最后数据修改时间戳。

```c
#include <stdio.h>
#include <time.h>
#include <sys/stat.h>

int
print_file_time(const char *pathname)
{
    struct stat statbuf;
    struct tm *tm;
    char timestr[BUFSIZ];

    if(stat(pathname, &statbuf) == -1)
        return -1;
    if((tm = localtime(&statbuf.st_mtime)) == NULL)
        return -1;
    if(strftime(timestr, sizeof(timestr), "%Y-%m-%d %H:%M:%S", tm) == 0)
        return -1;
    printf("%s: %s.%09ld\n", pathname, timestr, statbuf.st_mtim.tv_nsec);
    return 0;
}
```

### 事件计时

以下示例获取当前时间，使用 `localtime()` 和 `asctime()` 将其转换为字符串，并使用 `fputs()` 将其打印到标准输出。然后打印到正在计时的事件的分钟数。

```c
#include <time.h>
#include <stdio.h>
...
time_t now;
int minutes_to_event;
...
time(&now);
printf("The time is ");
fputs(asctime(localtime(&now)), stdout);
printf("There are still %d minutes to the event.\n",
    minutes_to_event);
...
```

## 应用程序使用

`localtime_r()` 函数是线程安全的，并在用户提供的缓冲区中返回值，而不是可能使用可能被每次调用覆盖的静态数据区域。

## 基本原理

无。

## 未来方向

无。

## 参见

- `asctime()`
- `clock()`
- `ctime()`
- `difftime()`
- `futimens()`
- `getdate()`
- `gmtime()`
- `mktime()`
- `strftime()`
- `strptime()`
- `time()`
- `tzset()`

XBD 4.19 自纪元以来的秒数, `<time.h>`

## 更改历史

### 首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 5

在描述中添加了一个注释，表明 `localtime()` 函数不需要是可重入的。

`localtime_r()` 函数被包含用于与 POSIX 线程扩展对齐。

### Issue 6

`localtime_r()` 函数被标记为线程安全函数选项的一部分。

超过 ISO C 标准的扩展被标记。

应用程序使用部分被更新，包含有关线程安全函数及其避免可能使用静态数据区域的注释。

`restrict` 关键字被添加到 `localtime_r()` 原型中，以与 ISO/IEC 9899:1999 标准对齐。

添加了示例。

应用 IEEE Std 1003.1-2001/Cor 1-2002，项目 XSH/TC1/D6/32，添加了 [EOVERFLOW] 错误。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/55，更新了 `localtime_r()` 的错误处理。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/56，添加了一个要求：如果 `localtime_r()` 不设置 `tzname` 变量，则不应设置 `daylight` 或 `timezone` 变量。在支持 XSI 的系统上，`daylight`、`timezone` 和 `tzname` 变量都应设置为提供相同时区的信息。这更新了 `localtime_r()` 的描述，提及 `daylight` 和 `timezone` 以及 `tzname`。更新了参见部分。

### Issue 7

应用 Austin Group Interpretation 1003.1-2001 #156。

`localtime_r()` 函数从线程安全函数选项移至基础。

对示例部分进行了与支持细粒度时间戳相关的更改。

应用 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0363 [291]。

应用 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0201 [664]。

### Issue 8

应用 Austin Group Defect 1125，将"使用本地时区信息"更改为"应设置本地时区信息"。

应用 Austin Group Defect 1302，使 `localtime()` 函数与 ISO/IEC 9899:2018 标准对齐。

应用 Austin Group Defect 1376，从源自 ISO C 标准的某些文本中移除 CX 着色，并更新它以匹配 ISO C 标准。

应用 Austin Group Defect 1533，向 `tm` 结构体添加 `tm_gmtoff` 和 `tm_zone`。

应用 Austin Group Defect 1570，移除"=="中的额外间距。