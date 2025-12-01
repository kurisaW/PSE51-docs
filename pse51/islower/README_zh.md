# islower, islower_l - 测试小写字母

## SYNOPSIS

```c
#include <ctype.h>

int islower(int c);

/* XSI 扩展 */
int islower_l(int c, locale_t locale);
```

## DESCRIPTION

对于 `islower()`：本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 的这一卷遵循 ISO C 标准。

`islower()` 和 `islower_l()` 函数应测试 `c` 是否分别为当前区域设置或由 `locale` 表示的区域设置中的 **lower** 类字符；参见 XBD 7. Locale。

`c` 参数是一个 `int`，其值应为应用程序可确保表示为 `unsigned char` 的字符或等于宏 EOF 的值。如果参数具有任何其他值，则行为未定义。

如果 `islower_l()` 的 `locale` 参数是特殊区域设置对象 LC_GLOBAL_LOCALE 或不是有效的区域设置对象句柄，则行为未定义。

## RETURN VALUE

如果 `c` 是小写字母，`islower()` 和 `islower_l()` 函数应返回非零值；否则，它们应返回 0。

## ERRORS

未定义错误。

## EXAMPLES

### 测试小写字母

下面是两个示例，第一个使用 `islower()`，第二个使用多个并发区域设置和 `islower_l()`。

这些示例基于当前区域设置测试值是否为小写字母，然后将其用作键值的一部分。

```c
/* 示例 1 -- 使用 islower() */
#include <ctype.h>
#include <stdlib.h>
#include <locale.h>
...
char *keystr;
int elementlen, len;
unsigned char c;
...
setlocale(LC_ALL, "");
...
len = 0;
while (len < elementlen) {
    c = (unsigned char) (rand() % 256);
...
    if (islower(c))
        keystr[len++] = c;
    }
...
```

```c
/* 示例 2 -- 使用 islower_l() */
#include <ctype.h>
#include <stdlib.h>
#include <locale.h>
...
char *keystr;
int elementlen, len;
unsigned char c;
...
locale_t loc = newlocale (LC_ALL_MASK, "", (locale_t) 0);
...
len = 0;
while (len < elementlen) {
    c = (unsigned char) (rand() % 256);
...
    if (islower_l(c, loc))
        keystr[len++] = c;
    }
...
```

## APPLICATION USAGE

为确保应用程序的可移植性，特别是在不同语言之间，只能使用这些函数和 SEE ALSO 部分列出的参考页面中的函数进行字符分类。

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
- [`isprint()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isprint.html)
- [`ispunct()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ispunct.html)
- [`isspace()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isspace.html)
- [`isupper()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isupper.html)
- [`isxdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isxdigit.html)
- [`setlocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlocale.html)
- [`uselocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/uselocale.html)

XBD 7. Locale, [`<ctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html), [`<locale.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/locale.h.html)

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

规范性文本已更新，避免在应用程序要求中使用术语"must"，并添加了示例。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 添加了 `islower_l()` 函数。

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0292 [302], XSH/TC1-2008/0293 [283], XSH/TC1-2008/0294 [283], XSH/TC1-2008/0295 [302], and XSH/TC1-2008/0296 [304] 已应用。