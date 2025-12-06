# gmtime, gmtime_r — 将时间值转换为分解的UTC时间

## SYNOPSIS

```c
#include <time.h>

struct tm *gmtime(const time_t *timer);

[CX] struct tm *gmtime_r(const time_t *restrict timer,
                         struct tm *restrict result);
```

## DESCRIPTION

对于 `gmtime()`：
[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵循 ISO C 标准。

`gmtime()` 函数应将由 `timer` 指向的自纪元以来以秒为单位的时间转换为分解时间，以协调世界时（UTC）表示。

[CX] 用作 `gmtime()` 参数的自纪元以来以秒为单位的时间与 `tm` 结构（在 `<time.h>` 头文件中定义）之间的关系是：结果应按照自纪元以来秒数定义中给出的表达式指定（参见 XBD 4.19 自纪元以来的秒数），其中结构中的名称与表达式中的名称相对应。

同样的关系适用于 `gmtime_r()`。

`gmtime()` 函数不必是线程安全的；但是，`gmtime()` 应避免与其自身、`asctime()`、`ctime()` 和 `localtime()` 之外的所有函数发生数据竞争。

`asctime()`、`ctime()`、`gmtime()` 和 `localtime()` 函数应返回两个静态对象之一的值：一个分解时间结构和一个 `char` 类型的数组。执行任何返回指向这些对象类型之一指针的函数都可能覆盖通过先前对其中任何函数的调用返回的值所指向的任何相同类型对象中的信息。

[CX] `gmtime_r()` 函数应将由 `timer` 指向的自纪元以来以秒为单位的时间转换为以协调世界时（UTC）表示的分解时间。分解时间存储在由 `result` 引用的结构中。`gmtime_r()` 函数还应返回同一结构的地址。

## RETURN VALUE

成功完成后，`gmtime()` 函数应返回指向 `struct tm` 的指针。如果检测到错误，`gmtime()` 应返回空指针 [CX] 并设置 `errno` 以指示错误。

成功完成后，`gmtime_r()` 应返回由参数 `result` 指向的结构地址。结构的 `tm_zone` 成员应设置为指向字符串 "UTC" 的指针，该字符串应具有静态存储持续时间。如果检测到错误，`gmtime_r()` 应返回空指针并设置 `errno` 以指示错误。

## ERRORS

`gmtime()` [CX] 和 `gmtime_r()` 函数在以下情况下应失败：

- **[EOVERFLOW]** [CX] 结果无法表示。

## EXAMPLES

无。

## APPLICATION USAGE

`gmtime_r()` 函数是线程安全的，并在用户提供的缓冲区中返回值，而不是可能使用每次调用都可能被覆盖的静态数据区域。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `asctime()`
- `clock()`
- `ctime()`
- `difftime()`
- `futimens()`
- `localtime()`
- `mktime()`
- `strftime()`
- `strptime()`
- `time()`

XBD 4.19 自纪元以来的秒数, `<time.h>`

## CHANGE HISTORY

### 首次发布于 Issue 1
源自 SVID 的 Issue 1。

### Issue 5
- 在 DESCRIPTION 中添加了一个注释，指出 `gmtime()` 函数不必是可重入的。
- 为了与 POSIX 线程扩展保持一致，包含了 `gmtime_r()` 函数。

### Issue 6
- `gmtime_r()` 函数被标记为线程安全函数选项的一部分。
- 标记了超出 ISO C 标准的扩展。
- 更新了 APPLICATION USAGE 部分，添加了关于线程安全函数及其避免可能使用静态数据区域的注释。
- 为了与 ISO/IEC 9899:1999 标准保持一致，向 `gmtime_r()` 原型添加了 `restrict` 关键字。
- 应用了 IEEE Std 1003.1-2001/Cor 1-2002，项目 XSH/TC1/D6/27，添加了 [EOVERFLOW] 错误。
- 应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/48，更新了 `gmtime_r()` 的错误处理。

### Issue 7
- 应用了 Austin Group Interpretation 1003.1-2001 #156。
- `gmtime_r()` 函数从线程安全函数选项移至 Base。

### Issue 8
- 应用了 Austin Group Defect 1302，使 `gmtime()` 函数与 ISO/IEC 9899:2018 标准保持一致。
- 应用了 Austin Group Defect 1376，从某些源自 ISO C 标准的文本中移除了 CX 着色，并更新以匹配 ISO C 标准。
- 应用了 Austin Group Defect 1533，向 `tm` 结构添加了 `tm_gmtoff` 和 `tm_zone`。

---
