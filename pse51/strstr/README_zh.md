# strstr — 查找子字符串

## 概要

```c
#include <string.h>

char *strstr(const char *s1, const char *s2);
```

## 描述

[CX] 本参考页面所描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。本 POSIX.1-2024 版本遵循 ISO C 标准。

`strstr()` 函数应在 `s1` 所指向的字符串中定位 `s2` 所指向的字符串的字节序列（不包括终止的 NUL 字符）的第一次出现。

[CX] `strstr()` 函数在有效输入时不应改变 `errno` 的设置。

## 返回值

成功完成时，`strstr()` 应返回指向所定位字符串的指针；如果未找到该字符串，则返回空指针。

如果 `s2` 指向长度为零的字符串，则函数应返回 `s1`。

## 错误

未定义错误。

---

## 示例

无。

## 应用用法

无。

## 原理

无。

## 未来方向

无。

## 参见

- [`memmem()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/memmem.html)
- [`strchr()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strchr.html)
- XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## 变更历史

首次发布于 Issue 3。为与 ANSI C 标准保持一致而包含在内。

### Issue 8

应用了 Austin Group Defect 448，增加了 `strstr()` 在有效输入时不改变 `errno` 设置的要求。

应用了 Austin Group Defect 1061，在 参见 部分增加了 [`memmem()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/memmem.html)。