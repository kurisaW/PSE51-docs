# isalnum, isalnum_l — 测试字母数字字符

## 概要

```c
#include <ctype.h>

int isalnum(int c);

[CX] int isalnum_l(int c, locale_t locale);
```

## 描述

对于 `isalnum()`：[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`isalnum()` [CX] 和 `isalnum_l()` 函数应测试 `c` 是否在当前区域设置中属于 **alpha** 或 **digit** 类的字符，[CX] 或分别在由 `locale` 表示的区域设置中；参见 XBD [7. 区域设置](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07)。

`c` 参数是一个 `int`，其值应确保应用程序可以表示为 `unsigned char` 或等于宏 EOF 的值。如果参数具有任何其他值，则行为是未定义的。

[CX] 如果 `isalnum_l()` 的 `locale` 参数是特殊区域设置对象 LC_GLOBAL_LOCALE 或不是有效的区域设置对象句柄，则行为是未定义的。

## 返回值

如果 `c` 是字母数字字符，`isalnum()` [CX] 和 `isalnum_l()` 函数应返回非零值；否则，它们应返回 0。

## 错误

未定义任何错误。

---

*以下部分是提供信息的。*

## 示例

无。

## 应用程序用法

为确保应用程序的可移植性，特别是在不同自然语言之间，应仅使用这些函数和参见 also 部分列出的参考页面中的函数进行字符分类。

## 原理

无。

## 未来方向

无。

## 参见

- [`isalpha()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalpha.html)
- [`isblank()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isblank.html)
- [`iscntrl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/iscntrl.html)
- [`isdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isdigit.html)
- [`isgraph()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isgraph.html)
- [`islower()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/islower.html)
- [`isprint()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isprint.html)
- [`ispunct()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ispunct.html)
- [`isspace()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isspace.html)
- [`isupper()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isupper.html)
- [`isxdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isxdigit.html)
- [`setlocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlocale.html)
- [`uselocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/uselocale.html)

XBD [7. 区域设置](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07), [`<ctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html), [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

规范性文本已更新，以避免使用术语"must"来表示应用程序要求。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 中添加了 `isalnum_l()` 函数。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0274 [302], XSH/TC1-2008/0275 [283], 和 XSH/TC1-2008/0276 [283]。

---
