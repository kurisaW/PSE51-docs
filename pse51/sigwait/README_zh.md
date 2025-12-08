# sigwait — 等待排队的信号

## 概要

```c
#include <signal.h>

int sigwait(const sigset_t *restrict set, int *restrict sig);
```

## 描述

`sigwait()` 函数应从 `set` 中选择一个挂起的信号，从系统的挂起信号集合中原子地清除它，并在 `sig` 引用的位置返回该信号编号。如果在调用 `sigwait()` 之前单个信号编号有多个挂起的实例，那么成功返回后该信号编号是否还有剩余挂起信号是由实现定义的。如果实现支持排队信号，并且为所选信号编号排队了多个信号，则第一个此类排队信号应导致 `sigwait()` 返回，其余信号应保持排队状态。如果在调用时 `set` 中没有信号挂起，线程应被挂起，直到一个或多个信号变为挂起状态。`set` 定义的信号在调用 `sigwait()` 时应已被阻塞；否则，行为是未定义的。`sigwait()` 对 `set` 中信号的信号动作的影响是未指定的。

如果多个线程使用 `sigwait()` 等待相同的信号，这些线程中最多只有一个应从 `sigwait()` 返回信号编号。如果超过一个线程在 `sigwait()` 中为某个信号而被阻塞，当该信号为进程生成时，哪个等待线程从 `sigwait()` 返回是未指定的。如果信号为特定线程生成，如通过 `pthread_kill()`，则只有该线程应返回。

如果选择了 SIGRTMIN 到 SIGRTMAX 范围内的多个挂起信号中的任何一个，它应该是编号最低的那个。实时信号和非实时信号之间的选择顺序，或者多个挂起的非实时信号之间的选择顺序是未指定的。

## 返回值

成功完成时，`sigwait()` 应将接收到信号的信号编号存储在 `sig` 引用的位置并返回零。否则，应返回错误编号以指示错误。

## 错误

`sigwait()` 函数可能会失败：

- **[EINVAL]** `set` 参数包含无效或不支持的信号编号。

## 示例

无。

## 应用用法

无。

## 基本原理

为线程提供等待信号的便捷方式，POSIX.1-2024 本卷提供了 `sigwait()` 函数。对于线程必须等待信号的大多数情况，`sigwait()` 函数应该相当方便、高效和足够。

然而，有请求要求提供比 `sigwait()` 更底层的原语以及线程可以使用的信号量。经过一些考虑后，允许线程使用信号量，并且 `sem_post()` 被定义为异步信号安全的。

总之，当需要在响应异步信号运行的代码通知线程时，应使用 `sigwait()` 来处理信号。或者，如果实现提供信号量，它们也可以使用，可以跟在 `sigwait()` 之后使用，或者在之前通过 `sigaction()` 注册的信号处理例程内部使用。

## 未来方向

无。

## 参见

- [信号概念](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_04)
- [实时信号](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_08_01)
- [`pause()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pause.html)
- [`pthread_sigmask()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_sigmask.html)
- [`sigaction()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html)
- [`sigpending()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigpending.html)
- [`sigsuspend()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigsuspend.html)
- [`sigtimedwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigtimedwait.html)
- [`<signal.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)
- [`<time.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/time.h.html)

## 变更历史

首次在 Issue 5 中发布。为与 POSIX 实时扩展和 POSIX 线程扩展保持一致而包含。

### Issue 6

为与 ISO/IEC 9899:1999 标准保持一致，向 `sigwait()` 原型添加了 **restrict** 关键字。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/131，更新描述以说明如果超过一个线程在 `sigwait()` 中被阻塞，哪个等待线程返回是未指定的，并且如果信号为特定线程生成，只有该线程应返回。

### Issue 7

与实时信号扩展选项相关的功能被移至基础规范。

应用 POSIX.1-2008，技术勘误 1，XSH/TC1-2008/0584 [76]。

---
