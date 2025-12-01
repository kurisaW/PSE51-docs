# strtoumax

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

strtoimax, strtoumax — 将字符串转换为整数类型

## SYNOPSIS

```c
#include <inttypes.h>

intmax_t strtoimax(const char *restrict nptr,
                   char **restrict endptr,
                   int base);
uintmax_t strtoumax(const char *restrict nptr,
                    char **restrict endptr,
                    int base);
```

## DESCRIPTION

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 本卷遵循 ISO C 标准。

这些函数应等效于 `strtol()`、`strtoll()`、`strtoul()` 和 `strtoull()` 函数，不同之处在于字符串的初始部分应分别转换为 **intmax_t** 和 **uintmax_t** 表示形式。

## RETURN VALUE

这些函数应返回转换后的值（如果有）。

如果无法执行转换，应返回零 [CX] 并可能将 `errno` 设置为 [EINVAL]。

[CX] 如果 `base` 的值不受支持，应返回 0 并将 `errno` 设置为 [EINVAL]。

如果正确的值超出可表示值的范围，应返回 {INTMAX_MAX}、{INTMAX_MIN} 或 {UINTMAX_MAX}（根据返回类型和值的符号，如果有），并将 `errno` 设置为 [ERANGE]。

## ERRORS

这些函数在以下情况下应失败：

- **[EINVAL]** [CX] `base` 的值不受支持。
- **[ERANGE]** 要返回的值无法表示。

这些函数在以下情况下可能失败：

- **[EINVAL]** 无法执行转换。

---

*以下部分为补充信息。*

## EXAMPLES

无。

## APPLICATION USAGE

由于如果 `base` 的值不受支持，`*endptr` 的值是未指定的，应用程序应在调用前确保 `base` 具有受支持的值（0 或 2 到 36 之间），或在检查 `*endptr` 前检查是否出现 [EINVAL] 错误。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`strtol()`](strtol.html)
- [`strtoul()`](strtoul.html)
- XBD [`<inttypes.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/inttypes.h.html)

## CHANGE HISTORY

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

### Issue 7

POSIX.1-2008、Technical Corrigendum 1、XSH/TC1-2008/0613 [453] 和 XSH/TC1-2008/0614 [453] 已应用。

---

*补充信息结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group，保留所有权利