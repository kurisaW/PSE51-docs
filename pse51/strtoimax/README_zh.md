# strtoimax, strtoumax — 将字符串转换为整数类型

## 概要

```c
#include <inttypes.h>

intmax_t strtoimax(const char *restrict nptr,
                   char **restrict endptr,
                   int base);

uintmax_t strtoumax(const char *restrict nptr,
                    char **restrict endptr,
                    int base);
```

## 描述

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本 POSIX.1-2024 卷遵循 ISO C 标准。

这些函数应等效于 `strtol()`、`strtoll()`、`strtoul()` 和 `strtoull()` 函数，不同之处在于字符串的初始部分应分别转换为 **intmax_t** 和 **uintmax_t** 表示。

## 返回值

这些函数应返回转换后的值（如果有）。

如果无法执行转换，应返回零 [CX] 且 `errno` 可设置为 [EINVAL]。

[CX] 如果 `base` 的值不受支持，应返回 0 且 `errno` 应设置为 [EINVAL]。

如果正确的值超出可表示值的范围，应返回 {INTMAX_MAX}、{INTMAX_MIN} 或 {UINTMAX_MAX}（根据返回类型和值的符号，如果有），且 `errno` 应设置为 [ERANGE]。

## 错误

这些函数���以下情况下应失败：

- **[EINVAL]** [CX] `base` 的值不受支持。

- **[ERANGE]** 要返回的值不可表示。

这些函数在以下情况下可能失败：

- **[EINVAL]** 无法执行转换。

---

*以下部分为参考信息。*

## 示例

无。

## 应用程序用法

由于如果 `base` 的值不受支持，`*endptr` 的值是未指定的，应用程序应在调用前确保 `base` 具有支持的值（0 或 2 到 36 之间），或者在检查 `*endptr` 之前检查 [EINVAL] 错误。

## 基本原理

无。

## 未来方向

无。

## 参见

- [`strtol()`](strtol.html)
- [`strtoul()`](strtoul.html)
- XBD [`<inttypes.h>`](basedefs/inttypes.h.html)

## 变更历史

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

### Issue 7

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0613 [453] 和 XSH/TC1-2008/0614 [453]。

---

*参考信息结束。*

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
版权所有 © 2001-2024 The IEEE and The Open Group，保留所有权利