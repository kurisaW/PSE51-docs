# ungetc — 将字节推回输入流

## 概要

```c
#include <stdio.h>

int ungetc(int c, FILE *stream);
```

## 描述

`ungetc()` 函数应将由 `c` 指定的字节（转换为 **unsigned char**）推回到由 `stream` 指向的输入流中。推回的字节应在该流上的后续读取中以与其推入相反的顺序返回。在该流上成功调用文件定位函数（[`fseek()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fseek.html)、[`fseeko()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fseeko.html)、[`fsetpos()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fsetpos.html) 或 [`rewind()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/rewind.html)）或 [`fflush()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fflush.html)（以 `stream` 指向的流为参数）应丢弃该流的任何推回字节。与该流对应的外部存储应保持不变。

应提供一个字节的推回功能。如果在同一流上调用 `ungetc()` 过多次数而中间没有对该流进行读取或文件定位操作，则操作可能会失败。

如果 `c` 的值等于宏 EOF 的值，则操作应失败且输入流应保持不变。

成功调用 `ungetc()` 应清除该流的文件结束指示符。该流的文件位置指示符应通过每次成功调用 `ungetc()` 而递减；如果在调用前其值为 0，则调用后其值是未指定的。在所有推回字节都被读取后，文件位置指示符的值应与字节被推回之前的值相同。

## 返回值

成功完成时，`ungetc()` 应返回转换后推回的字节。否则，应返回 EOF。

## 错误

未定义任何错误。

## 应用用法

无。

## 原理

ISO C 标准包含以下文本："在读取或丢弃所有推回字符后，流的文件位置指示符的值应与字符被推回之前的值相同。" POSIX.1 从中省略了"或丢弃"，因为它是多余的——在 ISO C 标准中，丢弃操作由文件定位函数完成，并且不影响这些函数设置的位置。特别是，使用 SEEK_CUR 的 [`fseek()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fseek.html) 或 [`fseeko()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fseeko.html) 的相对搜索会相对于函数入口时的位置调整位置，而不是相对于推回字节被丢弃后的位置。POSIX.1 还要求在 ISO C 标准说 [`fflush()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fflush.html) 行为未定义的情况下，[`fflush()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fflush.html) 丢弃推回字节。

## 未来方向

ISO C 标准规定，在文件位置指示符在调用前为零的二进制流上使用 `ungetc()` 是一个过时功能。在 POSIX.1 中，二进制流和文本流没有区别，因此这适用于所有流。此功能可能会在本标准的未来版本中被移除。

## 另请参见

- [2.5 标准I/O流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
- [`fseek()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fseek.html)
- [`getc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getc.html)
- [`fsetpos()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fsetpos.html)
- [`read()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/read.html#tag_17_476)
- [`rewind()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/rewind.html)
- [`setbuf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setbuf.html)
- XBD [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 7

应用了 POSIX.1-2008、技术勘误 1、XSH/TC1-2008/0687 [87,93]、XSH/TC1-2008/0688 [87] 和 XSH/TC1-2008/0689 [14]。

### Issue 8

应用了 Austin Group Defect 701，阐明了流的文件位置指示符如何更新。

应用了 Austin Group Defect 1302，更改了未来方向部分。
