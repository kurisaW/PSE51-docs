# getc_unlocked, getchar_unlocked, putc_unlocked, putchar_unlocked — 带有显式客户端锁定的 stdio 函数

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## 概要 (SYNOPSIS)

```c
#include <stdio.h>

int getc_unlocked(FILE *stream);
int getchar_unlocked(void);
int putc_unlocked(int c, FILE *stream);
int putchar_unlocked(int c);
```

## 描述 (DESCRIPTION)

应提供 `getc()`、`getchar()`、`putc()` 和 `putchar()` 函数的版本，分别命名为 `getc_unlocked()`、`getchar_unlocked()`、`putc_unlocked()` 和 `putchar_unlocked()`，这些版本在功能上等同于原始版本，但它们不需要以完全线程安全的方式实现。当在由 `flockfile()`（或 `ftrylockfile()`）和 `funlockfile()` 保护的范围内使用时，它们应该是线程安全的。这些函数可以安全地用于多线程程序中，当且仅当调用线程拥有 (**FILE \***) 对象时才调用它们，这是在成功调用 `flockfile()` 或 `ftrylockfile()` 函数之后的情况。

如果 `getc_unlocked()` 或 `putc_unlocked()` 作为宏实现，它们可能会多次求值 `stream`，因此 `stream` 参数永远不应是有副作用的表达式。

## 返回值 (RETURN VALUE)

参见 `getc()`、`getchar()`、`putc()` 和 `putchar()`。

## 错误 (ERRORS)

参见 `getc()`、`getchar()`、`putc()` 和 `putchar()`。

---

## 示例 (EXAMPLES)

无。

## 应用程序使用 (APPLICATION USAGE)

由于它们可能作为宏实现，`getc_unlocked()` 和 `putc_unlocked()` 可能错误地处理有副作用的 `stream` 参数。特别是，`getc_unlocked(*f++)` 和 `putc_unlocked(c,*f++)` 不一���按预期工作。因此，在这种情况下使用这些函数之前应适当地添加以下语句：

```c
#undef getc_unlocked
#undef putc_unlocked
```

## 原理说明 (RATIONALE)

出于性能原因，一些 I/O 函数通常作为宏实现（例如 `putc()` 和 `getc()`）。为了安全，它们需要同步，但在每个字符上进行同步通常开销太大。尽管如此，人们认为安全考虑更为重要；因此，`getc()`、`getchar()`、`putc()` 和 `putchar()` 函数被要求为线程安全的。然而，也提供了解锁版本，其名称清楚地表明了操作的不安全性质，但可以用来利用其更高的性能。这些解锁版本只能在使用导出的锁定原语显式锁定的程序区域内安全使用。特别是，像这样的序列：

```c
flockfile(fileptr);
putc_unlocked('1', fileptr);
putc_unlocked('\n', fileptr);
fprintf(fileptr, "Line 2\n");
funlockfile(fileptr);
```

是允许的，并且导致文本序列：

```
1
Line 2
```

被打印而不与其他线程的输出交错。

让标准名称如 `getc()`、`putc()` 等映射到"更快但不安全"而不是"更慢但安全"的版本是错误的。在任何一种情况下，当转换现有代码时，您仍然希望手动检查 `getc()`、`putc()` 等的所有使用。选择安全绑定作为默认值至少会产生正确的代码并保持"函数级别的原子性"不变量。否则会在转换后的代码中引入不必要的同步错误。修改 **stdio** (**FILE \***) 结构或缓冲区的其他例程也被安全地同步。

注意，不需要像 `getc_locked()`、`putc_locked()` 等形式的函数，因为这是 `getc()`、`putc()` 等的功能。使用功能测试宏在 `getc_locked()` 和 `getc_unlocked()` 之间切换 `getc()` 的宏定义是不合适的，因为 ISO C 标准要求存在一个实际函数，其行为不能被功能测试宏改变。此外，同时提供 `xxx_locked()` 和 `xxx_unlocked()` 形式会导致混淆，即后缀是描述函数的行为还是应该使用它的情况。

提供了三个额外的例程 `flockfile()`、`ftrylockfile()` 和 `funlockfile()`（它们可能是宏），允许用户划定一系列同步执行的 I/O 语句。

`ungetc()` 函数相对于其他函数/宏的调用频率较低，因此不需要解锁变体。

## 未来方向 (FUTURE DIRECTIONS)

无。

## 参见 (SEE ALSO)

- [2.5 标准 I/O 流 (Standard I/O Streams)](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
- [flockfile()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/flockfile.html)
- [getc()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getc.html)
- [getchar()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getchar.html)
- [putc()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/putc.html)
- [putchar()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/putchar.html)
- [<stdio.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## 变更历史 (CHANGE HISTORY)

### 首次发布于 Issue 5
包含在内以与 POSIX 线程扩展对齐。

### Issue 6
这些函数被标记为线程安全函数选项的一部分。

应用了 The Open Group 更正 U030/2，添加了应用程序使用部分，描述了应用程序应如何编写以避免函数作为宏实现时的情况。

### Issue 7
`getc_unlocked()`、`getchar_unlocked()`、`putc_unlocked()` 和 `putchar_unlocked()` 函数从线程安全函数选项移至基础规范。

应用了 POSIX.1-2008，技术更正 1，XSH/TC1-2008/0232 [395]、XSH/TC1-2008/0233 [395]、XSH/TC1-2008/0234 [395] 和 XSH/TC1-2008/0235 [14]。

应用了 POSIX.1-2008，技术更正 2，XSH/TC2-2008/0151 [826]。

---

*信息性文本结束。*

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group，保留所有权利