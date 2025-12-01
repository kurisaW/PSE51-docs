# strxfrm, strxfrm_l — 字符串转换

## 概要

```c
#include <string.h>

size_t strxfrm(char *restrict s1, const char *restrict s2, size_t n);

/* [CX] 扩展 */
size_t strxfrm_l(char *restrict s1, const char *restrict s2,
                 size_t n, locale_t locale);
```

## 描述

对于 `strxfrm()`：[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`strxfrm()` [CX] 和 `strxfrm_l()` 函数应转换由 `s2` 指向的字符串，并将结果字符串放置到由 `s1` 指向的数组中。转换的方式是：如果对两个转换后的字符串应用 `strcmp()`，它应返回大于、等于或小于 0 的值，分别对应于对相同的两个原始字符串 [CX] 在相同区域设置下应用 `strcoll()` [CX] 或 `strcoll_l()` 的结果。放置到由 `s1` 指向的结果数组中的字节数不超过 `n` 个，包括终止的 NUL 字符。如果 `n` 为 0，则允许 `s1` 为空指针。如果在重叠的对象之间进行复制，则行为未定义。

[CX] `strxfrm()` 和 `strxfrm_l()` 函数如果成功执行，不应更改 `errno` 的设置。

由于没有保留返回值来指示错误，希望检查错误情况的应用程序应将 `errno` 设置为 0，然后调用 `strxfrm()` [CX] 或 `strxfrm_l()`，然后检查 `errno`。

[CX] 如果 `strxfrm_l()` 的 `locale` 参数是特殊的区域设置对象 LC_GLOBAL_LOCALE 或不是有效的区域设置对象句柄，则行为未定义。

## 返回值

成功完成后，`strxfrm()` [CX] 和 `strxfrm_l()` 应返回转换后字符串的长度（不包括终止的 NUL 字符）。如果返回值为 `n` 或更大，则由 `s1` 指向的数组的内容未指定。

出错时，`strxfrm()` [CX] 和 `strxfrm_l()` 可能设置 `errno`，但没有保留返回值来指示错误。

## 错误

这些函数可能失败的情况：

- **[EINVAL]** [CX] `s2` 参数指向的字符串包含排序序列域之外的字符。

---

*以下部分为提供信息的部分。*

## 示例

无。

## 应用程序用法

转换函数的设计使得两个转换后的字符串可以通过 `strcmp()` 进行排序，其排序结果与当前区域设置（类别 `LC_COLLATE`）中的排序序列信息相对应。

当 `n` 为 0 时允许 `s1` 为空指针这一特性有助于在进行转换之前确定 `s1` 数组的大小。

## 基本原理

无。

## 未来方向

无。

## 另请参阅

- `strcmp()`
- `strcoll()`
- `<string.h>`

## 变更历史

首次发布于第 3 版。为与 ISO C 标准保持一致而包含在内。

### 第 5 版

更新了 DESCRIPTION 部分，以表明如果函数成功执行，`errno` 不会改变。

### 第 6 版

标记了超出 ISO C 标准的扩展。

以下对 POSIX 实现的新要求来源于与单一 UNIX 规范的对齐：

- 在 RETURN VALUE 和 ERRORS 部分中，如果无法执行转换，则添加了 [EINVAL] 可选错误条件。

`strxfrm()` 原型已更新，以与 ISO/IEC 9899:1999 标准对齐。

### 第 7 版

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 中添加了 `strxfrm_l()` 函数。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0624 [283], XSH/TC1-2008/0625 [283], 和 XSH/TC1-2008/0626 [302]。

---

*提供信息的文本结束。*

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
版权所有 © 2001-2024 The IEEE and The Open Group，保留所有权利。