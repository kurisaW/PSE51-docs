# putc_unlocked

## SYNOPSIS

```c
#include <stdio.h>

int getc_unlocked(FILE *stream);
int getchar_unlocked(void);
int putc_unlocked(int c, FILE *stream);
int putchar_unlocked(int c);
```

## DESCRIPTION

提供的函数版本 getc_unlocked()、getchar_unlocked()、putc_unlocked() 和 putchar_unlocked() 分别是 getc()、getchar()、putc() 和 putchar() 函数的对应版本，功能上与原始版本等效，但它们不要求以完全线程安全的方式实现。当在受 flockfile()（或 ftrylockfile()）和 funlockfile() 保护的范围内使用时，它们应该是线程安全的。这些函数可以在多线程程序中安全使用，当且仅当在调用线程拥有 (FILE *) 对象时调用它们，就像成功调用 flockfile() 或 ftrylockfile() 函数之后的情况。

如果 getc_unlocked() 或 putc_unlocked() 作为宏实现，它们可能会多次计算 stream 参数，因此 stream 参数永远不应该是有副作用的表达式。

## RETURN VALUE

参见 getc()、getchar()、putc() 和 putchar()。

## ERRORS

参见 getc()、getchar()、putc() 和 putchar()。

---

*以下部分为补充信息。*

## EXAMPLES

无。

## APPLICATION USAGE

由于它们可能作为宏实现，getc_unlocked() 和 putc_unlocked() 可能错误地处理有副作用的 stream 参数。特别是，getc_unlocked(*f++) 和 putc_unlocked(c,*f++) 不一定能按预期工作。因此，在这种情况下使用这些函数之前，应根据需要使用以下语句：

```c
#undef getc_unlocked
#undef putc_unlocked
```

## RATIONALE

出于性能原因，某些 I/O 函数通常实现为宏（例如，putc() 和 getc()）。为了安全，它们需要同步，但为每个字符进行同步往往开销过大。尽管如此，仍认为安全问题是更重要的；因此，要求 getc()、getchar()、putc() 和 putchar() 函数是线程安全的。然而，也提供了名称中明确指示其操作不安全但可以利用其更高性能的 unlocked 版本。这些 unlocked 版本只能在使用导出的锁定原语的显式锁定程序区域内安全使用。特别是，如下序列：

```c
flockfile(fileptr);
putc_unlocked('1', fileptr);
putc_unlocked('\n', fileptr);
fprintf(fileptr, "Line 2\n");
funlockfile(fileptr);
```

是允许的，并且输出结果为：

```
1
Line 2
```

不会被其他线程的输出插断。

将 getc()、putc() 等标准名称映射到"更快但不安全"版本而非"更慢但安全"版本是错误的。无论哪种情况，在转换现有代码时，您仍然希望手动检查 getc()、putc() 等的所有使用。至少选择安全绑定作为默认，会生成正确的代码并保持"函数级原子性"不变性。否则会在转换后的代码中引入不必要的同步错误。其他修改 stdio (FILE *) 结构或缓冲区的例程也进行了安全同步。

请注意，不需要 getc_locked()、putc_locked() 等形式的函数，因为这就是 getc()、putc() 等的功能。使用特性测试宏在 getc_locked() 和 getc_unlocked() 之间切换 getc() 的宏定义是不合适的，因为 ISO C 标准要求存在实际函数，其行为无法被特性测试宏改变。此外，同时提供 xxx_locked() 和 xxx_unlocked() 形式会导致混淆，即后缀是描述函数的行为还是应该使用的环境。

提供了三个额外的例程：flockfile()、ftrylockfile() 和 funlockfile（它们可能是宏），允许用户勾勒出同步执行的 I/O 语句序列。

ungetc() 函数相对于其他函数/宏的调用频率较低，因此不需要 unlocked 变体。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [2.5 标准 I/O 流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
- [flockfile()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/flockfile.html)
- [getc()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getc.html)
- [getchar()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getchar.html)
- [putc()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/putc.html)
- [putchar()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/putchar.html)
- XBD [<stdio.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY

首次在 Issue 5 中发布。为了与 POSIX 线程扩展对齐而包含在内。

### Issue 6

这些函数被标记为线程安全函数选项的一部分。

应用了 The Open Group 勘误 U030/2，添加了描述应用程序应如何编写以避免函数作为宏实现的情况的 APPLICATION USAGE 部分。

### Issue 7

getc_unlocked()、getchar_unlocked()、putc_unlocked() 和 putchar_unlocked() 函数从线程安全函数选项移动到基础规范。

应用了 POSIX.1-2008 技术勘误 1：XSH/TC1-2008/0232 [395]、XSH/TC1-2008/0233 [395]、XSH/TC1-2008/0234 [395] 和 XSH/TC1-2008/0235 [14]。

应用了 POSIX.1-2008 技术勘误 2：XSH/TC2-2008/0151 [826]。

---

*补充信息结束。*

***

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved