# isxdigit, isxdigit_l — 测试十六进制数字字符

## 概要

```c
#include <ctype.h>

int isxdigit(int c);

[CX] int isxdigit_l(int c, locale_t locale);
```

## 描述

对于 `isxdigit()`：
[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 本卷遵循 ISO C 标准。

`isxdigit()` [CX] 和 `isxdigit_l()` 函数应测试 `c` 在当前语言环境中是否为 **xdigit** 类的字符，[CX] 或分别在由 `locale` 表示的语言环境中是否为 **xdigit** 类的字符；参见 XBD [7. 语言环境](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07)。

`c` 参数是一个 `int`，应用程序应确保其值是可表示为 `unsigned char` 的字符或等于宏 EOF 的值。如果参数具有任何其他值，则行为是未定义的。

[CX] 如果 `isxdigit_l()` 的 `locale` 参数是特殊的语言环境对象 LC_GLOBAL_LOCALE 或不是有效的语言环境对象句柄，则行为是未定义的。

## 返回值

如果 `c` 是十六进制数字字符，`isxdigit()` [CX] 和 `isxdigit_l()` 函数应返回非零值；否则，它们应返回 0。

## 错误

未定义错误。

*以下章节为信息性内容。*

## 示例

无。

## 应用程序用法

为确保应用程序的可移植性，尤其是在不同自然语言之间，应仅使用这些函数和 SEE ALSO 部分列出的参考页中的函数来进行字符分类。

## 原理

无。

## 未来方向

无。

## 另请参阅

[`isalnum()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalnum.html), [`isalpha()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalpha.html), [`isblank()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isblank.html), [`iscntrl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/iscntrl.html), [`isdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isdigit.html), [`isgraph()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isgraph.html), [`islower()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/islower.html), [`isprint()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isprint.html), [`ispunct()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ispunct.html), [`isspace()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isspace.html), [`isupper()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isupper.html)

XBD [7. 语言环境](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07), [`<ctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html)

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

更新规范性文本，避免对应用程序要求使用术语"必须"（must）。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 添加了 `isxdigit_l()` 函数。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0347 [302], XSH/TC1-2008/0348 [283], 和 XSH/TC1-2008/0349 [283]。

*信息性文本结束。*

---

*UNIX® 是 The Open Group 的注册商标。*
*POSIX™ 是 The IEEE 的商标。*
*版权所有 © 2001-2024 The IEEE and The Open Group，保留所有权利*