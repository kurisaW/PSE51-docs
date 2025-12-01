# strspn — 获取子字符串长度

## 概要

```c
#include <string.h>

size_t strspn(const char *s1, const char *s2);
```

## 描述

`strspn()` 函数应计算由 `s1` 所指向的字符串的最大初始段的长度（以字节为单位），该段完全由来自 `s2` 所指向字符串的字节组成。

`strspn()` 函数在有效输入时不改变 `errno` 的设置。

## 返回值

`strspn()` 函数应返回计算的长度；没有保留返回值来指示错误。

## 错误

未定义错误。

## 应用用法

无。

## 原理

无。

## 未来方向

无。

## 参见

- `strcspn()`
- `<string.h>`

## 变更历史

**首次发布于 Issue 1。** 源自 SVID 的 Issue 1。

**Issue 5：** 更新了返回值部分，指明 `strspn()` 返回 `s` 的长度，而不是像之前所说的返回 `s` 本身。

**Issue 7：** 应用了 SD5-XSH-ERN-182。

**Issue 8：** 应用了 Austin Group Defect 448，添加了 `strspn()` 在有效输入时不改变 `errno` 设置的要求。