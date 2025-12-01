# setvbuf — 为流分配缓冲区

## SYNOPSIS（函数概要）

```c
#include <stdio.h>

int setvbuf(FILE *restrict stream, char *restrict buf, int type, size_t size);
```

## DESCRIPTION（函数说明）

`setvbuf()` 函数可以在由 `stream` 指向的流与打开的文件关联之后，但在对该流执行任何其他��作（除了对 `setvbuf()` 的不成功调用之外）之前使用。参数 `type` 确定 `stream` 应如何进行缓冲，如下所示：

- `_IOFBF` 将导致输入/输出完全缓冲。
- `_IOLBF` 将导致输入/输出行缓冲。
- `_IONBF` 将导致输入/输出无缓冲。

如果 `buf` 不是空指针，则它指向的数组可以用来代替由 `setvbuf()` 分配的缓冲区，参数 `size` 指定数组的大小；否则，`size` 可以确定由 `setvbuf()` 函数分配的缓冲区的大小。数组在任何时候的内容都是未指定的。

有关流的信息，请参见 [2.5 标准I/O流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)。

## RETURN VALUE（返回值）

成功完成后，`setvbuf()` 应返回 0。否则，如果为 `type` 给定了无效值或无法满足请求，它应返回非零值，并可能设置 `errno` 来指示错误。

## ERRORS（错误条件）

`setvbuf()` 函数可能会失败：

- **EBADF**：流不是内存流，且流底层的文件描述符无效。

## EXAMPLES（示例）

无。

## APPLICATION USAGE（应用程序用法）

一个常见的错误源是在代码块中将缓冲区空间作为"自动"变量分配，然后未能在同一代码块中关闭流。

使用 `setvbuf()` 时，分配 `size` 字节的缓冲区并不一定意味着所有 `size` 字节都用于缓冲区。

应用程序应注意，许多实现对来自终端设备的输入只提供行缓冲。

## RATIONALE（基本原理）

无。

## FUTURE DIRECTIONS（未来方向）

无。

## SEE ALSO（参见）

- [2.5 标准I/O流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
- [fopen()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fopen.html)
- [setbuf()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setbuf.html)
- [<stdio.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY（变更历史）

首次在 Issue 1 中发布。源自 SVID 的 Issue 1。

### Issue 6

标记了超越 ISO C 标准的扩展。

`setvbuf()` 原型已更新，以与 ISO/IEC 9899:1999 标准对齐。

### Issue 8

应用了 Austin Group Defect 1144，更改了 [EBADF] 错误条件。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*版权所有 © 2001-2024 The IEEE and The Open Group*