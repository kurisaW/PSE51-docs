# signal — 信号管理

## 概要

```c
#include <signal.h>

void (*signal(int sig, void (*func)(int)))(int);
```

## 描述

`signal()` 函数选择三种方式之一来处理后续接收到的信号编号 `sig`。如果 `func` 的值为 SIG_DFL，则对该信号进行默认处理。如果 `func` 的值为 SIG_IGN，则该信号将被忽略。否则，应用程序应确保 `func` 指向一个当该信号发生时将被调用的函数。由于信号而调用这样的函数，或者（递归地）调用该调用所调用的任何其他函数（标准库中的函数除外），被称为"信号处理器"。

当信号发生且 `func` 指向一个函数时，是执行等价于：

```c
signal(sig, SIG_DFL);
```

的语句，还是实现阻止某些实现定义的信号集合（至少包括 `sig`）在当前信号处理完成之前发生，这是由实现定义的。（如果 `sig` 的值为 SIGILL，实现也可以选择定义不采取任何操作。）接着执行等价于：

```c
(*func)(sig);
```

的语句。如果函数返回时，`sig` 的值为 SIGFPE、SIGILL 或 SIGSEGV 或任何其他对应于计算异常的实现定义值，则行为是未定义的。否则，程序应在被中断的点恢复执行。

如果进程是多线程的，或者进程是单线程的且信号处理器不是作为以下操作的结果执行：

- 进程调用 `abort()`、`raise()`、`kill()`、`pthread_kill()` 或 `sigqueue()` 生成一个未阻塞的信号
- 一个待处理的信号被解除阻塞，并在解除阻塞的调用返回之前被传递

则在以下情况下行为是未定义的：

[其他行为条件在完整规范中继续...]

在程序启动时，对某些信号执行等价于：

```c
signal(sig, SIG_IGN);
```

的语句，对所有其他信号执行等价于：

```c
signal(sig, SIG_DFL);
```

的语句（参见 `exec`）。

如果 `signal()` 函数成功，不应改变 `errno` 的设置。

要求 `signal()` 函数是线程安全的。

## 返回值

如果请求可以被满足，`signal()` 应返回指定信号 `sig` 的最近一次 `signal()` 调用的 `func` 值。否则，应返回 SIG_ERR 并在 `errno` 中存储一个正值。

## 错误

如果出现以下情况，`signal()` 函数应失败：

- **[EINVAL]** `sig` 参数不是有效的信号编号，或者试图捕获无法捕获的信号，或忽略无法忽略的信号。

如果出现以下情况，`signal()` 函数可能失败：

- **[EINVAL]** 对于无法捕获或忽略（或两者都不可）的信号，试图将其操作设置为 SIG_DFL。

## 应用程序使用

`sigaction()` 函数提供了更全面和可靠的信号控制机制；新应用程序应使用 `sigaction()` 而不是 `signal()`。

## 原理

ISO C 标准规定，在多线程程序中使用 `signal()` 会导致未定义的行为。然而，在线程被添加到 ISO C 标准之前，POSIX.1 就已经要求 `signal()` 是线程安全的。

## 参见

- [2.4 信号概念](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_04)
- [exec](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html)
- [pause()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pause.html)
- [raise()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/raise.html)
- [sigaction()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html)
- [sigsuspend()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigsuspend.html)
- [waitid()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/waitid.html)
- [<signal.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 5
从 X/OPEN UNIX 扩展移至 BASE。

### Issue 6
标记了超出 ISO C 标准的扩展。

### Issue 7
应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0580 [275], XSH/TC1-2008/0581 [66], 和 XSH/TC1-2008/0582 [105]。
应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0331 [785]。

### Issue 8
应用了 Austin Group Defect 728，减少了信号处理器引用具有静态或线程存储持续时间的对象时导致未定义行为的情况集合。
应用了 Austin Group Defect 1302，使该函数与 ISO/IEC 9899:2018 标准保持一致。

---

*来源：The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*