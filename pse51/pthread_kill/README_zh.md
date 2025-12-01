# pthread_kill

**Open Group 基础规范第 8 版**
**IEEE Std 1003.1-2024**
版权所有 © 2001-2024 IEEE 和 The Open Group

## 名称

`pthread_kill` — 向线程发送信号

## 概要

```c
#include <signal.h>

int pthread_kill(pthread_t thread, int sig);
```

## 描述

`pthread_kill()` 函数应请求将信号传递给指定的线程。如果 `thread` 是僵尸线程，这不应被视为错误。

## 返回值

成功完成后，函数应返回零值。否则，函数应返回错误编号。如果 `pthread_kill()` 函数失败，则不会发送任何信号。

## 错误

`pthread_kill()` 函数可能在以下情况下失败：

- **[EINVAL]**
  - `sig` 参数的值为零。

`pthread_kill()` 函数应在以下情况下失败：

- **[EINVAL]**
  - `sig` 参数的值非零，并且是无效或不支持的信号编号。

`pthread_kill()` 函数不应返回 **[EINTR]** 错误代码。

---

*以下章节为参考信息。*

## 示例

无。

## 应用程序用法

`pthread_kill()` 函数提供了一种机制，用于异步地将信号定向到调用进程中的线程。例如，这可以由一个线程用来影响向一组线程广播信号传递。

请注意，`pthread_kill()` 仅导致信号在给定线程的上下文中被处理；信号动作（终止或停止）影响整个进程。

## 原理

如果实现在线程生命周期结束后检测到使用线程 ID，建议函数应失败并报告 **[ESRCH]** 错误。

历史实现在使用表示僵尸线程的线程 ID 调用 `pthread_kill()` 时的结果各不相同。一些实现在此类调用中表示成功，而其他实给出 **[ESRCH]** 错误。由于 POSIX.1-2024 本卷中线程生命周期的定义涵盖了僵尸线程，因此描述的 **[ESRCH]** 错误在这种情况下是不合适的，给出此错误的实现不符合规范。特别是，这意味着应用程序不能通过使用零 `sig` 参数调用 `pthread_kill()` 来让一个线程检查另一个线程的终止，而当 `sig` 为零时，实现可以通过返回 **[EINVAL]** 来表明这是不可能的。

## 未来方向

无。

## 另请参阅

- [`kill()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/kill.html#tag_17_296)
- [`pthread_self()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_self.html#)
- [`raise()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/raise.html#)

XBD [`<signal.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)

## 变更历史

**首次发布于第 5 版。** 为与 POSIX 线程扩展对齐而包含在内。

### 第 6 版

`pthread_kill()` 函数被标记为线程选项的一部分。

添加了应用程序用法章节。

### 第 7 版

`pthread_kill()` 函数从线程选项移动到基础部分。

应用了 Austin Group 解释 1003.1-2001 #142，移除了 **[ESRCH]** 错误条件。

应用了 POSIX.1-2008，技术勘误表 2，XSH/TC2-2008/0277 [765]。

### 第 8 版

应用了 Austin Group 缺陷 792，添加了将僵尸线程的线程 ID 传递给 `pthread_kill()` 不视为错误的要求。

应用了 Austin Group 缺陷 1214，允许当 `sig` 参数为零时 `pthread_kill()` 失败并返回 **[EINVAL]**。

*参考信息结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 IEEE 的商标。
版权所有 © 2001-2024 IEEE 和 The Open Group，保留所有权利

[主索引](https://pubs.opengroup.org/onlinepubs/9799919799/mindex.html) | [XBD](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/contents.html) | [XSH](https://pubs.opengroup.org/onlinepubs/9799919799/functions/contents.html) | [XCU](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/contents.html) | [XRAT](https://pubs.opengroup.org/onlinepubs/9799919799/xrat/contents.html)