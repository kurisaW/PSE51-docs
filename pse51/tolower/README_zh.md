# tolower, tolower_l — 将大写字符转换为小写字符

## SYNOPSIS（概要）

```c
#include <ctype.h>

int tolower(int c);

[CX] int tolower_l(int c, locale_t locale);
```

## DESCRIPTION（描述）

对于 `tolower()`：[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 标准遵循 ISO C 标准。

`tolower()` [CX] 和 `tolower_l()` 函数的域为 `int` 类型，其值可以表示为 `unsigned char` 或 EOF 的值。如果参数具有任何其他值，则行为未定义。如果 `tolower()` [CX] 或 `tolower_l()` 的参数表示一个大写字母，并且在当前区域 [CX] 或由 `locale` 表示的区域（类别 `LC_CTYPE`）中分别存在相应的小写字母（如字符类型信息所定义），则结果应为相应的小写字母。域中所有其他参数均原样返回。

[CX] 如果 `tolower_l()` 的 `locale` 参数是特殊的区域对象 LC_GLOBAL_LOCALE 或不是有效的区域对象句柄，则行为未定义。

## RETURN VALUE（返回值）

成功完成后，`tolower()` [CX] 和 `tolower_l()` 函数应返回与传入参数相对应的小写字母；否则，它们应原样返回该参数。

## ERRORS（错误）

未定义错误。

## EXAMPLES（示例）

无。

## APPLICATION USAGE（应用程序使用）

无。

## RATIONALE（基本原理）

无。

## FUTURE DIRECTIONS（未来方向）

无。

## SEE ALSO（参见）

- `setlocale()`
- `uselocale()`
- XBD 7. Locale（区域设置）
- `<ctype.h>`
- `<locale.h>`

## CHANGE HISTORY（变更历史）

首次在 Issue 1 中发布。源自 SVID 的 Issue 1。

### Issue 6

标记了超出 ISO C 标准的扩展。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 中添加了 `tolower_l()` 函数。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0671 [283] 和 XSH/TC1-2008/0672 [283]。

---
