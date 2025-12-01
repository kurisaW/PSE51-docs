# isdigit, isdigit_l — 测试十进制数字字符

## 概要

```c
#include <ctype.h>

int isdigit(int c);

[CX] int isdigit_l(int c, locale_t locale);
```

## 描述

对于 `isdigit()`：
[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 的本卷遵循 ISO C 标准。

`isdigit()` [CX] 和 `isdigit_l()` 函数应测试 `c` 是否为当前语言环境 [CX] 或由 `locale` 表示的语言环境中的 **digit** 类字符；参见 XBD [7. Locale](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07)。

`c` 参数是一个 `int`，应用程序应确保其值是可表示为 `unsigned char` 的字符或等于宏 EOF 的值。如果参数具有任何其他值，则行为未定义。

[CX] 如果 `isdigit_l()` 的 `locale` 参数是特殊语言环境对象 LC_GLOBAL_LOCALE 或不是有效的语言环境对象句柄，则行为未定义。

## 返回值

如果 `c` 是十进制数字字符，`isdigit()` [CX] 和 `isdigit_l()` 函数应返回非零值；否则应返回 0。

## 错误

未定义错误。

*以下章节为补充信息。*

## 示例

无。

## 应用用法

为确保应用程序的可移植性，特别是在自然语言之间，应仅使用这些函数和 SEE ALSO 部分列出的参考页面中的函数进行字符分类。

## 原理

无。

## 未来方向

无。

## 参见

- [`isalnum()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalnum.html)
- [`isalpha()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalpha.html)
- [`isblank()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isblank.html)
- [`iscntrl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/iscntrl.html)
- [`isgraph()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isgraph.html)
- [`islower()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/islower.html)
- [`isprint()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isprint.html)
- [`ispunct()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ispunct.html)
- [`isspace()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isspace.html)
- [`isupper()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isupper.html)
- [`isxdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isxdigit.html)

XBD [7. Locale](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07), [<ctype.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html), [<locale.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/locale.h.html)

## 更改历史

### 首次发布于 Issue 1。
源自 SVID 的 Issue 1。

### Issue 6

更新规范性文本，避免对应用程序要求使用术语"必须"。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 添加了 `isdigit_l()` 函数。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0286 [302], XSH/TC1-2008/0287 [283] 和 XSH/TC1-2008/0288 [283]。

*补充信息结束。*

---

**版权所有 © 2001-2024 The IEEE 和 The Open Group，保留所有权利**