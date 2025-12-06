# isspace, isspace_l — 测试空白字符

## SYNOPSIS

```c
#include <ctype.h>

int isspace(int c);

[CX] int isspace_l(int c, locale_t locale);
```

## DESCRIPTION

对于 `isspace()`：[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`isspace()` [CX] 和 `isspace_l()` 函数应测试 `c` 是否在当前区域环境中属于 **space** 字符类别，[CX] 或分别在由 `locale` 表示的区域环境中属于该字符类别；参见 XBD [7. Locale](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07)。

`c` 参数是一个 `int`，应用程序应确保其值是一个可表示为 `unsigned char` 的字符或等于宏 EOF 的值。如果参数具有任何其他值，则行为未定义。

[CX] 如果 `isspace_l()` 的 `locale` 参数是特殊的区域对象 LC_GLOBAL_LOCALE 或不是有效的区域对象句柄，则行为未定义。

## RETURN VALUE

如果 `c` 是空白字符，`isspace()` [CX] 和 `isspace_l()` 函数应返回非零值；否则应返回 0。

## ERRORS

未定义错误。

## EXAMPLES

无。

## APPLICATION USAGE

为确保应用程序的可移植性，特别是在不同自然语言之间，应仅使用这些函数和 SEE ALSO 部分列出的参考页中的函数进行字符分类。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

[`isalnum()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalnum.html), [`isalpha()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalpha.html), [`iscntrl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/iscntrl.html), [`isdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isdigit.html), [`isgraph()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isgraph.html), [`islower()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/islower.html), [`isprint()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isprint.html), [`ispunct()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ispunct.html), [`isupper()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isupper.html), [`isxdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isxdigit.html), [`setlocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlocale.html), [`uselocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/uselocale.html)

XBD [7. Locale](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07), [`<ctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html), [`<locale.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/locale.h.html)

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

更新规范性文本以避免对应用程序要求使用术语"must"。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 添加了 `isspace_l()` 函数。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0303 [302], XSH/TC1-2008/0304 [283], 和 XSH/TC1-2008/0305 [283]。

---
