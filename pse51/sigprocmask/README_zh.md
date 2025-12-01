# sigprocmask

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

`pthread_sigmask`, `sigprocmask` — 检查和更改阻塞信号

## SYNOPSIS

```c
#include <signal.h>

int pthread_sigmask(int how, const sigset_t *restrict set,
                   sigset_t *restrict oset);
int sigprocmask(int how, const sigset_t *restrict set,
                sigset_t *restrict oset);
```

## DESCRIPTION

`pthread_sigmask()` 函数应检查或更改（或两者兼有）调用线程的信号掩码。

如果参数 `set` 不是空指针，它指向用于更改当前阻塞信号集的信号集。

参数 `how` 指示更改信号集的方式，应用程序应确保其由以下值之一组成：

**SIG_BLOCK**
结果集应为当前集与 `set` 指向的信号集的并集。

**SIG_SETMASK**
结果集应为 `set` 指向的信号集。

**SIG_UNBLOCK**
结果集应为当前集与 `set` 指向的信号集补集的交集。

如果参数 `oset` 不是空指针，则之前的掩码应存储在 `oset` 指向的位置。如果 `set` 是空指针，参数 `how` 的值不显著，线程的信号掩码应保持不变；因此该调用可用于查询当前被阻塞的信号。

如果参数 `set` 不是空指针，在 `pthread_sigmask()` 更改当前阻塞信号集后，它应确定是否有任何未决的非阻塞信号；如果有，则在 `pthread_sigmask()` 调用返回之前应至少传递这些信号中的一个。

无法阻塞那些不能被忽略的信号。系统应强制执行此规则而不导致指示错误。

如果在 SIGFPE、SIGILL、SIGSEGV 或 SIGBUS 信号被阻塞时生成了其中任何一个信号，结果未定义，除非该信号是由另一个进程的动作或由 `kill()`、`pthread_kill()`、`raise()` 或 `sigqueue()` 函数之一生成的。

如果 `pthread_sigmask()` 失败，线程的信号掩码不应更改。

`sigprocmask()` 函数应等价于 `pthread_sigmask()`，除了如果从多线程进程中调用其行为未指定，并且在出错时它返回 -1 并将 `errno` 设置为错误编号而不是直接返回错误编号。

## RETURN VALUE

成功完成后，`pthread_sigmask()` 应返回 0；否则，它应返回相应的错误编号。

成功完成后，`sigprocmask()` 应返回 0；否则，应返回 -1 并设置 `errno` 以指示错误。

## ERRORS

这些函数在以下情况失败：

**[EINVAL]**
`set` 参数不是空指针且 `how` 参数的值不等于定义的值之一。

这些函数不应返回 [EINTR] 错误代码。

## EXAMPLES

### 多线程进程中的信号处理

此示例展示了 `pthread_sigmask()` 的使用，以便在多线程进程中处理信号。它提供了一个相当通用的框架，可以轻松地进行调整/扩展。

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <signal.h>
#include <string.h>
#include <errno.h>
...

static sigset_t   signal_mask;  /* 要阻塞的信号         */

int main (int argc, char *argv[])
{
    pthread_t  sig_thr_id;      /* 信号处理线程 ID      */
    int        rc;              /* 返回代码             */

    sigemptyset (&signal_mask);
    sigaddset (&signal_mask, SIGINT);
    sigaddset (&signal_mask, SIGTERM);
    rc = pthread_sigmask (SIG_BLOCK, &signal_mask, NULL);
    if (rc != 0) {
        /* 处理错误 */
        ...
    }
    /* 任何新创建的线程继承信号掩码 */

    rc = pthread_create (&sig_thr_id, NULL, signal_thread, NULL);
    if (rc != 0) {
        /* 处理错误 */
        ...
    }

    /* 应用程序代码 */
    ...
}

void *signal_thread (void *arg)
{
    int       sig_caught;    /* 捕获的信号         */
    int       rc;            /* 返回代码           */

    rc = sigwait (&signal_mask, &sig_caught);
    if (rc != 0) {
        /* 处理错误 */
    }
    switch (sig_caught)
    {
    case SIGINT:     /* 处理 SIGINT  */
        ...
        break;
    case SIGTERM:    /* 处理 SIGTERM */
        ...
        break;
    default:         /* 通常不应该发生 */
        fprintf (stderr, "\n意外信号 %d\n", sig_caught);
        break;
    }
}
```

## APPLICATION USAGE

虽然 `pthread_sigmask()` 在更改当前阻塞信号集后必须传递至少一个存在的未决非阻塞信号，但没有要求传递的信号包括那些被更改取消阻塞的信号。如果在 `pthread_sigmask()` 调用执行期间，一个或多个已经非阻塞的信号变为未决状态（参见 [2.4.1 信号生成和传递](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_04_01)），则在调用返回之前传递的信号可能仅包括那些信号。

## RATIONALE

当在由 `sigaction()` 安装的信号捕获函数中更改线程的信号掩码时，从信号捕获函数返回时信号掩码的恢复会覆盖该更改（参见 `sigaction()`）。如果信号捕获函数是用 `signal()` 安装的，是否发生这种情况未指定。

关于信号传递要求的讨论，请参见 `kill()`。

## FUTURE DIRECTIONS

无。

## SEE ALSO

[`exec`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html), [`kill()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/kill.html), [`sigaction()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html), [`sigaddset()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaddset.html), [`sigdelset()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigdelset.html), [`sigemptyset()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigemptyset.html), [`sigfillset()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigfillset.html), [`sigismember()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigismember.html), [`sigpending()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigpending.html), [`sigqueue()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigqueue.html), [`sigsuspend()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigsuspend.html)

XBD [`<signal.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)

## CHANGE HISTORY

首次在 Issue 3 中发布。为了与 POSIX.1-1988 标准对齐而包含在内。

### Issue 5

DESCRIPTION 已更新，以与 POSIX 线程扩展对齐。

`pthread_sigmask()` 函数已添加，以与 POSIX 线程扩展对齐。

### Issue 6

`pthread_sigmask()` 函数被标记为 Threads 选项的一部分。

`sigprocmask()` 的 SYNOPSIS 被标记为 CX 扩展，以注意此函数在 `<signal.h>` 头中的存在是对 ISO C 标准的扩展。

为与 ISO POSIX-1:1996 标准对齐进行了以下更改：

*   DESCRIPTION 已更新，以明确说明可能生成信号的函数。

规范文本已更新，以避免对应用程序要求使用术语"必须"。

`restrict` 关键字已添加到 `pthread_sigmask()` 和 `sigprocmask()` 原型中，以与 ISO/IEC 9899:1999 标准对齐。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/105，将 DESCRIPTION 和 RATIONALE 部分中的"进程的信号掩码"更新为"线程的信号掩码"。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/106，向 EXAMPLES 部分添加了示例。

### Issue 7

`pthread_sigmask()` 函数从 Threads 选项移动到 Base。

应用了 POSIX.1-2008，Technical Corrigendum 1，XSH/TC1-2008/0467 [319]。

### Issue 8

应用了 Austin Group Defect 1132，澄清了 [EINVAL] 错误。

应用了 Austin Group Defect 1636，澄清了 `pthread_sigmask()` 和 `sigprocmask()` 等价性的例外情况。

应用了 Austin Group Defect 1731，澄清了虽然 `pthread_sigmask()` 在更改当前阻塞信号集后必须传递至少一个存在的未决非阻塞信号，但没有要求传递的信号包括那些被更改取消阻塞的信号。

---

*UNIX® 是 The Open Group 的注册商标。*
*POSIX™ 是 IEEE 的商标。*
*Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved*