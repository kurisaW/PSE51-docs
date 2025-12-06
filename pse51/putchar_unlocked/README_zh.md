# putchar_unlocked

## SYNOPSIS（概要）

```c
#include <stdio.h>

int getc_unlocked(FILE *stream);
int getchar_unlocked(void);
int putc_unlocked(int c, FILE *stream);
int putchar_unlocked(int c);
```

## DESCRIPTION（描述）

应提供分别名为 `getc_unlocked()`、`getchar_unlocked()`、`putc_unlocked()` 和 `putchar_unlocked()` 的 `getc()`、`getchar()`、`putc()` 和 `putchar()` 函数版本，这些版本在功能上与原始版本等价，但例外情况是它们不需要以完全线程安全的方式实现。当在由 `flockfile()`（或 `ftrylockfile()`）和 `funlockfile()` 保护的范围内使用时，它们应该是线程安全的。当且仅当调用线程拥有 (FILE *) 对象时（就像成功调用 `flockfile()` 或 `ftrylockfile()` 函数后的情况），这些函数可以安全地在多线程程序中使用。

如果 `getc_unlocked()` 或 `putc_unlocked()` 作为宏实现，它们可能会多次计算stream参数，因此stream参数永远不应该是有副作用的表达式。

## RETURN VALUE（返回值）

参见 [`getc()`](getc.html)、[`getchar()`](getchar.html)、[`putc()`](putc.html) 和 [`putchar()`](putchar.html)。

## ERRORS（错误）

参见 [`getc()`](getc.html)、[`getchar()`](getchar.html)、[`putc()`](putc.html) 和 [`putchar()`](putchar.html)。

---

*以下部分为补充信息。*

## EXAMPLES（示例）

无。

## APPLICATION USAGE（应用程序使用）

由于它们可能作为宏实现，`getc_unlocked()` 和 `putc_unlocked()` 可能会错误地处理有副作用的stream参数。特别是，`getc_unlocked(*f++)` 和 `putc_unlocked(c,*f++)` 不一定会按预期工作。因此，在这种情况下使用这些函数前，应视情况适当执行以下语句：

```c
#undef getc_unlocked
#undef putc_unlocked
```

## RATIONALE（基本原理）

出于性能原因，一些I/O函数通常作为宏实现（例如，`putc()` 和 `getc()`）。为了安全，它们需要同步，但在每个字符上进行同步往往开销太大。尽管如此，安全考虑被认为更重要；因此，`getc()`、`getchar()`、`putc()` 和 `putchar()` 函数被要求是线程安全的。然而，也提供了名称明确指示其操作不安全性质的未锁定版本，但可以利用它们的高性能。这些未锁定版本只能在明确锁定的程序区域内安全使用，使用导出的锁定原语。特别是，如下序列：

```c
flockfile(fileptr);
putc_unlocked('1', fileptr);
putc_unlocked('\n', fileptr);
fprintf(fileptr, "Line 2\n");
funlockfile(fileptr);
```

是允许的，并且会输出文本序列：

```
1
Line 2
```

而不会被其他线程的输出所穿插。

让像 `getc()`、`putc()` 等标准名称映射到"更快但不安全"的版本而不是"更慢但安全"的版本是错误的。无论哪种情况，在转换现有代码时，您仍然希望手动检查所有 `getc()`、`putc()` 等的使用。至少选择安全绑定作为默认值会产生正确的代码，并保持"函数原子性"不变量。否则会在转换后的代码中引入不必要的同步错误。其他修改stdio (FILE *) 结构或缓冲区的例程也被安全地同步。

注意，不需要像 `getc_locked()`、`putc_locked()` 等形式的函数，因为这是 `getc()`、`putc()` 等的功能。使用特性测试宏来在 `getc_locked()` 和 `getc_unlocked()` 之间切换 `getc()` 的宏定义是不合适的，因为ISO C标准要求存在实际函数，函数的行为不能被特性测试宏改变。此外，同时提供 xxx_locked() 和 xxx_unlocked() 形式会导致混淆：后缀是描述函数的行为还是应该使用它的环境。

提供了三个额外的例程，`flockfile()`、`ftrylockfile()` 和 `funlockfile()`（它们可能是宏），允许用户勾勒出同步执行的I/O语句序列。

`ungetc()` 函数相对于其他函数/宏的调用频率较低，因此不需要未锁定变体。

## FUTURE DIRECTIONS（未来方向）

无。

## SEE ALSO（参见）

* [2.5 标准I/O流](V2_chap02.html#tag_16_05)
* [`flockfile()`](flockfile.html)
* [`getc()`](getc.html)
* [`getchar()`](getchar.html)
* [`putc()`](putc.html)
* [`putchar()`](putchar.html)

XBD [<stdio.h>](../basedefs/stdio.h.html)

## CHANGE HISTORY（变更历史）

### 首次发布于Issue 5。为与POSIX线程扩展对齐而包含。

### Issue 6

这些函数被标记为线程安全函数选项的一部分。

应用The Open Group Corrigendum U030/2，添加了描述应用程序应如何编写以避免函数作为宏实现情况的应用程序使用说明。

### Issue 7

`getc_unlocked()`、`getchar_unlocked()`、`putc_unlocked()` 和 `putchar_unlocked()` 函数从线程安全函数选项移至基础规范。

应用POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0232 [395], XSH/TC1-2008/0233 [395], XSH/TC1-2008/0234 [395], 和 XSH/TC1-2008/0235 [14]。

应用POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0151 [826]。

*补充信息结束。*

---

UNIX® 是The Open Group的注册商标。
POSIX™ 是IEEE的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved