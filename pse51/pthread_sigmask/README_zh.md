# pthread_sigmask, sigprocmask — 检查和更改阻塞信号

## 概要

```c
#include <signal.h>

int pthread_sigmask(int how, const sigset_t *restrict set,
                   sigset_t *restrict oset);
int sigprocmask(int how, const sigset_t *restrict set,
                sigset_t *restrict oset);
```

## 描述

`pthread_sigmask()` 函数用于检查、更改（或同时进行两者）调用线程的信号掩码。

如果参数 `set` 不是空指针，它指向一个用于更改当前阻塞信号集合的信号集合。

参数 `how` 指示更改集合的方式，应用程序应确保其值由以下之一组成：

**SIG_BLOCK**
- 结果集合应为当前集合与 `set` 指向的信号集合的并集。

**SIG_SETMASK**
- 结果集合应为 `set` 指向的信号集合。

**SIG_UNBLOCK**
- 结果集合应为当前集合与 `set` 指向的信号集合的补集的交集。

如果参数 `oset` 不是空指针，则应将之前的掩码存储在 `oset` 指向的位置。如果 `set` 是空指针，则参数 `how` 的值没有意义，且线程的信号掩码应保持不变；因此该调用可用于查询当前被阻塞的信号。

如果参数 `set` 不是空指针，在 `pthread_sigmask()` 更改当前阻塞信号集合后，它应确定是否有任何挂起的未阻塞信号；如果有，则在 `pthread_sigmask()` 调用返回之前至少应传递这些信号中的一个。

无法阻塞那些不能被忽略的信号。系统应在不导致错误指示的情况下强制执行此规定。

如果 SIGFPE、SIGILL、SIGSEGV 或 SIGBUS 信号中的任何一个在被阻塞时生成，则结果未定义，除非该信号是由另一个进程的操作，或由 `kill()`、`pthread_kill()`、`raise()` 或 `sigqueue()` 函数之一生成的。

如果 `pthread_sigmask()` 失败，线程的信号掩码不应被更改。

`sigprocmask()` 函数应等同于 `pthread_sigmask()`，不同之处在于如果从多线程进程中调用其行为未指定，并且它在错误时返回 -1 并将 `errno` 设置为错误编号，而不是直接返回错误编号。

## 返回值

成功完成后，`pthread_sigmask()` 应返回 0；否则，应返回相应的错误编号。

成功完成后，`sigprocmask()` 应返回 0；否则，应返回 -1 并设置 `errno` 以指示错误。

## 错误

这些函数在以下情况下可能会失败：

**[EINVAL]**
- `set` 参数不是空指针，且 `how` 参数的值不等于任何定义的值。

这些函数不应返回 [EINTR] 错误代码。

## 示例

### 多线程进程中的信号处理

此示例显示了 `pthread_sigmask()` 在多线程进程中处理信号的使用。它提供了一个相当通用的框架，可以轻松地适配/扩展。

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
    pthread_t  sig_thr_id;      /* 信号处理线程 ID */
    int        rc;              /* 返回代码              */

    sigemptyset (&signal_mask);
    sigaddset (&signal_mask, SIGINT);
    sigaddset (&signal_mask, SIGTERM);
    rc = pthread_sigmask (SIG_BLOCK, &signal_mask, NULL);
    if (rc != 0) {
        /* 处理错误 */
        ...
    }
    /* 任何新创建的线程都会继承信号掩码 */

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
    int       sig_caught;    /* 捕获的信号       */
    int       rc;            /* 返回代码       */

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
    default:         /* 通常不应发生 */
        fprintf (stderr, "\n意外信号 %d\n", sig_caught);
        break;
    }
}
```

## 应用程序用法

虽然 `pthread_sigmask()` 必须在更改当前阻塞信号集合后传递至少一个存在的挂起未阻塞信号，但没有要求传递的信号包括通过该更改解除阻塞的任何信号。如果在 `pthread_sigmask()` 调用执行期间，一个或多个已经解除阻塞的信号变为挂起状态（参见 2.4.1 信号生成和传递），在调用返回之前传递的信号可能仅包括那些信号。

## 原理

当在由 `sigaction()` 安装的信号捕获函数中更改线程的信号掩码时，从信号捕获函数返回时信号掩码的恢复会覆盖该更改（参见 `sigaction()`）。如果信号捕获函数是通过 `signal()` 安装的，则是否发生这种情况未指定。

关于信号传递要求，请参见 `kill()` 的讨论。

## 未来方向

无。

## 另请参见

`exec`, `kill()`, `sigaction()`, `sigaddset()`, `sigdelset()`, `sigemptyset()`, `sigfillset()`, `sigismember()`, `sigpending()`, `sigqueue()`, `sigsuspend()`

XBD `<signal.h>`

## 更改历史

首次在 Issue 3 中发布。为与 POSIX.1-1988 标准对齐而包含。

### Issue 5
- 描述已更新，以与 POSIX 线程扩展对齐。
- 为与 POSIX 线程扩展对齐而添加了 `pthread_sigmask()` 函数。

### Issue 6
- `pthread_sigmask()` 函数被标记为线程选项的一部分。
- `sigprocmask()` 的概要被标记为 CX 扩展，以注意该函数在 `<signal.h>` 头文件中的存在是对 ISO C 标准的扩展。
- 为与 ISO POSIX-1:1996 标准对齐而进行了以下更改：
  - 描述已更新，以明确说明可能生成信号的函数。
- 规范性文本已更新，以避免在应用程序要求中使用术语"必须"。
- 为与 ISO/IEC 9899:1999 标准对齐，`restrict` 关键字被添加到 `pthread_sigmask()` 和 `sigprocmask()` 原型中。
- 应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/105，将描述和原理部分中的"进程的信号掩码"更新为"线程的信号掩码"。
- 应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/106，向示例部分添加了示例。

### Issue 7
- `pthread_sigmask()` 函数从线程选项移动到基础。
- POSIX.1-2008，技术勘误 1，XSH/TC1-2008/0467 [319] 被应用。

### Issue 8
- 应用了 Austin Group 缺陷 1132，澄清了 [EINVAL] 错误。
- 应用了 Austin Group 缺陷 1636，澄清了 `pthread_sigmask()` 和 `sigprocmask()` 等价性的例外情况。
- 应用了 Austin Group 缺陷 1731，澄清了虽然 `pthread_sigmask()` 必须在更改当前阻塞信号集合后传递至少一个存在的挂起未阻塞信号，但没有要求传递的信号包括通过该更改解除阻塞的任何信号。