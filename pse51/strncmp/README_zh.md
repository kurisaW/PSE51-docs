# strncmp

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

---

## NAME (名称)

strncmp — 比较两个字符串的部分内容

## SYNOPSIS (概要)

```c
#include <string.h>

int strncmp(const char *s1, const char *s2, size_t n);
```

## DESCRIPTION (描述)

[Option Start] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 本卷遵循 ISO C 标准。 [Option End]

`strncmp()` 函数应当比较由 `s1` 指向的数组与由 `s2` 指向的数组中不超过 `n` 个字节的内容（NUL 字符后的字节不参与比较）。

非零返回值的符号由被比较字符串中第一对不同字节（两者都解释为 `unsigned char` 类型）的差值符号决定。

[Option Start] 在有效输入时，`strncmp()` 函数不应改变 `errno` 的设置。 [Option End]

## RETURN VALUE (返回值)

成功完成后，如果 `s1` 指向的可能以 null 结尾的数组分别大于、等于或小于 `s2` 指向的可能以 null 结尾的数组，`strncmp()` 应返回一个大于、等于或小于 0 的整数。

## ERRORS (错误)

未定义任何错误。

---

*以下部分为补充信息。*

## EXAMPLES (示例)

无。

## APPLICATION USAGE (应用程序用法)

无。

## RATIONALE (基本原理)

无。

## FUTURE DIRECTIONS (未来方向)

无。

## SEE ALSO (参见)

- [`strcmp()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcmp.html)
- XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY (变更历史)

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

超出 ISO C 标准的扩展被标记出来。

### Issue 8

应用了 Austin Group Defect 448，增加了在有效输入时 `strncmp()` 不改变 `errno` 设置的要求。

*补充信息结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved
[Main Index](https://pubs.opengroup.org/onlinepubs/9799919799/mindex.html) | [XBD](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/contents.html) | [XSH](https://pubs.opengroup.org/onlinepubs/9799919799/functions/contents.html) | [XCU](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/contents.html) | [XRAT](https://pubs.opengroup.org/onlinepubs/9799919799/xrat/contents.html)