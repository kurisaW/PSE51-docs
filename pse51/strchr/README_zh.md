# strchr

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

---

## NAME（名称）

strchr — 字符串扫描操作

## SYNOPSIS（概要）

```c
#include <string.h>

char *strchr(const char *s, int c);
```

## DESCRIPTION（描述）

[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`strchr()` 函数应在 `s` 指向的字符串中定位 `c`（转换为 `char` 类型）第一次出现的位置。终止的 NUL 字符被认为是字符串的一部分。

[CX] `strchr()` 函数在有效输入时不应改变 `errno` 的设置。

## RETURN VALUE（返回值）

完成后，`strchr()` 应返回指向该字节的指针，如果未找到该字节则返回空指针。

## ERRORS（错误）

未定义错误。

---

*以下部分为补充信息。*

## EXAMPLES（示例）

无。

## APPLICATION USAGE（应用程序用法）

无。

## RATIONALE（基本原理）

无。

## FUTURE DIRECTIONS（未来方向）

无。

## SEE ALSO（参见）

- [`strrchr()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strrchr.html)
- XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY（变更历史）

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

标记了超出 ISO C 标准的扩展。

### Issue 8

应用了 Austin Group 缺陷 448，增加了 `strchr()` 在有效输入时不改变 `errno` 设置的要求。

*补充信息结束。*

---

*UNIX® 是 The Open Group 的注册商标。*
*POSIX™ 是 The IEEE 的商标。*
*Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved*