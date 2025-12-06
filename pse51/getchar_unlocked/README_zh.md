# getc_unlocked, getchar_unlocked, putc_unlocked, putchar_unlocked — 带有显式客户端锁定的 stdio 函数

## 概要

```c
[CX] #include <stdio.h>

int getc_unlocked(FILE *stream);
int getchar_unlocked(void);
int putc_unlocked(int c, FILE *stream);
int putchar_unlocked(int c);
```

## 描述

应提供分别名为 `getc_unlocked()`、`getchar_unlocked()`、`putc_unlocked()` 和 `putchar_unlocked()` 的 `getc()`、`getchar()`、`putc()` 和 `putchar()` 函数版本，这些函数在功能上与原始版本等价，但它们不需要以完全线程安全的方式实现。当在 `flockfile()`（或 `ftrylockfile()`）和 `funlockfile()` 保护的范围内使用时，它们应该是线程安全的。这些函数可以安全地用于多线程程序中，当且仅当调用线程拥有 (FILE *) 对象时，就像在成功调用 `flockfile()` 或 `ftrylockfile()` 函数之后的情况。

如果 `getc_unlocked()` 或 `putc_unlocked()` 作为宏实现，它们可能多次计算 *stream*，因此 *stream* 参数永远不应该是有副作用的表达式。

## 返回值

参见 `getc()`、`getchar()`、`putc()` 和 `putchar()`。

## 错误

参见 `getc()`、`getchar()`、`putc()` 和 `putchar()`。

---

*以下部分是提供信息的。*

## 示例

无。

## 应用程序用法

由于它们可能作为宏实现，`getc_unlocked()` 和 `putc_unlocked()` 可能会错误处理有副作用的 *stream* 参数。特别是，`getc_unlocked(*f++)` 和 `putc_unlocked(c,*f++)` 不一定按预期工作。因此，在这种情况下使用这些函数之前应适当地执行以下语句：

```c
#undef getc_unlocked
#undef putc_unlocked
```

## 原理

出于性能原因，一些 I/O 函数通常作为宏实现（例如 `putc()` 和 `getc()`）。为了安全，它们需要同步，但在每个字符上进行同步通常太昂贵了。尽管如此，人们认为安全问题更为重要；因此，要求 `getc()`、`getchar()`、`putc()` 和 `putchar()` 函数是线程安全的。然而，也提供了无锁版本，其名称清楚地表明其操作的不安全性质，但可以利用它们的高性能。这些无锁版本只能在显式锁定程序区域内安全使用，使用导出的锁定原语。特别是，像这样的序列：

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

而不会被其他线程的输出穿插。

让像 `getc()`、`putc()` 等标准名称映射到"更快但不安全"而不是"更慢但安全"的版本是错误的。无论哪种情况，在转换现有代码时，您仍然需要手动检查 `getc()`、`putc()` 等的所有使用。至少选择安全绑定作为默认会导致正确的代码，并保持"函数级别的原子性"不变量。否则会在转换的代码中引入不必要的同步错误。其他修改 *stdio* (FILE *) 结构或缓冲区的例程也被安全地同步。

注意，不需要 `getc_locked()`、`putc_locked()` 等形式的函数，因为这就是 `getc()`、`putc()` 等的功能。使用功能测试宏在 `getc_locked()` 和 `getc_unlocked()` 之间切换 `getc()` 的宏定义是不合适的，因为 ISO C 标准要求存在一个实际函数，其行为不能被功能测试宏改变。此外，同时提供 *xxx_locked*() 和 *xxx_unlocked*() 形式会导致关于后缀是描述函数行为还是应使用情况的混淆。

提供了三个额外的例程 `flockfile()`、`ftrylockfile()` 和 `funlockfile()`（它们可能是宏），以允许用户划定同步执行的 I/O 语句序列。

`ungetc()` 函数相对于其他函数/宏的调用频率较低，因此不需要无锁变体。

## 未来方向

无。

## 参见

- [2.5 标准 I/O 流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
- [flockfile()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/flockfile.html)
- [getc()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getc.html)
- [getchar()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getchar.html)
- [putc()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/putc.html)
- [putchar()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/putchar.html)

**XBD** [<stdio.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## 变更历史

**首次发布于 Issue 5。** 为与 POSIX 线程扩展对齐而包含。

### Issue 6

这些函数被标记为线程安全函数选项的一部分。

应用了 The Open Group Corrigendum U030/2，添加了描述应用程序应如何编写以避免函数作为宏实现的情况的应用程序用法。

### Issue 7

`getc_unlocked()`、`getchar_unlocked()`、`putc_unlocked()` 和 `putchar_unlocked()` 函数从线程安全函数选项移动到基础部分。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0232 [395], XSH/TC1-2008/0233 [395], XSH/TC1-2008/0234 [395], 和 XSH/TC1-2008/0235 [14]。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0151 [826]。

*信息性文本结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 IEEE 的商标。
版权所有 © 2001-2024 IEEE 和 The Open Group，保留所有权利