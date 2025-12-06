# isprint, isprint_l — 测试可打印字符

## SYNOPSIS

```c
#include <ctype.h>

int isprint(int c);

/* [CX] 扩展 */
int isprint_l(int c, locale_t locale);
```

## DESCRIPTION

对于 `isprint()`：
[本参考页所描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非预期的。本卷 POSIX.1-2024 遵从 ISO C 标准。]

`isprint()` 和 `isprint_l()` 函数应测试 `c` 是否分别为当前语言环境或由 `locale` 表示的语言环境中的 **print** 类字符；参见 XBD [7. Locale]。

`c` 参数是一个 `int`，其值应��应用程序确保是可表示为 `unsigned char` 的字符或等于宏 EOF 的值。如果参数具有任何其他值，则行为未定义。

[如果 `isprint_l()` 的 `locale` 参数是特殊语言环境对象 LC_GLOBAL_LOCALE 或不是有效的语言环境对象句柄，则行为未定义。]

## RETURN VALUE

如果 `c` 是可打印字符，`isprint()` 和 `isprint_l()` 函数应返回非零值；否则，它们应返回 0。

## ERRORS

未定义任何错误。

---

## APPLICATION USAGE

为确保应用程序的可移植性，特别是在不同自然语言之间，应仅使用这些函数和 SEE ALSO 部分所列参考页面中的函数进行字符分类。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`isalnum()`](isalnum.html)
- [`isalpha()`](isalpha.html)
- [`isblank()`](isblank.html)
- [`iscntrl()`](iscntrl.html)
- [`isdigit()`](isdigit.html)
- [`isgraph()`](isgraph.html)
- [`islower()`](islower.html)
- [`ispunct()`](ispunct.html)
- [`isspace()`](isspace.html)
- [`isupper()`](isupper.html)
- [`isxdigit()`](isxdigit.html)
- [`setlocale()`](setlocale.html)
- [`uselocale()`](uselocale.html)

XBD [7. Locale], [`<ctype.h>`](ctype.h), [`<locale.h>`](locale.h)

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

规范文本已更新，以避免在应用程序要求中使用"必须"一词。

### Issue 7

`isprint_l()` 函数从 The Open Group Technical Standard, 2006, Extended API Set Part 4 中添加。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0297 [302], XSH/TC1-2008/0298 [283], 和 XSH/TC1-2008/0299 [283]。

---
