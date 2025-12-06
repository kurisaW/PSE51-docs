# isalpha, isalpha_l — 测试字母字符

## 概要

```c
#include <ctype.h>

int isalpha(int c);

[CX] int isalpha_l(int c, locale_t locale);
```

## 描述

对于 `isalpha()`：[CX] 本参考页所描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本 POSIX.1-2024 版本遵循 ISO C 标准。

`isalpha()` [CX] 和 `isalpha_l()` 函数应测试 `c` 是否为当前区域设置中 **alpha** 字符类的字符，[CX] 或分别为 `locale` 表示的区域设置中的字符；参见 XBD 7. 区域设置。

`c` 参数是一个 `int` 类型，应用程序应确保其值可以表示为 `unsigned char` 或等于宏 EOF 的值。如果参数具有任何其他值，则行为未定义。

[CX] 如果 `isalpha_l()` 的 `locale` 参数是特殊区域设置对象 LC_GLOBAL_LOCALE 或不是有效的区域设置对象句柄，则行为未定义。

## 返回值

如果 `c` 是字母字符，`isalpha()` [CX] 和 `isalpha_l()` 函数应返回非零值；否则应返回 0。

## 错误

未定义任何错误。

---

*以下部分为提供信息的内容。*

## 示例

无。

## 应用程序用法

为确保应用程序的可移植性，尤其是在自然语言之间，仅应使用这些函数以及参考部分中列出的其他函数进行字符分类。

## 基本原理

无。

## 未来方向

无。

## 另请参阅

- `isalnum()`
- `isblank()`
- `iscntrl()`
- `isdigit()`
- `isgraph()`
- `islower()`
- `isprint()`
- `ispunct()`
- `isspace()`
- `isupper()`
- `isxdigit()`
- `setlocale()`
- `uselocale()`

XBD 7. 区域设置, `<ctype.h>`, `<locale.h>`, `<stdio.h>`

## 变更历史

首次在 Issue 1 中发布。源自 SVID 的 Issue 1。

### Issue 6

规范性文本已更新，以避免对应用程序要求使用"必须"一词。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 中添加了 `isalpha_l()` 函数。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0277 [302], XSH/TC1-2008/0278 [283], 和 XSH/TC1-2008/0279 [283]。

---
