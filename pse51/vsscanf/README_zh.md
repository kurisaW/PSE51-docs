# vsscanf

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

vfscanf, vscanf, vsscanf — 格式化输入 stdarg 参数列表

## SYNOPSIS

```c
#include <stdarg.h>
#include <stdio.h>

int vfscanf(FILE *restrict stream,
            const char *restrict format,
            va_list arg);

int vscanf(const char *restrict format,
           va_list arg);

int vsscanf(const char *restrict s,
            const char *restrict format,
            va_list arg);
```

## DESCRIPTION

[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`vscanf()`、`vfscanf()` 和 `vsscanf()` 函数应分别等价于 `scanf()`、`fscanf()` 和 `sscanf()` 函数，不同之处在于它们不是以可变数量的参数调用，而是以 `<stdarg.h>` 头文件中定义的参数列表调用。这些函数不应调用 `va_end` 宏。由于这些函数调用 `va_arg` 宏，因此返回后 `ap` 的值是未指定的。

## RETURN VALUE

参考 `fscanf()`。

## ERRORS

参考 `fscanf()`。

---

*以下章节为参考信息。*

## EXAMPLES

无。

## APPLICATION USAGE

使用这些函数的应用程序应调用 `va_end(ap)` 进行清理。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [2.5 标准 I/O 流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
- [fscanf()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fscanf.html)
- XBD [`<stdarg.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdarg.h.html), [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

### Issue 7

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0704 [14]。

