# strncpy

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

---

## NAME

stpncpy, strncpy — 复制固定长度字符串，返回指向数组末尾的指针

## SYNOPSIS

```c
#include <string.h>

[CX] char *stpncpy(char *restrict s1, const char *restrict s2, size_t n);
char *strncpy(char *restrict s1, const char *restrict s2, size_t n);
```

## DESCRIPTION

对于 strncpy()：[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 本卷遵循 ISO C 标准。

[CX] stpncpy() 和 strncpy() 函数应从 s2 指向的数组向 s1 指向的数组复制不超过 n 个字节（NUL 字符后的字节不复制）。

如果 s2 指向的数组是一个长度小于 n 字节的字符串，则应在 s1 指向的数组中的副本后附加 NUL 字符，直到总共写入 n 个字节。

如果复制操作发生在重叠的对象之间，则行为未定义。

[CX] strncpy() 和 stpncpy() 函数在有效输入时不应更改 errno 的设置。

## RETURN VALUE

[CX] 如果向目标写入了 NUL 字符，stpncpy() 函数应返回第一个此类 NUL 字符的地址。否则，应返回 &s1[n]。

strncpy() 函数应返回 s1。

没有保留用于指示错误的返回值。

## ERRORS

未定义错误。

---

*以下章节为补充信息。*

## EXAMPLES

无。

## APPLICATION USAGE

应用程序必须在 s1 中提供用于传输 n 个字节的空间，并确保 s2 和 s1 数组不重叠。

字符移动在不同实现中的执行方式不同。因此，重叠移动可能会产生意外结果。

如果 s2 指向的数组的前 n 个字节中没有 NUL 字符字节，则结果不是以 null 结尾的。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- strcpy()
- strlcat()
- wcsncpy()

XBD <string.h>

## CHANGE HISTORY

**首次发布于 Issue 1。** 源自 SVID 的 Issue 1。

### Issue 6

strncpy() 原型已更新，以与 ISO/IEC 9899:1999 标准保持一致。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 1 添加了 stpncpy() 函数。

### Issue 8

应用了 Austin Group Defect 448，添加了 strncpy() 和 stpncpy() 在有效输入时不更改 errno 设置的要求。

应用了 Austin Group Defect 986，在 SEE ALSO 章节中添加了 strlcat()。

*补充信息结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved
[Main Index](https://pubs.opengroup.org/onlinepubs/9799919799/mindex.html) | [XBD](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/contents.html) | [XSH](https://pubs.opengroup.org/onlinepubs/9799919799/functions/contents.html) | [XCU](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/contents.html) | [XRAT](https://pubs.opengroup.org/onlinepubs/9799919799/xrat/contents.html)