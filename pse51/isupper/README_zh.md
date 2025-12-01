# isupper, isupper_l — 测试大写字母

## 概要

```c
#include <ctype.h>

int isupper(int c);

[CX] int isupper_l(int c, locale_t locale);
```

## 描述

对于 `isupper()`：[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本 POSIX.1-2024 卷遵从 ISO C 标准。

`isupper()` [CX] 和 `isupper_l()` 函数应测试 `c` 在当前区域设置中，[CX] 或在由 `locale` 表示的区域设置中是否为 **upper** 类字符；参见 XBD [7. 区域设置](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07)。

`c` 参数是一个 `int`，其值应由应用程序确保是可表示为 `unsigned char` 的字符或等于宏 EOF 的值。如果参数具有任何其他值，则行为未定义。

[CX] 如果 `isupper_l()` 的 `locale` 参数是特殊区域设置对象 LC_GLOBAL_LOCALE 或不是有效的区域设置对象句柄，则行为未定义。

## 返回值

如果 `c` 是大写字母，`isupper()` [CX] 和 `isupper_l()` 函数应返回非零值；否则，它们应返回 0。

## 错误

未定义错误。

## 示例

无。

## 应用程序用法

为确保应用程序的可移植性，尤其是在不同自然语言之间，只应使用这些函数和 参见 部分所列参考页中的函数进行字符分类。

## 原理

无。

## 未来方向

无。

## 参见

[`isalnum()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalnum.html), [`isalpha()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalpha.html), [`isblank()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isblank.html), [`iscntrl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/iscntrl.html), [`isdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isdigit.html), [`isgraph()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isgraph.html), [`islower()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/islower.html), [`isprint()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isprint.html), [`ispunct()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ispunct.html), [`isspace()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isspace.html), [`isxdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isxdigit.html), [`setlocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlocale.html), [`uselocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/uselocale.html)

XBD [7. 区域设置](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07), [`<ctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html), [`<locale.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/locale.h.html)

## 变更历史

首次发布于 Issue 1。派生自 SVID 的 Issue 1。

### Issue 6

规范性文本已更新，以避免使用术语"must"来表示应用程序要求。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 添加了 `isupper_l()` 函数。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0306 [302], XSH/TC1-2008/0307 [283], 和 XSH/TC1-2008/0308 [283]。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*