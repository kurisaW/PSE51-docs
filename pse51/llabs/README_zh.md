# llabs

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## 名称

labs, llabs — 返回长整型绝对值

## 概要

```c
#include <stdlib.h>

long labs(long i);
long long llabs(long long i);
```

## 描述

[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`labs()` 函数应计算 **long** 整型操作数 `i` 的绝对值。`llabs()` 函数应计算 **long long** 整型操作数 `i` 的绝对值。如果结果无法表示，则行为是未定义的。

## 返回值

`labs()` 函数应返回 **long** 整型操作数的绝对值。

`llabs()` 函数应返回 **long long** 整型操作数的绝对值。

## 错误

未定义错误。

---

*以下部分为参考信息。*

## 示例

无。

## 应用程序用法

由于 POSIX.1 要求 **long** 和 **long long** 采用二进制补码表示，因此具有最大量级的负整数 {LONG_MIN} 和 {LLONG_MIN} 的绝对值是无法表示的，所以 `labs(LONG_MIN)` 和 `llabs(LLONG_MIN)` 是未定义的。

## 原理

无。

## 未来方向

无。

## 参见

- [abs()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/abs.html)
- XBD [<stdlib.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## 变更历史

### 首次发布于 Issue 4。源自 ISO C 标准。

### Issue 6

为了与 ISO/IEC 9899:1999 标准保持一致，添加了 `llabs()` 函数。

### Issue 7

应用了 SD5-XSH-ERN-152，修正了返回值部分。

### Issue 8

应用了 Austin Group Defect 1108，更改了应用程序用法部分。

*参考信息结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved
[主索引](https://pubs.opengroup.org/onlinepubs/9799919799/mindex.html) | [XBD](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/contents.html) | [XSH](https://pubs.opengroup.org/onlinepubs/9799919799/functions/contents.html) | [XCU](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/contents.html) | [XRAT](https://pubs.opengroup.org/onlinepubs/9799919799/xrat/contents.html)