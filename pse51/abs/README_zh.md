# abs

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

* * *

## NAME

abs — 返回整数的绝对值

## SYNOPSIS

```c
#include <stdlib.h>

int abs(int i);
```

## DESCRIPTION

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 本卷遵循 ISO C 标准。

`abs()` 函数应计算其整数操作数 `i` 的绝对值。如果结果无法表示，则行为未定义。

## RETURN VALUE

`abs()` 函数应返回其整数操作数的绝对值。

## ERRORS

未定义任何错误。

* * *

*以下小节为参考信息。*

## EXAMPLES

无。

## APPLICATION USAGE

由于 POSIX.1 要求 `int` 类型使用二进制补码表示，具有最大量级的负整数 {INT_MIN} 的绝对值无法表示，因此 `abs(INT_MIN)` 是未定义的。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`fabs()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fabs.html)
- [`labs()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/labs.html)

XBD [`<stdlib.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

标记了超出 ISO C 标准的扩展。

### Issue 8

应用了 Austin Group Defect 1108，更改了 APPLICATION USAGE 小节。

*参考信息结束。*

* * *

[返回页首](#top)

* * *

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved
[ [Main Index](https://pubs.opengroup.org/onlinepubs/9799919799/mindex.html) | [XBD](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/contents.html) | [XSH](https://pubs.opengroup.org/onlinepubs/9799919799/functions/contents.html) | [XCU](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/contents.html) | [XRAT](https://pubs.opengroup.org/onlinepubs/9799919799/xrat/contents.html) ]

* * *