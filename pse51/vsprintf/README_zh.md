# vsprintf - 格式化 stdarg 参数列表的输出

## 概要

```c
#include <stdarg.h>
#include <stdio.h>

/* XSI 扩展 */
int vasprintf(char **restrict ptr, const char *restrict format,
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

本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 标准卷遵循 ISO C 标准。

`vasprintf()`、`vdprintf()`、`vfprintf()`、`vprintf()`、`vsnprintf()` 和 `vsprintf()` 函数应分别等同于 `asprintf()`、`dprintf()`、`fprintf()`、`printf()`、`snprintf()` 和 `sprintf()` 函数，不同之处在于它们不是以可变数量的参数调用，而是以 `<stdarg.h>` 定义的参数列表调用。

这些函数不应调用 `va_end` 宏。由于这些函数调用 `va_arg` 宏，返回后 `ap` 的值是未指定的。

## 返回值

参考 `fprintf()`。

## 错误

参考 `fprintf()`。

# 示例

无。

# 应用程序用法

使用这些函数的应用程序之后应调用 `va_end(ap)` 进行清理。

# 原理

无。

# 未来方向

无。

# 参见

- [2.5 标准 I/O 流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
- [fprintf()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fprintf.html)
- `<stdarg.h>`
- `<stdio.h>`

# 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

## Issue 5

添加了 `vsnprintf()` 函数。

## Issue 6

更新了 `vfprintf()`、`vprintf()`、`vsnprintf()` 和 `vsprintf()` 函数以与 ISO/IEC 9899:1999 标准保持一致。

## Issue 7

添加了 `vdprintf()` 函数，以补充来自 The Open Group 技术标准 2006 年扩展 API 集第 1 部分的 `dprintf()` 函数。

应用了 POSIX.1-2008，技术勘误表 1，XSH/TC1-2008/0703 [14]。

## Issue 8

应用了 Austin Group 缺陷 1496，添加了 `vasprintf()` 函数。

---
