# isblank, isblank_l — 测试空白字符

## 概要

```c
#include <ctype.h>

int isblank(int c);

[CX] int isblank_l(int c, locale_t locale);
```

## 描述

对于 `isblank()`：[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 卷遵循 ISO C 标准。

`isblank()` [CX] 和 `isblank_l()` 函数应测试 `c` 是否为当前语言环境中 **blank**（空白）类别的字符，[CX] 或分别测试 `locale` 表示的语言环境中的字符；参见 XBD [7. Locale]。

`c` 参数是 `int` 类型，应用程序应确保其值是可表示为 `unsigned char` 的字符或等于宏 EOF 的值。如果参数具有任何其他值，则行为是未定义的。

[CX] 如果传递给 `isblank_l()` 的 `locale` 参数是特殊语言环境对象 LC_GLOBAL_LOCALE 或不是有效的语言环境对象句柄，则行为是未定义的。

## 返回值

如果 `c` 是 <blank>（空白字符），`isblank()` [CX] 和 `isblank_l()` 函数应返回非零值；否则应返回 0。

## 错误

未定义错误。

## 示例

无。

## 应用程序使用

为确保应用程序的可移植性，特别是在不同自然语言之间，应仅使用这些函数和"参见"部分列出的参考页面中的函数进行字符分类。

## 原理

无。

## 未来方向

无。

## 参见

[`isalnum()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalnum.html)、
[`isalpha()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalpha.html)、
[`iscntrl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/iscntrl.html)、
[`isdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isdigit.html)、
[`isgraph()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isgraph.html)、
[`islower()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/islower.html)、
[`isprint()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isprint.html)、
[`ispunct()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ispunct.html)、
[`isspace()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isspace.html)、
[`isupper()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isupper.html)、
[`isxdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isxdigit.html)、
[`setlocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlocale.html)、
[`uselocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/uselocale.html)

XBD [7. Locale]、[`<ctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html)、[`<locale.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/locale.h.html)

## 变更历史

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 中添加了 `isblank_l()` 函数。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0280 [302], XSH/TC1-2008/0281 [283] 和 XSH/TC1-2008/0282 [283]。