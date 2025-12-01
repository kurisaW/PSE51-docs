# strcat

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

strcat — 连接两个字符串

## SYNOPSIS

```c
#include <string.h>

char *strcat(char *restrict s1, const char *restrict s2);
```

## DESCRIPTION

`strcat()` 函数应将 `s2` 所指向的字符串的副本（包括终止的 NUL 字符）追加到 `s1` 所指向的字符串的末尾。`s2` 的初始字节覆盖 `s1` 末尾的 NUL 字符。如果在重叠的对象之间进行复制，则行为是未定义的。

`strcat()` 函数在有效输入时不应改变 `errno` 的设置。

## RETURN VALUE

`strcat()` 函数应返回 `s1`；没有保留返回值来指示错误。

## ERRORS

未定义错误。

---

*以下部分为信息性内容。*

## EXAMPLES

无。

## APPLICATION USAGE

此版本与 ISO C 标准对齐；这不会影响与 XPG3 应用程序的兼容性。此函数的可靠错误检测从未得到保证。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`strncat()`](strncat.html)
- XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

首次在 Issue 1 中发布。派生自 SVID 的 Issue 1。

### Issue 6

`strcat()` 原型已更新，以与 ISO/IEC 9899:1999 标准对齐。

### Issue 8

应用了 Austin Group Defect 448，增加了 `strcat()` 在有效输入时不改变 `errno` 设置的要求。

---

*信息性文本结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved