# strncat — 将一个字符串的部分内容连接到另一个字符串末尾

## 概要

```c
#include <string.h>

char *strncat(char *restrict s1, const char *restrict s2, size_t n);
```

## 描述

`strncat()` 函数应将 `s2` 指向的数组中的不超过 `n` 个字节（不附加 NUL 字符及其后续字节）附加到 `s1` 指向的字符串末尾。`s2` 的初始字节覆盖 `s1` 末尾的 NUL 字符。结果总是附加一个终止的 NUL 字符。如果在重叠的对象之间进行复制，则行为是未定义的。

`strncat()` 函数在有效输入时不应改变 `errno` 的设置。

## 返回值

`strncat()` 函数应返回 `s1`；没有保留返回值来指示错误。

## 错误

未定义错误。

## 示例

无。

## 应用程序用法

无。

## 原理

无。

## 未来方向

无。

## 参见

- [`strcat()`](strcat.html)
- [`strlcat()`](strlcat.html)

- XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## 变更历史

### 首次发布于 Issue 1。
源自 SVID 的 Issue 1。

### Issue 6
`strncat()` 原型已更新，以与 ISO/IEC 9899:1999 标准对齐。

### Issue 8
应用了 Austin Group 缺陷 448，增加了 `strncat()` 在有效输入时不改变 `errno` 设置的要求。

应用了 Austin Group 缺陷 986，在 SEE ALSO 部分添加了 [`strlcat()`](strlcat.html)。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*