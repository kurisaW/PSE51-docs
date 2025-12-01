# toupper, toupper_l — 将小写字符转换为大写字符

## 概要

```c
#include <ctype.h>

int toupper(int c);

/* XSI 选项 */
int toupper_l(int c, locale_t locale);
```

## 描述

对于 `toupper()`：
本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 遵从 ISO C 标准。

`toupper()` 和 `toupper_l()` 函数的定义域为 `int` 类型，其值可以表示为 `unsigned char` 或 EOF 的值。如果参数具有任何其他值，则行为是未定义的。

如果 `toupper()` 或 `toupper_l()` 的参数表示一个小写字母，并且在当前语言环境中或由 `locale` 表示的语言环境中（类别 `LC_CTYPE`）分别存在对应的大写字母，则结果应为对应的大写字母。

定义域中的所有其他参数均保持不变返回。

如果 `toupper_l()` 的 `locale` 参数是特殊的语言环境对象 LC_GLOBAL_LOCALE 或不是有效的语言环境对象句柄，则行为是未定义的。

## 返回值

成功完成后，`toupper()` 和 `toupper_l()` 应返回与传入参数对应的大写字母；否则，应返回未更改的参数。

## 错误

未定义错误。

## 示例

无。

## 应用程序用法

无。

## 基本原理

无。

## 未来方向

无。

## 另请参阅

- [`setlocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlocale.html)
- [`uselocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/uselocale.html)
- XBD 7. 语言环境
- [`<ctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html)
- [`<locale.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/locale.h.html)

## 变更历史

首次发布于 Issue 1。派生自 SVID 的 Issue 1。

### Issue 6

标记了超出 ISO C 标准的扩展。

### Issue 7

应用了 SD5-XSH-ERN-181，阐明了返回值部分。

从 The Open Group 技术标准 2006 年扩展 API 集第 4 部分添加了 `toupper_l()` 函数。

应用了 POSIX.1-2008，技术勘误表 1，XSH/TC1-2008/0673 [283] 和 XSH/TC1-2008/0674 [283]。