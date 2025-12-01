# strlen, strnlen — 获取固定大小字符串的长度

## 概要

```c
#include <string.h>

size_t strlen(const char *s);

[CX] size_t strnlen(const char *s, size_t maxlen);
```

## 描述

对于 `strlen()`：[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`strlen()` 函数应计算 `s` 指向的字符串中的字节数，不包括终止的 NUL 字符。

[CX] `strnlen()` 函数应计算 `s` 指向的数组中的字节数（不包括任何终止 NUL 字符）与 `maxlen` 参数值中的较小者。`strnlen()` 函数决不应检查超过 `s` 指向的数组的前 `maxlen` 个字节。

[CX] `strlen()` 和 `strnlen()` 函数在有效输入时不应更改 `errno` 的设置。

## 返回值

`strlen()` 函数应返回 `s` 的长度；不保留任何返回值来指示错误。

[CX] `strnlen()` 函数应返回 `s` 指向的数组中第一个空字节之前的字节数，如果 `s` 在前 `maxlen` 个字节内包含空字节；否则，它应返回 `maxlen`。

## 错误

未定义任何错误。

## 示例

### 获取字符串长度

以下示例使用 `strlen()` 获取 `key` 和 `data` 字符串的长度，以设置它们的最大长度。

```c
#include <string.h>
...
struct element {
    char *key;
    char *data;
};
...
char *key, *data;
int len;

*keylength = *datalength = 0;
...
if ((len = strlen(key)) > *keylength)
    *keylength = len;
if ((len = strlen(data)) > *datalength)
    *datalength = len;
...
```

## 应用用法

无。

## 基本原理

无。

## 未来方向

无。

## 参见

- [`strlcat()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strlcat.html)
- [`wcslen()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcslen.html)

XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 5

RETURN VALUE 部分更新为指明 `strlen()` 返回 `s` 的长度，而不是先前所述的 `s` 本身。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 1 添加了 `strnlen()` 函数。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0344 [560]。

### Issue 8

应用了 Austin Group Defect 448，添加了 `strlen()` 和 `strnlen()` 在有效输入时不更改 `errno` 设置的要求。

应用了 Austin Group Defect 986，将 [`strlcat()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strlcat.html) 添加到 SEE ALSO 部分。