# clock_getres, clock_gettime, clock_settime — 时钟和定时器函数

## 概述

```c
#include <time.h>

int clock_getres(clockid_t clock_id, struct timespec *res);
int clock_gettime(clockid_t clock_id, struct timespec *tp);
int clock_settime(clockid_t clock_id, const struct timespec *tp);
```

## 描述

`clock_getres()` 函数应返回任何时钟的分辨率。时钟分辨率是由实现定义的，不能由进程设置。如果参数 `res` 不为 NULL，指定时钟的分辨率应存储在 `res` 指向的位置。如果 `res` 为 NULL，则不返回时钟分辨率。如果 `clock_settime()` 的时间参数不是 `res` 的倍数，则该值被截断为 `res` 的倍数。

`clock_gettime()` 函数应返回指定时钟 `clock_id` 的当前值 `tp`。

`clock_settime()` 函数应将指定时钟 `clock_id` 设置为 `tp` 指定的值。介于指定时钟分辨率的两个连续非负整数倍数之间的时间值应被向下截断为较小的分辨率倍数。

时钟可以是系统范围的（即对所有进程可见）或每进程的（仅在进程内有意义的时间测量）。所有实现都应支持 `<time.h>` 中定义的 `clock_id` CLOCK_REALTIME。此时钟表示测量系统实时时间的时钟。对于此时钟，`clock_gettime()` 返回的值和 `clock_settime()` 指定的值表示自纪元以来的时间量（以秒和纳秒为单位）。实现也可以支持额外的时钟。这些时钟的时间值解释是未指定的。

如果通过 `clock_settime()` 设置 CLOCK_REALTIME 时钟的值，时钟的新值应用于确定基于 CLOCK_REALTIME 时钟的绝对时间服务的到期时间。这适用于已启动的绝对定时器到期的时间。如果在调用此类时间服务时请求的绝对时间早于时钟的新值，则时间服务应立即到期，就像时钟已正常达到请求的时间一样。

通过 `clock_settime()` 设置 CLOCK_REALTIME 时钟的值对基于此时钟等待相对时间服务的阻塞线程（包括 `nanosleep()` 和 `thrd_sleep()` 函数）没有影响；对基于此时钟的相对定时器的到期也没有影响。因此，这些时间服务应在请求的相对间隔过去时到期，与时钟的新值或旧值无关。

所有实现都应支持 `<time.h>` 中定义的 `clock_id` CLOCK_MONOTONIC。此时钟表示系统的单调时钟。对于此时钟，`clock_gettime()` 返回的值表示自过去某个未指定点（例如，系统启动时间或纪元）以来的时间量（以秒和纳秒为单位）。此点在系统启动时间后不会改变。CLOCK_MONOTONIC 时钟的值不能通过 `clock_settime()` 设置。如果使用 CLOCK_MONOTONIC 的 `clock_id` 参数调用此函数，则该函数应失败。

通过 `clock_settime()` 设置时钟对除 CLOCK_REALTIME 之外的其他时钟关联的已启动每进程定时��的影响是由实现定义的。

如果通过 `clock_settime()` 设置 CLOCK_REALTIME 时钟的值，时钟的新值应用于确定系统应唤醒基于 CLOCK_REALTIME 时钟阻塞在绝对 `clock_nanosleep()` 调用上的线程的时间。如果在调用此类时间服务时请求的绝对时间早于时钟的新值，则调用应立即返回，就像时钟已正常达到请求的时间一样。

通过 `clock_settime()` 设置 CLOCK_REALTIME 时钟的值对任何阻塞在相对 `clock_nanosleep()` 调用上的线程没有影响。因此，调用应在请求的相对间隔过去时返回，与时钟的新值或旧值无关。

设置特定时钟的适当特权是由实现定义的。

### CPU 时间时钟支持（选项：CPT）

如果定义了 _POSIX_CPUTIME，实现应支持通过调用 `clock_getcpuclockid()` 获得的时钟 ID 值，这些值表示给定进程的 CPU 时间时钟。实现还应支持特殊的 **clockid_t** 值 CLOCK_PROCESS_CPUTIME_ID，该值在调用 `clock_*()` 或 `timer_*()` 函数之一时表示调用进程的 CPU 时间时钟。对于这些时钟 ID，`clock_gettime()` 返回的值和 `clock_settime()` 指定的值表示与时钟关联的进程的执行时间量。通过 `clock_settime()` 更改 CPU 时间时钟的值对间歇服务器调度策略的行为没有影响。

### 线程 CPU 时间时钟支持（选项：TCT）

