# ispunct, ispunct_l — 测试标点符号字符

## SYNOPSIS

```c
#include <ctype.h>

int ispunct(int c);

[CX] int ispunct_l(int c, locale_t locale);
```

## DESCRIPTION

对于 `ispunct()`：[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`ispunct()` [CX] 和 `ispunct_l()` 函数应测试 `c` 是否为当前区域设置中的 **punct** 字符类别，[CX] 或分别为 `locale` 所代表的区域设置中的 **punct** 字符类别；参见 XBD [7. 区域设置](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07)。

`c` 参数是一个 `int`，应用程序应确保其值是可表示为 `unsigned char` 的字符或等于宏 EOF 的值。如果参数具有任何其他值，则行为未定义。

[CX] 如果 `ispunct_l()` 的 `locale` 参数是特殊区域设置对象 LC_GLOBAL_LOCALE 或不是有效的区域设置对象句柄，则行为未定义。

## RETURN VALUE

如果 `c` 是标点符号，`ispunct()` [CX] 和 `ispunct_l()` 函数应返回非零值；否则，它们应返回 0。

## ERRORS

未定义任何错误。

---

*以下章节为参考信息。*

## EXAMPLES

无。

## APPLICATION USAGE

为确保应用程序的可移植性，特别是在不同自然语言之间，应仅使用这些函数和 SEE ALSO 章节中列出的参考页中的函数进行字符分类。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`isalnum()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalnum.html)
- [`isalpha()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalpha.html)
- [`isblank()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isblank.html)
- [`iscntrl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/iscntrl.html)
- [`isdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isdigit.html)
- [`isgraph()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isgraph.html)
- [`islower()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/islower.html)
- [`isprint()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isprint.html)
- [`isspace()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isspace.html)
- [`isupper()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isupper.html)
- [`isxdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isxdigit.html)
- [`setlocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlocale.html)
- [`uselocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/uselocale.html)

XBD [7. 区域设置](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap07.html#tag_07), [`<ctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html), [`<locale.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/locale.h.html)

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

规范性文本已更新，以避免在应用程序要求中使用"必须"一词。

### Issue 7

`ispunct_l()` 函数从 The Open Group Technical Standard, 2006, Extended API Set Part 4 中添加。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0300 [302], XSH/TC1-2008/0301 [283], 和 XSH/TC1-2008/0302 [283]。

