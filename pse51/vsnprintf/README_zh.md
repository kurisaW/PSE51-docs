# vsnprintf

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

vasprintf, vdprintf, vfprintf, vprintf, vsnprintf, vsprintf — 格式化输出 stdarg 参数列表

## SYNOPSIS

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
int vsprintf(char *restrict s, const char *restrict format,
             va_list ap);
```

## DESCRIPTION

本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 本卷遵循 ISO C 标准。

`vasprintf()`、`vdprintf()`、`vfprintf()`、`vprintf()`、`vsnprintf()` 和 `vsprintf()` 函数应分别等效于 `asprintf()`、`dprintf()`、`fprintf()`、`printf()`、`snprintf()` 和 `sprintf()` 函数，不同之处在于它们不是使用可变数量的参数调用，而是使用由 `<stdarg.h>` 定义的参数列表调用。

这些函数不应调用 `va_end` 宏。由于这些函数调用 `va_arg` 宏，返回后 `ap` 的值是未指定的。

## RETURN VALUE

参考 `fprintf()`。

## ERRORS

参考 `fprintf()`。

---

*以下章节为信息性内容。*

## EXAMPLES

无。

## APPLICATION USAGE

使用这些函数的应用程序应随后调用 `va_end(ap)` 进行清理。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- 2.5 标准 I/O 流
- `fprintf()`

XBD `<stdarg.h>`, `<stdio.h>`

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 5

添加了 `vsnprintf()` 函数。

### Issue 6

更新了 `vfprintf()`、`vprintf()`、`vsnprintf()` 和 `vsprintf()` 函数，以与 ISO/IEC 9899:1999 标准保持一致。

### Issue 7

添加了 `vdprintf()` 函数，以补充 The Open Group Technical Standard, 2006, Extended API Set Part 1 中的 `dprintf()` 函数。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0703 [14]。

### Issue 8

应用了 Austin Group Defect 1496，添加了 `vasprintf()` 函数。

---

*信息性文本结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved