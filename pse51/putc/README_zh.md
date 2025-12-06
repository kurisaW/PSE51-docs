# putc — 向流中写入一个字节

## 概要

```c
#include <stdio.h>

int putc(int c, FILE *stream);
```

## 描述

`putc()` 函数应等价于 `fputc()`，但如果它被实现为宏，则可能会多次求值 `stream` 参数，因此该参数永远不应该是具有副作用的表达式。

## 返回值

参考 `fputc()`。

## 错误

参考 `fputc()`。

## 示例

无。

## 应用程序用法

由于 `putc()` 可能被实现为宏，它可能会错误地处理具有副作用的 `stream` 参数。特别是，`putc(c,*f++)` 不一定能正确工作。因此，在这种情况下不推荐使用此函数；应该使用 `fputc()` 代替。

## 原理

无。

## 未来方向

无。

## 参见

2.5 标准 I/O 流，`fputc()`

XBD `<stdio.h>`

## 变更历史

首次在 Issue 1 中发布。源自 SVID 的 Issue 1。

### Issue 7

应用了 POSIX.1-2008 Technical Corrigendum 1, XSH/TC1-2008/0470 [14]。

---
