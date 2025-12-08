# setbuf

## SYNOPSIS (概要)

```c
#include <stdio.h>

void setbuf(FILE *restrict stream, char *restrict buf);
```

## DESCRIPTION (描述)

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

除了不返回值外，函数调用：

```c
setbuf(stream, buf)
```

应等效于：

```c
setvbuf(stream, buf, _IOFBF, BUFSIZ)
```

如果 `buf` 不是空指针，或者等效于：

```c
setvbuf(stream, buf, _IONBF, BUFSIZ)
```

如果 `buf` 是空指针。

## RETURN VALUE (返回值)

`setbuf()` 函数不应返回任何值。

## ERRORS (错误)

尽管 `setvbuf()` 接口可能以定义的方式设置 `errno`，但在调用 `setbuf()` 后 `errno` 的值是未指定的。

* * *

*以下各节为提供参考信息的部分。*

## EXAMPLES (示例)

无。

## APPLICATION USAGE (应用程序使用)

一个常见的错误源是在代码块中将缓冲区空间分配为"自动"变量，然后未能在同一代码块中关闭流。

使用 `setbuf()` 时，分配 BUFSIZ 字节的缓冲区不一定意味着所有 BUFSIZ 字节都用于缓冲区。

由于成功时不要求 `errno` 保持不变，为了正确检测并可能从错误中恢复，应用程序应使用 `setvbuf()` 而不是 `setbuf()`。

## RATIONALE (基本原理)

无。

## FUTURE DIRECTIONS (未来方向)

无。

## SEE ALSO (参见)

* [2.5 标准I/O流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
* [fopen()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fopen.html)
* [setvbuf()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setvbuf.html)

XBD [<stdio.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY (变更历史)

首次在 Issue 1 中发布。源自 SVID 的 Issue 1。

### Issue 6

`setbuf()` 的原型已更新以与 ISO/IEC 9899:1999 标准保持一致。

### Issue 7

应用了 POSIX.1-2008、Technical Corrigendum 1、XSH/TC1-2008/0546 [397]、XSH/TC1-2008/0547 [397] 和 XSH/TC1-2008/0548 [14]。

