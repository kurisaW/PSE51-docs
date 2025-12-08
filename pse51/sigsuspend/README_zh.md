# sigsuspend — 等待信号

## 概要

```c
#include <signal.h>

int sigsuspend(const sigset_t *sigmask);
```

## 描述

`sigsuspend()` 函数应当原子性地执行以下两个操作：将调用线程的当前信号掩码替换为由 `sigmask` 指向的信号集，并挂起线程直到传递一个信号，该信号的动作是要执行信号捕获函数或终止进程。这不应导致可能在进程上挂起的任何其他信号在线程上变为挂起状态。

如果动作是终止进程，那么 `sigsuspend()` 永远不会返回。如果动作是执行信号捕获函数，那么 `sigsuspend()` 应在信号捕获函数返回后返回，并将信号掩码恢复到 `sigsuspend()` 调用之前存在的集合。

无法阻塞不能被忽略的信号。系统强制执行此规则而不会指示错误。

## 返回值

由于 `sigsuspend()` 无限期挂起线程执行，因此没有成功完成的返回值。如果发生返回，应返回 -1 并设置 `errno` 以指示错误。

## 错误

`sigsuspend()` 函数在以下情况下应失败：

- **[EINTR]** - 调用进程捕获了一个信号，并且控制从信号捕获函数返回。

## 应用程序用法

通常，在关键代码段的开始，使用 `sigprocmask()` 函数阻塞指定的信号集。当线程完成关键代码段并且需要等待先前阻塞的信号时，它通过调用 `sigsuspend()` 并使用 `sigprocmask()` 调用返回的掩码来暂停执行。

## 原理说明

希望避免线程取消处理器的信号掩码歧义的代码可以安装额外的取消处理器，将信号掩码重置为预期值。

```c
void cleanup(void *arg)
{
    sigset_t *ss = (sigset_t *) arg;
    pthread_sigmask(SIG_SETMASK, ss, NULL);
}

int call_sigsuspend(const sigset_t *mask)
{
    sigset_t oldmask;
    int result;
    pthread_sigmask(SIG_SETMASK, NULL, &oldmask);
    pthread_cleanup_push(cleanup, &oldmask);
    result = sigsuspend(mask);
    pthread_cleanup_pop(0);
    return result;
}
```

## 参见

- [2.4 信号概念](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_04)
- [pause()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pause.html)
- [sigaction()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html)
- [sigaddset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaddset.html)
- [sigdelset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigdelset.html)
- [sigemptyset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigemptyset.html)
- [sigfillset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigfillset.html)
- [<signal.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)

## 变更历史

### 首次发布于 Issue 3
为与 POSIX.1-1988 标准对齐而包含。

### Issue 5
更新了描述以与 POSIX 线程扩展对齐。

### Issue 6
- 返回值部分中的文本已从"suspends process execution"（挂起进程执行）更改为"suspends thread execution"（挂起线程执行）。这反映了 IEEE PASC 解释 1003.1c #40。
- 应用程序用法部分中的文本已被替换。
- 由于此函数在 <signal.h> 头文件中的存在是对 ISO C 标准的扩展，因此 SYNOPSIS 被标记为 CX。

### Issue 7
应用了 SD5-XSH-ERN-122，在原理说明中添加了示例代码。

### Issue 8
- 应用了 Austin Group 缺陷 1201，阐明了 `sigsuspend()` 的原子性要求。
- 应用了 Austin Group 缺陷 1223，更改了原理说明中的示例代码。

---
