# memcpy — 内存字节复制

## 概要

```c
#include <string.h>

void *memcpy(void *restrict s1, const void *restrict s2, size_t n);
```

## 描述

[CX] 本参考页所描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。本卷 POSIX.1-2024 遵循 ISO C 标准。

`memcpy()` 函数应将 `n` 个字节从 `s2` 指向的对象复制到 `s1` 指向的对象中。如果在重叠的对象之间进行复制，则行为未定义。

[CX] `memcpy()` 函数在有效输入时不应改变 `errno` 的设置。

## 返回值

`memcpy()` 函数应返回 `s1`；没有保留返回值来指示错误。

## 错误

未定义任何错误。

---

*以下各节为信息性内容。*

## 示例

无。

## 应用用法

`memcpy()` 函数不检查接收内存区域的溢出。

## 基本原理

无。

## 未来方向

无。

## 参见

XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## 变更历史

**首次发布于 Issue 1。** 源自 SVID 的 Issue 1。

### Issue 6

`memcpy()` 原型已更新，以与 ISO/IEC 9899:1999 标准对齐。

### Issue 8

应用了 Austin Group Defect 448，增加了 `memcpy()` 在有效输入时不改变 `errno` 设置的要求。

---

*信息性文本结束。*