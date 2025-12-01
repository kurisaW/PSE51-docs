# putchar

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## 名称

putchar — 在标准输出流中写入一个字节

## 概要

```c
#include <stdio.h>

int putchar(int c);
```

## 描述

[CX] 本参考页所描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵循 ISO C 标��。

函数调用 `putchar(c)` 应等效于 `putc(c,stdout)`。

## 返回值

参考 `fputc()`。

## 错误

参考 `fputc()`。

---

*以下章节为参考性内容。*

## 示例

无。

## 应用程序使用

无。

## 基本原理

无。

## 未来方向

无。

## 另请参阅

- 2.5 标准输入输出流
- `putc()`
- XBD `<stdio.h>`

## 变更历史

���次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 7

POSIX.1-2008 技术勘误 1，XSH/TC1-2008/0471 [14] 已应用。

---

*参考性文本结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group，保留所有权利