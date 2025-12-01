# isgraph, isgraph_l — 测试可见字符

## 概要

```c
#include <ctype.h>

int isgraph(int c);

[CX] int isgraph_l(int c, locale_t locale);
```

## 描述

对于 `isgraph()`：[CX] 本参考页所描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`isgraph()` [CX] 和 `isgraph_l()` 函数应测试 `c` 在当前语言环境中，[CX] 或在由 `locale` 表示的语言环境中是否为 **graph** 字符类；参见 XBD [7. 语言环境](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07)。

`c` 参数是一个 **int** 类型，应用程序应确保其值是可以表示为 **unsigned char** 的字符或等于宏 EOF 的值。如果参数具有任何其他值，则行为未定义。

[CX] 如果 `isgraph_l()` 的 `locale` 参数是特殊语言环境对象 LC_GLOBAL_LOCALE 或不是有效的语言环境对象句柄，则行为未定义。

## 返回值

`isgraph()` [CX] 和 `isgraph_l()` 函数应在 `c` 是具有可见表示形式的字符时返回非零值；否则，它们应返回 0。

## 错误

未定义任何错误。

---

*以下各节为信息性内容。*

## 示例

无。

## 应用程序用法

为确保应用程序的可移植性，特别是在不同自然语言之间，只能使用这些函数和 参见 部分中列出的参考页中的函数来进行字符分类。

## 原理

无。

## 未来方向

无。

## 参见

- [`isalnum()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalnum.html)
- [`isalpha()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalpha.html)
- [`isblank()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isblank.html)
- [`iscntrl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/iscntrl.html)
- [`isdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isdigit.html)
- [`islower()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/islower.html)
- [`isprint()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isprint.html)
- [`ispunct()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ispunct.html)
- [`isspace()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isspace.html)
- [`isupper()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isupper.html)
- [`isxdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isxdigit.html)
- [`setlocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlocale.html)
- [`uselocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/uselocale.html)

XBD [7. 语言环境](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07), [`<ctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html), [`<locale.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/locale.h.html)

## 变更历史

### 首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

规范性文本已更新，避免对应用程序要求使用术语"必须"（must）。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 添加了 `isgraph_l()` 函数。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0289 [302], XSH/TC1-2008/0290 [283], 和 XSH/TC1-2008/0291 [283]。

*信息性文本结束。*

---

**版权所有 © 2001-2024 IEEE 和 The Open Group，保留所有权利**