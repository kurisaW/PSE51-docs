# va_arg

## SYNOPSIS（概要）

```c
#include <stdarg.h>

type va_arg(va_list ap, type);
void va_copy(va_list dest, va_list src);
void va_end(va_list ap);
void va_start(va_list ap, argN);
```

## DESCRIPTION（描述）

参考 XBD [`<stdarg.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdarg.h.html)

[返回页面顶部](#top)

* * *

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved
\[ [主索引](https://pubs.opengroup.org/onlinepubs/9799919799/mindex.html) | [XBD](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/contents.html) | [XSH](https://pubs.opengroup.org/onlinepubs/9799919799/functions/contents.html) | [XCU](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/contents.html) | [XRAT](https://pubs.opengroup.org/onlinepubs/9799919799/xrat/contents.html) \]

* * *

[<<< 前一个](https://pubs.opengroup.org/onlinepubs/9799919799/functions/uselocale.html) | [首页](https://pubs.opengroup.org/onlinepubs/9799919799/functions/contents.html) | [下一个 >>>](https://pubs.opengroup.org/onlinepubs/9799919799/functions/vfprintf.html)