如果定义了 _POSIX_THREAD_CPUTIME，实现应支持通过调用 `pthread_getcpuclockid()` 获得的时钟 ID 值，这些值表示给定线程的 CPU 时间时钟。实现还应支持特殊的 **clockid_t** 值 CLOCK_THREAD_CPUTIME_ID，该值在调用 `clock_*()` 或 `timer_*()` 函数之一时表示调用线程的 CPU 时间时钟。对于这些时钟 ID，`clock_gettime()` 返回的值和 `clock_settime()` 指定的值表示与时钟关联的线程的执行时间量。通过 `clock_settime()` 更改 CPU 时间时钟的值对间歇服务器调度策略的行为没有影响。

## 返回值

返回值为 0 应表示调用成功。返回值为 -1 应表示发生了错误，并且 `errno` 应被设置为指示错误。

## 错误

`clock_getres()`、`clock_gettime()` 和 `clock_settime()` 函数在以下情况下应失败：

- **[EINVAL]**
  `clock_id` 参数未指定已知时钟。

`clock_gettime()` 函数在以下情况下应失败：

- **[EOVERFLOW]**
  秒数不适合 `time_t` 类型的对象。

`clock_settime()` 函数在以下情况下应失败：

- **[EINVAL]**
  `clock_settime()` 的 `tp` 参数超出了给定时钟 ID 的范围。
- **[EINVAL]**
  `tp` 参数指定的纳秒值小于零或大于等于 1000 百万。
- **[EINVAL]**
  `clock_id` 参数的值是 CLOCK_MONOTONIC。

`clock_settime()` 函数在以下情况下可能失败：

- **[EPERM]**
  请求进程没有设置指定时钟的适当特权。

---

## 应用程序用法

请注意，单调时钟的绝对值是无意义的（因为其原点是任意的），因此无需设置它。此外，实时应用程序可以依赖这样一个事实：此时钟的值永远不会被设置，因此用此时钟测量的时间间隔不会受到 `clock_settime()` 调用的影响。

## 示例

无。

## 原理

无。

## 未来方向

无。

## 另请参阅

- 调度策略
- `clock_getcpuclockid()`
- `clock_nanosleep()`
- `ctime()`
- `mq_receive()`
- `mq_send()`
- `nanosleep()`
- `pthread_mutex_clocklock()`
- `sem_clockwait()`
- `thrd_sleep()`
- `time()`
- `timer_create()`
- `timer_getoverrun()`

XBD `<time.h>`

## 变更历史

### 首次发布于 Issue 5
为与 POSIX 实时扩展对齐而包含。

### Issue 6
- [ENOSYS] 错误条件已被删除，因为如果实现不支持定时器选项，则不需要提供存根。
- 添加了应用程序用法部分。
- 添加了重置时钟分辨率影响的说明。
- 为与 IEEE Std 1003.1d-1999 对齐而添加了 CPU 时间时钟和 `clock_getcpuclockid()` 函数。
- 为与 IEEE Std 1003.1j-2000 对齐的更改：
  - 更新了描述，如下：
    - 指定了 `clock_gettime()` 为 CLOCK_MONOTONIC 返回的值。
    - 指定了 `clock_settime()` 函数对 CLOCK_MONOTONIC 失败。
    - 指定了 `clock_settime()` 对 `clock_nanosleep()` 函数相对于 CLOCK_REALTIME 的影响。
  - 在错误部分添加了 [EINVAL] 错误，表示 `clock_settime()` 对 CLOCK_MONOTONIC 失败。
  - 应用程序用法部分指出，CLOCK_MONOTONIC 时钟无需也不应由 `clock_settime()` 设置，因为 CLOCK_MONOTONIC 时钟的绝对值是无意义的。
  - 将 `clock_nanosleep()`、`mq_timedreceive()`、`mq_timedsend()`、`pthread_mutex_timedlock()`、`sem_timedwait()`、`timer_create()` 和 `timer_settime()` 函数添加到另请参阅部分。

### Issue 7
- 与时钟选择选项相关的功能被移至基础。
- `clock_getres()`、`clock_gettime()` 和 `clock_settime()` 函数从定时器选项移至基础。
- 应用了 POSIX.1-2008 技术勘误 1，XSH/TC1-2008/0058 [106]。

### Issue 8
- 应用了 Austin Group 缺陷 1302，将"the `nanosleep()` function"更改为"the `nanosleep()` and `thrd_sleep()` functions"。
- 应用了 Austin Group 缺陷 1346，要求支持单调时钟。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*