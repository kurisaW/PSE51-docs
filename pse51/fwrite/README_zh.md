# fwrite — 二进制输出

## 概要

```c
#include <stdio.h>

size_t fwrite(const void *restrict ptr,
              size_t size,
              size_t nitems,
              FILE *restrict stream);
```

## 描述

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`fwrite()` 函数应从 `ptr` 指向的数组中，向 `stream` 指向的流写入最多 `nitems` 个元素，每个元素的大小由 `size` 指定。对于每个对象，应调用 `size` 次 `fputc()` 函数，从精确覆盖该对象的 **unsigned char** 数组中按顺序取值。流的文件位置指示器（如果已定义）应按成功写入的字节数前进。如果发生错误，流的文件位置指示器的最终值是未指定的。

[CX] 文件的数据最后修改时间戳和文件状态最后更改时间戳应被标记为更新，更新时机在 `fwrite()` 成功执行与下一次对同一流成功完成 `fflush()` 或 `fclose()` 调用之间，或调用 `exit()` 或 `abort()` 之间。

## 返回值

`fwrite()` 函数应返回成功写入的元素数量，只有在遇到写入错误时该数量才可能小于 `nitems`。如果 `size` 或 `nitems` 为 0，`fwrite()` 应返回 0 且流的状态保持不变。否则，如果发生写入错误，应设置流的错误指示器，[CX] 并且应设置 `errno` 来指示错误。

## 错误

参考 `fputc()`。

*以下部分为参考信息。*

## 示例

无。

## 应用用法

由于元素长度和字节顺序可能存在差异，使用 `fwrite()` 写入的文件是应用程序相关的，可能无法被不同应用程序或同一应用程序在不同处理器上使用 `fread()` 读取。

## 原理

无。

## 未来方向

无。

## 参见

- 2.5 标准 I/O 流
- `ferror()`
- `fopen()`
- `fprintf()`
- `putc()`
- `puts()`
- `write()`
- XBD `<stdio.h>`

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

标记了超越 ISO C 标准的扩展。

为与 ISO/IEC 9899:1999 标准对齐，进行了以下更改：

- 更新了 `fwrite()` 原型。
- 更新了描述部分，阐明如何使用 `fputc()` 写出数据。

### Issue 7

进行了与支持细粒度时间戳相关的更改。

应用了 POSIX.1-2008 Technical Corrigendum 1, XSH/TC1-2008/0228 [14]。

### Issue 8

应用了 Austin Group Defect 1196，阐明了返回值部分。