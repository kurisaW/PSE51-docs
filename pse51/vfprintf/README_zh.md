# vfprintf

## 概要

```c
#include <stdarg.h>
#include <stdio.h>

[CX] int vasprintf(char **restrict ptr, const char *restrict format,
       va_list ap);
int vdprintf(int fildes, const char *restrict format, va_list ap);

int vfprintf(FILE *restrict stream, const char *restrict format,
      va_list ap);
int vprintf(const char *restrict format, va_list ap);
int vsnprintf(char *restrict s, size_t n, const char *restrict format,
      va_list ap);
int vsprintf(char *restrict s, const char *restrict format, va_list ap);
```

## 描述

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

[CX] `vasprintf()`、`vdprintf()`、`vfprintf()`、`vprintf()`、`vsnprintf()` 和 `vsprintf()` 函数应分别等同于 [CX] [`asprintf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/asprintf.html)、[`dprintf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/dprintf.html)、[`fprintf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fprintf.html)、[`printf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/printf.html)、[`snprintf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/snprintf.html) 和 [`sprintf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sprintf.html) 函数，不同之处在于它们不是使用可变数量的参数调用，而是使用由 [`<stdarg.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdarg.h.html) 定义的参数列表调用。

这些函数不应调用 `va_end` 宏。由于这些函数调用了 `va_arg` 宏，返回后 `ap` 的值是未指定的。

## 返回值

参考 [`fprintf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fprintf.html)。

## 错误

参考 [`fprintf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fprintf.html)。

---

*以下部分为补充信息。*

## 示例

无。

## 应用程序使用

使用这些函数的应用程序应在之后调用 `va_end(ap)` 进行清理。

## 原理

无。

## 未来方向

无。

## 另请参阅

[2.5 标准 I/O 流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05), [`fprintf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fprintf.html)

XBD [`<stdarg.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdarg.h.html), [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 5

添加了 `vsnprintf()` 函数。

### Issue 6

`vfprintf()`、`vprintf()`、`vsnprintf()` 和 `vsprintf()` 函数已更新以与 ISO/IEC 9899:1999 标准对齐。

### Issue 7

添加了 `vdprintf()` 函数，以补充来自 The Open Group 技术标准 2006 年扩展 API 集第 1 部分的 [`dprintf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/dprintf.html) 函数。

应用了 POSIX.1-2008 技术勘误 1，XSH/TC1-2008/0703 [14]。

### Issue 8

应用了 Austin Group 缺陷 1496，添加了 `vasprintf()` 函数。

*补充信息文本结束。*

---
