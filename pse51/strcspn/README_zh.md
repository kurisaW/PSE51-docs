# strcspn

## 概要

```c
#include <string.h>

size_t strcspn(const char *s1, const char *s2);
```

## 描述

`strcspn()` 函数应该计算由 `s1` 指向的字符串的最大初始段长度（以字节为单位），该段完全由**不**来自 `s2` 指向的字符串的字节组成。

本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵循 ISO C 标准。

在有效输入上，`strcspn()` 函数不应改变 `errno` 的设置。

## 返回值

`strcspn()` 函数应返回计算的 `s1` 指向字符串段的长度；没有保留返回值来表示错误。

## 错误

未定义任何错误。

---

*以下各节为参考信息。*

## 示例

无。

## 应用程序用法

无。

## 原理

无。

## 未来方向

无。

## 参见

- [`strspn()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strspn.html)
- XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## 变更历史

**首次发布于 Issue 1。** 源自 SVID 的 Issue 1。

### Issue 5

RETURN VALUE 部分更新为说明 `strcspn()` 返回 `s1` 的长度，而非之前所述的 `s1` 本身。

### Issue 6

应用了 The Open Group Corrigendum U030/1。RETURN VALUE 部分的文本更新为说明返回的是计算的段长度，而非 `s1` 的长度。

### Issue 8

应用了 Austin Group Defect 448，添加了要求：在有效输入上，`strcspn()` 不改变 `errno` 的设置。

---

