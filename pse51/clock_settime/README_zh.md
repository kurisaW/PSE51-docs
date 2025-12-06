# clock_settime

## 名称

clock_getres, clock_gettime, clock_settime - 时钟和定时器函数

## 概要

```c
#include <time.h>

int clock_getres(clockid_t clock_id, struct timespec *res);
int clock_gettime(clockid_t clock_id, struct timespec *tp);
int clock_settime(clockid_t clock_id, const struct timespec *tp);
```

## 描述

`clock_getres()` 函数应返回任何时钟的分辨率。时钟分辨率由实现定义，进程无法设置。如果参数 `res` 不为 NULL，指定时钟的分辨率应存储在 `res` 指向的位置。如��� `res` 为 NULL，则不返回时钟分辨率。如果 `clock_settime()` 的 `time` 参数不是 `res` 的倍数，则该值将被截断为 `res` 的倍数。

`clock_gettime()` 函数应返回指定时钟 `clock_id` 的当前值 `tp`。

`clock_settime()` 函数应将指定时钟 `clock_id` 设置为 `tp` 指定的值。介于指定时钟分辨率的两个连续非负整数倍之间的时间值应向下截断为较小的分辨率倍数。

时��可以是系统范围的（即对所有进程可见）或每进程的（仅在进程内有意义的时间测量）。所有实现都应支持 `<time.h>` 中定义的 `clock_id` CLOCK_REALTIME。该时钟表示测量系统实时时间的时钟。对于此时钟，`clock_gettime()` 返回的值和 `clock_settime()` 指定的值表示自纪元以来的时间量（以秒和纳秒为单位）。实现也可以支持额外的时钟。这些时钟的时间值解释是未指定的。

### CLOCK_REALTIME 行为

如果通过 `clock_settime()` 设置 CLOCK_REALTIME 时钟的值，则应使用时钟的新值来确定基于 CLOCK_REALTIME 时钟的绝对时间服务的到期时间。这适用于已启动的绝对定时器到期的时间。如果在调用此类时间服务时请求的绝对时间在时钟的新值之前，则该时间服务应立即到期，就像时钟已正常达到请求的时间一样。

通过 `clock_settime()` 设置 CLOCK_REALTIME 时钟的值对基于此时钟等待相对时间服务的阻塞线程没有影响，包括 `nanosleep()` 和 `thrd_sleep()` 函数；对基于此时钟的相对定时器的到期也没有影响。因此，这些时间服务应在请求的相对间隔过去时到期，与时钟的新值或旧值无关。

### CLOCK_MONOTONIC 行为

所有实现都应支持 `<time.h>` 中定义的 `clock_id` CLOCK_MONOTONIC。该时钟表示系统的单调时钟。对于此时钟，`clock_gettime()` 返回的值表示自过去某个未指定点（例如，系统启动时间或纪元）以来的时间量（以秒和纳秒为单位）。此点在系统启动时间后不会改变。CLOCK_MONOTONIC 时钟的值无法通过 `clock_settime()` 设置。如果使用 CLOCK_MONOTONIC 的 `clock_id` 参数调用此函数，则该函数应失败。

### clock_nanosleep 交互

如果通过 `clock_settime()` 设置 CLOCK_REALTIME 时钟的值，则应使用时钟的新值来确定系统唤醒基于 CLOCK_REALTIME 时钟阻塞在绝对 `clock_nanosleep()` 调用上的线程的时间。如果在调用此类时间服务时请求的绝对时间在时钟的新值之前，则该调用应立即返回，就像时钟已正常达到请求的时间一样。

通过 `clock_settime()` 设置 CLOCK_REALTIME 时钟的值对任何阻塞在相对 `clock_nanosleep()` 调用上的线程没有影响。因此，该调用应在请求的相对间隔过去时返回，与时钟的新值或旧值无关。

### 定时器效果

通过 `clock_settime()` 设置时钟对与除 CLOCK_REALTIME 之外的时钟关联的已启动每进程定时器的效果是实现定义的。

### 权限

设置特定时钟的适当权限是实现定义的。

### CPU 时间时钟 [CPT]

如果定义了 _POSIX_CPUTIME，实现应支持通过调用 `clock_getcpuclockid()` 获得的时钟 ID 值，这些值表示给定进程的 CPU 时间时钟。实现还应支持特殊的 `clockid_t` 值 CLOCK_PROCESS_CPUTIME_ID，该值在调用 `clock_*()` 或 `timer_*()` 函数之一时表示调用进程的 CPU 时间时钟。对于这些时钟 ID，`clock_gettime()` 返回的值和 `clock_settime()` 指定的值表示与时钟关联的进程的执行时间量。通过 `clock_settime()` 更改 CPU 时间时钟的值对零星服务器调度策略的行为没有影响。

### 线程 CPU 时间时钟 [TCT]

如果定义了 _POSIX_THREAD_CPUTIME，实现应支持通过调用 `pthread_getcpuclockid()` 获得的时钟 ID 值，这些值表示给定线程的 CPU 时间时钟。实现还应支持特殊的 `clockid_t` 值 CLOCK_THREAD_CPUTIME_ID，该值在调用 `clock_*()` 或 `timer_*()` 函数之一时表示调用线程的 CPU 时间时钟。对于这些时钟 ID，`clock_gettime()` 返回的值和 `clock_settime()` 指定的值表示与时钟关联的线程的执行时间量。通过 `clock_settime()` 更改 CPU 时间时钟的值对零星服务器调度策略的行为没有影响。

## 返回值

成功完成后，`clock_getres()`、`clock_gettime()` 和 `clock_settime()` 应返回 0；否则，应返回 -1 并设置 errno 以指示错误。

## 错误

这些函数可能在以下情况下失败：

- **EINVAL**: `clock_id` 参数未指定已知时钟，或 `tp` 参数超出指定时钟的范围。

`clock_settime()` 函数可能在以下情况下失败：

- **EPERM**: 请求进程没有设置指定时钟的适当权限。
- **EINVAL**: `clock_id` 参数是 CLOCK_MONOTONIC。

这些函数可能在以下情况下失败：

- **EOVERFLOW**: 秒数无法放入 32 位有符号整数。

## 示例

本规范中未提供示例。

## 应用程序用法

以下部分是参考信息。

## 基本原理

未提供。

## 未来方向

未提供。

## 另请参阅

- `clock_getcpuclockid()`
- `clock_nanosleep()`
- `nanosleep()`
- `pthread_getcpuclockid()`
- `thrd_sleep()`
- `<time.h>`

## XSH

第 7 版

*以下部分是参考信息。*

