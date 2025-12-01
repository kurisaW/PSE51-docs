# ferror — 测试流的错误指示器

## SYNOPSIS（概要）

```c
#include <stdio.h>

int ferror(FILE *stream);
```

## DESCRIPTION（描述）

本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 遵循 ISO C 标准。

`ferror()` 函数应测试由 `stream` 指向的流的错误指示器。

如果 `stream` 有效，`ferror()` 函数不应更改 `errno` 的设置。

## RETURN VALUE（返回值）

当且仅当 `stream` 的错误指示器被设置时，`ferror()` 函数应返回非零值。

## ERRORS（错误）

未定义错误。

---

## EXAMPLES（示例）

无。

## APPLICATION USAGE（应用程序使用）

无。

## RATIONALE（基本原理）

无。

## FUTURE DIRECTIONS（未来方向）

无。

## SEE ALSO（参见）

- [`clearerr()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/clearerr.html)
- [`feof()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/feof.html)
- [`fopen()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fopen.html)

XBD [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY（变更历史）

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 7

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0125 [401]。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*