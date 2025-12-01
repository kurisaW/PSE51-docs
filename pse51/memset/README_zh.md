# memset

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

* * *

## NAME

memset — 设置内存中的字节

## SYNOPSIS

```c
#include <string.h>

void *memset(void *s, int c, size_t n);
```

## DESCRIPTION

[选项开始] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 本卷遵从 ISO C 标准。[选项结束]

`memset()` 函数应将 `c`（转换为 **unsigned char** 类型）复制到由 `s` 指向的对象的前 `n` 个字节中的每一个字节。

[选项开始] `memset()` 函数在有效输入时不应更改 errno 的设置。[选项结束]

## RETURN VALUE

`memset()` 函数应返回 `s`；没有保留用于指示错误的返回值。

## ERRORS

未定义错误。

* * *

_以下章节为信息性内容。_

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

XBD [\<string.h\>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 8

应用了 Austin Group Defect 448，增加了 `memset()` 在有效输入时不更改 errno 设置的要求。

_信息性文本结束。_

* * *

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved
\[ [Main Index](https://pubs.opengroup.org/onlinepubs/9799919799/mindex.html) | [XBD](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/contents.html) | [XSH](https://pubs.opengroup.org/onlinepubs/9799919799/functions/contents.html) | [XCU](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/contents.html) | [XRAT](https://pubs.opengroup.org/onlinepubs/9799919799/xrat/contents.html) \]