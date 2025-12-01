# imaxabs — 返回绝对值

## 概要

```c
#include <inttypes.h>

intmax_t imaxabs(intmax_t j);
```

## 描述

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。本 POSIX.1-2024 卷遵循 ISO C 标准。

`imaxabs()` 函数应计算整数 `j` 的绝对值。如果结果无法表示，则行为未定义。

## 返回值

`imaxabs()` 函数应返回绝对值。

## 错误

未定义错误。

---

## 示例

无。

## 应用用法

由于 POSIX.1 要求 `intmax_t` 使用二进制补码表示，具有最大量级的负整数 {INTMAX_MIN} 的绝对值无法表示，因此 `imaxabs(INTMAX_MIN)` 是未定义的。

## 原理

无。

## 未来方向

无。

## 参见

- [`imaxdiv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/imaxdiv.html)
- XBD [`<inttypes.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/inttypes.h.html)

## 变更历史

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

### Issue 8

应用了 Austin Group Defect 1108，更改了 APPLICATION USAGE 部分。