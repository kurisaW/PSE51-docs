# strtol, strtoll — 将字符串转换为长整型

## 概要

```c
#include <stdlib.h>

long strtol(const char *restrict nptr, char **restrict endptr, int base);
long long strtoll(const char *restrict nptr, char **restrict endptr, int base);
```

## 描述

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的冲突是无意的。POSIX.1-2024 标准服从 ISO C 标准。

这些函数应将 `nptr` 指向的字符串的初始部分分别转换为 `long` 和 `long long` 类型的表示。首先，它们将输入字符串分解为三个部分：

1. 一个初始的、可能为空的空白字节序列
2. 被解释为整数的主题序列，该整数的基数由 `base` 的值确定
3. 一个包含一个或多个无法识别字符的最终字符串，包括输入字符串的终止 NUL 字符。

然后它们应尝试将主题序列转换为整数，并返回结果。

如果 `base` 的值为 0，主题序列的预期形式为十进制常量、八进制常量或十六进制常量，其中任何一个都可以前面加 '+' 或 '-' 符号。十进制常量以非零数字开头，由十进制数字序列组成。八进制常量由前缀 '0' 组成，后面可以选择性地跟只包含数字 '0' 到 '7' 的序列。十六进制常量由前缀 0x 或 0X 组成，后面跟十进制数字序列和字母 'a'（或 'A'）到 'f'（或 'F'），这些字母的值分别为 10 到 15。

如果 `base` 的值在 2 到 36 之间，主题序列的预期形式是表示整数的字母和数字序列，该整数的基数由 `base` 指定，可以选择性地在前面加 '+' 或 '-' 符号。从 'a'（或 'A'）到 'z'（或 'Z'）的字母被赋予 10 到 35 的值；只允许那些赋予值小于 `base` 的字母。如果 `base` 的值为 16，字符 0x 或 0X 可以选择性地在字母和数字序列之前，如果有符号则在符号之后。

主题序列被定义为输入字符串的最长子序列，从第一个非空白字节开始，且具有预期形式。如果输入字符串为空或完全由空白字节组成，或者如果第一个非空白字节不是符号或允许的字母或数字，则主题序列应不包含任何字符。

如果主题序列具有预期形式且 `base` 的值为 0，则从第一个数字开始的字符序列应被解释为整数常量。如果主题序列具有预期形式且 `base` 的值在 2 到 36 之间，它应用作转换的基数，将每个字母赋予上述值。如果主题序列以 <连字符> 开头，则结果值应为转换值的负值。如果 `endptr` 不是空指针，则指向最终字符串的指针应存储在 `endptr` 指向的对象中。

在 C [CX] 或 POSIX 语言环境之外，可以接受其他特定于语言环境的主题序列形式。

如果主题序列为空或不具有预期形式，则不执行转换；如果 `endptr` 不是空指针，`nptr` 的值应存储在 `endptr` 指向的对象中。

如果成功，这些函数不应改变 `errno` 的设置。

由于在错误时返回 0、{LONG_MIN} 或 {LLONG_MIN}、以及 {LONG_MAX} 或 {LLONG_MAX}，而这些值在成功时也是有效的返回值，因此希望检查错误情况的应用程序应将 `errno` 设置为 0，然后调用 `strtol()` 或 `strtoll()`，然后检查 `errno`。

## 返回值

成功完成时，这些函数应返回转换后的值（如果有的话）。如果无法执行转换，应返回 0 [CX] 且 `errno` 可能被设置为 [EINVAL]。

[CX] 如果 `base` 的值不受支持，应返回 0 且 `errno` 应被设置为 [EINVAL]。

如果正确的值超出可表示值的范围，应返回 {LONG_MIN}、{LONG_MAX}、{LLONG_MIN} 或 {LLONG_MAX}（根据值的符号），并设置 `errno` 为 [ERANGE]。

## 错误

这些函数可能在以下情况下失败：

- **[EINVAL]** [CX] `base` 的值不受支持。
- **[ERANGE]** 要返回的值无法表示。

这些函数可能在以下情况下失败：

- **[EINVAL]** 无法执行转换。

## 示例

无。

## 应用程序用法

由于如果 `base` 的值不受支持，`*endptr` 的值是未指定的，应用程序应在调用前确保 `base` 具有支持的值（0 或 2 到 36 之间），或者在检查 `*endptr` 之前检查 [EINVAL] 错误。

## 基本原理

无。

## 未来方向

无。

## 参见

- [`fscanf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fscanf.html)
- [`isalpha()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalpha.html)
- [`strtod()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strtod.html)
- XBD [`<stdlib.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 5

更新了描述以表明如果函数成功则不改变 `errno`。

### Issue 6

标记了超出 ISO C 标准的扩展。

以下对 POSIX 实现的新要求源于与单一 UNIX 规范的对齐：

- 在返回值和错误部分，如果无法执行转换，则添加 [EINVAL] 可选错误条件。

为与 ISO/IEC 9899:1999 标准对齐进行了以下更改：

- 更新了 `strtol()` 原型。
- 添加了 `strtoll()` 函数。

### Issue 7

应用了 POSIX.1-2008 技术勘误表 1 的 XSH/TC1-2008/0616 [453]、XSH/TC1-2008/0617 [105]、XSH/TC1-2008/0618 [453]、XSH/TC1-2008/0619 [453] 和 XSH/TC1-2008/0620 [453]。

应用了 POSIX.1-2008 技术勘误表 2 的 XSH/TC2-2008/0351 [892]、XSH/TC2-2008/0352 [584]、XSH/TC2-2008/0353 [796] 和 XSH/TC2-2008/0354 [892]。

### Issue 8

应用了 Austin Group 缺陷 700，阐明了如何转换以 <连字符> 开头的主题序列。

应用了 Austin Group 缺陷 1163，阐明了输入字符串中空白字符的处理。

---

*来源：The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*
*版权所有 © 2001-2024 IEEE 和 The Open Group*