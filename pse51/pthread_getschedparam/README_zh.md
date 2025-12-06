# pthread_getschedparam, pthread_setschedparam — 动态线程调度参数访问 (REALTIME THREADS)

## SYNOPSIS

```c
#include <pthread.h>

int pthread_getschedparam(pthread_t thread,
                         int *restrict policy,
                         struct sched_param *restrict param);

int pthread_setschedparam(pthread_t thread,
                         int policy,
                         const struct sched_param *param);
```

## DESCRIPTION

`pthread_getschedparam()` 和 `pthread_setschedparam()` 函数分别用于获取和设置多线程进程中单个线程的调度策略和参数。对于 SCHED_FIFO 和 SCHED_RR，`sched_param` 结构中唯一必需的成员是优先级 `sched_priority`。对于 SCHED_OTHER，受影响的调度参数由实现定义。

`pthread_getschedparam()` 函数应检索由 `thread` 给定线程 ID 的线程的调度策略和调度参数，并分别将这些值存储在 `policy` 和 `param` 中。从 `pthread_getschedparam()` 返回的优先级值应为影响目标线程的最近一次 `pthread_setschedparam()`、`pthread_setschedprio()` 或 `pthread_create()` 调用指定的值。它不应反映由于任何优先级继承或上限函数导致的对其优先级的任何临时调整。`pthread_setschedparam()` 函数应将由 `thread` 给定线程 ID 的线程的调度策略和关联调度参数设置为 `policy` 和 `param` 中分别提供的策略和关联参数。

`policy` 参数可以具有 SCHED_OTHER、SCHED_FIFO 或 SCHED_RR 的值。SCHED_OTHER 策略的调度参数由实现定义。SCHED_FIFO 和 SCHED_RR 策略应具有单一调度参数 `priority`。

如果定义了 `_POSIX_THREAD_SPORADIC_SERVER`，那么 `policy` 参数可以具有 SCHED_SPORADIC 的值，但 `pthread_setschedparam()` 函数的例外是，如果在调用时调度策略不是 SCHED_SPORADIC，则是否支持该函数由实现定义；换句话说，实现不需要允许应用程序动态将调度策略更改为 SCHED_SPORADIC。偶发服务器调度策略具有关联参数 `sched_ss_low_priority`、`sched_ss_repl_period`、`sched_ss_init_budget`、`sched_priority` 和 `sched_ss_max_repl`。指定的 `sched_ss_repl_period` 必须大于或等于指定的 `sched_ss_init_budget` 函数才能成功；如果不是，则函数应失败。`sched_ss_max_repl` 的值必须在 [1,{SS_REPL_MAX}] 的包含范围内函数才能成功；如果不是，函数应失败。`sched_ss_repl_period` 和 `sched_ss_init_budget` 值是否按此函数提供的值存储或舍入以与正在使用的时钟分辨率对齐是未指定的。

如果 `pthread_setschedparam()` 函数失败，则目标线程的调度参数不应更改。

## RETURN VALUE

如果成功，`pthread_getschedparam()` 和 `pthread_setschedparam()` 函数应返回零；否则，应返回错误编号以指示错误。

## ERRORS

`pthread_setschedparam()` 函数应在以下情况下失败：

- **ENOTSUP**: 尝试将策略或调度参数设置为不支持的值。
- **ENOTSUP**: 尝试将调度策略动态更改为 SCHED_SPORADIC，而实现不支持此更改。

`pthread_setschedparam()` 函数可能在以下情况下失败：

- **EINVAL**: 由 `policy` 指定的值或与调度策略 `policy` 关联的某个调度参数无效。
- **EPERM**: 调用者没有适当的权限来设置指定线程的调度参数或调度策略。
- **EPERM**: 实现不允许应用程序将某个参数修改为指定的值。

这些函数不应返回 [EINTR] 错误代码。

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

如果实现在线程生命周期结束后检测到线程 ID 的使用，建议函数失败并报告 [ESRCH] 错误。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`pthread_setschedprio()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_setschedprio.html)
- [`sched_getparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_getparam.html)
- [`sched_getscheduler()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_getscheduler.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)
- XBD [`<sched.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sched.h.html)

## CHANGE HISTORY

### 首次发布于 Issue 5
为与 POSIX 线程扩展对齐而包含。

### Issue 6
- `pthread_getschedparam()` 和 `pthread_setschedparam()` 函数被标记为线程和线程执行调度选项的一部分。
- [ENOSYS] 错误条件已被移除，因为如果实现不支持线程执行调度选项，则无需提供存根。
- 应用了 Open Group Corrigendum U026/2，更正了 `pthread_setschedparam()` 函数的原型，使其第二个参数为 `int` 类型。
- 为与 IEEE Std 1003.1d-1999 对齐而添加了 SCHED_SPORADIC 调度策略。
- 为与 ISO/IEC 9899:1999 标准对齐而向 `pthread_getschedparam()` 原型添加了 `restrict` 关键字。
- 应用了 Open Group Corrigendum U047/1。
- 应用了 IEEE PASC Interpretation 1003.1 #96，注意优先级值也可以通过调用 [`pthread_setschedprio()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_setschedprio.html) 函数来设置。

### Issue 7
- `pthread_getschedparam()` 和 `pthread_setschedparam()` 函数仅被标记为线程执行调度选项的一部分，因为线程选项现在是基础的一部分。
- 应用了 Austin Group Interpretation 1003.1-2001 #119，阐明了 `sched_ss_repl_period` 和 `sched_ss_init_budget` 值的准确性要求。
- 应用了 Austin Group Interpretation 1003.1-2001 #142，移除了 [ESRCH] 错误条件。
- 应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0459 [314]。
- 应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0276 [757]。

---
