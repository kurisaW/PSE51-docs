# iscntrl, iscntrl_l - 测试控制字符

## SYNOPSIS

```c
#include <ctype.h>

int iscntrl(int c);

[CX] int iscntrl_l(int c, locale_t locale);
```

## DESCRIPTION

对于 `iscntrl()`：[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 本卷遵循 ISO C 标准。

`iscntrl()` [CX] 和 `iscntrl_l()` 函数应测试 `c` 是否在当前语言环境中 [CX] 或分别由 `locale` 表示的语言环境中属于 **cntrl** 类字符；参见 XBD [7. Locale]。

`c` 参数的类型为 `int`，应用程序应确保其值是一个可表示为 `unsigned char` 的字符，或等于宏 EOF 的值。如果参数具有任何其他值，则行为未定义。

[CX] 如果 `iscntrl_l()` 的 `locale` 参数是特殊的语言环境对象 LC_GLOBAL_LOCALE 或不是有效的语言环境对象句柄，则行为未定义。

## RETURN VALUE

如果 `c` 是控制字符，`iscntrl()` [CX] 和 `iscntrl_l()` 函数应返回非零值；否则，它们应返回 0。

## ERRORS

未定义错误。

## EXAMPLES

无。

## APPLICATION USAGE

为确保应用程序的可移植性，特别是在不同自然语言环境之间，字符分类只能使用这些函数以及 SEE ALSO 部分列出的参考页面中的函数。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`isalnum()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalnum.html)
- [`isalpha()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalpha.html)
- [`isblank()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isblank.html)
- [`isdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isdigit.html)
- [`isgraph()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isgraph.html)
- [`islower()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/islower.html)
- [`isprint()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isprint.html)
- [`ispunct()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ispunct.html)
- [`isspace()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isspace.html)
- [`isupper()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isupper.html)
- [`isxdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isxdigit.html)
- [`setlocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlocale.html)
- [`uselocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/uselocale.html)

XBD [7. Locale], [`<ctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html), [`<locale.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/locale.h.html)

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

更新规范性文本，避免对应用程序要求使用"必须"一词。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 中添加了 `iscntrl_l()` 函数。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0283 [302], XSH/TC1-2008/0284 [283] 和 XSH/TC1-2008/0285 [283]。

---
