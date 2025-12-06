# clock_nanosleep

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

clock_nanosleep — 使用指定时钟的高精度休眠

## SYNOPSIS

```c
#include <time.h>

int clock_nanosleep(clockid_t clock_id, int flags,
                   const struct timespec *rqtp, struct timespec *rmtp);
```

## DESCRIPTION

如果在 `flags` 参数中未设置 TIMER_ABSTIME 标志，`clock_nanosleep()` 函数将使当前线程暂停执行，直到 `rqtp` 参数指定的时间间隔已经过去，或者向调用线程传递一个信号且其��作是调用信号捕获函数，或者进程终止。用于测量时间的时钟应该是 `clock_id` 指定的时钟。

如果在 `flags` 参数中设置了 TIMER_ABSTIME 标志，`clock_nanosleep()` 函数将使当前线程暂停执行，直到 `clock_id` 指定的时钟的时间值达到 `rqtp` 参数指定的绝对时间，或者向调用线程传递一个信号且其动作是调用信号捕获函数，或者进程终止。如果在调用时，`rqtp` 指定的时间值小于或等���指定时钟的时间值，那么 `clock_nanosleep()` 应立即返回，调用进程不应被暂停。

此函数导致的暂停时间可能比请求的时间更长，因为参数值会向上舍入为休眠分辨率的整数倍，或者因为系统对其他活动的调度。但是，除了被信号中断的情况外，相对 `clock_nanosleep()` 函数（即未设置 TIMER_ABSTIME 标志）的暂停时间不应小于 `rqtp` 指定的时间间隔，这以相应时钟的测量为准。绝对 `clock_nanosleep()` 函数（即设置了 TIMER_ABSTIME 标志）的暂停应至少持续到相应时钟的值达到 `rqtp` 指定的绝对时间为止，除了被信号中断的情况。

使用 `clock_nanosleep()` 函数对任何信号的动作或阻塞没有影响。

如果 `clock_id` 参数指代调用线程的 CPU 时间时钟，`clock_nanosleep()` 函数应该失败。是否允许其他 CPU 时间时钟的 `clock_id` 值是未指定的。

## RETURN VALUE

如果 `clock_nanosleep()` 函数因为请求的时间已经过去而返回，其返回值应该为零。

如果 `clock_nanosleep()` 函数因为被信号中断而返回，它应该返回相应的错误值。对于相对 `clock_nanosleep()` 函数，如果 `rmtp` 参数非 NULL，它引用的 `timespec` 结构应该被更新为包含间隔内剩余的时间量（请求时间减去实际睡眠时间）。`rqtp` 和 `rmtp` 参数可以指向同一个对象。如果 `rmtp` 参数为 NULL，则不返回剩余时间。绝对 `clock_nanosleep()` 函数对 `rmtp` 引用的结构没有影响。

如果 `clock_nanosleep()` 失败，它应该返回相应的错误值。

## ERRORS

`clock_nanosleep()` 函数在以下情况下应该失败：

- **[EINTR]**
  `clock_nanosleep()` 函数被信号中断。

- **[EINVAL]**
  `rqtp` 参数指定的纳秒值小于零或大于等于 1000 百万；或者在 flags 中指定了 TIMER_ABSTIME 标志，且 `rqtp` 参数在 `clock_id` 指定的时钟范围之外；或者 `clock_id` 参数没有指定已知时钟，或者指定了调用线程的 CPU 时间时钟。

- **[ENOTSUP]**
  `clock_id` 参数指定了一个不支持 `clock_nanosleep()` 的时钟，例如 CPU 时间时钟。

---

*以下章节为参考信息。*

## EXAMPLES

无。

## APPLICATION USAGE

使用未在 `flags` 参数中设置 TIMER_ABSTIME 值且 `clock_id` 为 CLOCK_REALTIME 的 `clock_nanosleep()` 调用等同于使用相同 `rqtp` 和 `rmtp` 参数的 `nanosleep()` 调用。

## RATIONALE

`nanosleep()` 函数指定使用系统范围的时钟 CLOCK_REALTIME 来测量此时间服务的经过时间。然而，随着单调时钟 CLOCK_MONOTONIC 的引入，需要一个新的相对休眠函数来允许应用程序利用这种时钟的特殊特性。

在许多应用程序中，进程需要以周期方式多次暂停然后激活；例如，轮询非中断设备的状态或刷新显示设备。对于这些情况，已知无法使用相对 `sleep()` 或 `nanosleep()` 函数调用实现精确的周期性激活。例如，假设一个周期性进程在时间 T0 激活，执行一段时间，然后想要暂停自身直到时间 T0+T，周期为 T。如果此进程想要使用 `nanosleep()` 函数，它必须首先调用 `clock_gettime()` 获取当前时间，然后计算当前时间与 T0+T 之间的差异，最后使用计算出的间隔调用 `nanosleep()`。然而，进程可能在两个函数调用之间被不同的进程抢占，在这种情况下计算的间隔会是错误的；进程会比期望的更晚唤醒。使用绝对 `clock_nanosleep()` 函数不会出现这个问题，因为只需要一个函数调用来暂停进程直到期望的时间。但在其他情况下，需要相对休眠，这就是为什么需要这两种功能。

虽然可以使用定时器接口实现周期性进程，但这种实现需要使用信号，并保留一些信号编号。在这方面，在 POSIX.1-2024 中包含 `clock_nanosleep()` 函数的绝对版本的原因与包含相对 `nanosleep()` 的原因相同。

也可以使用 `pthread_cond_timedwait()` 实现精确的周期性进程，其中指定一个绝对超时，如果涉及的条件变量从未被发信号，则该超时生效。然而，使用这种接口是不自然的，并且涉及对互斥锁和条件变量执行其他操作，这意味着不必要的开销。此外，在不支持线程的实现中，`pthread_cond_timedwait()` 不可用。

虽然新的高精度休眠服务的相对版本和绝对版本的接口是同一个 `clock_nanosleep()` 函数，但 `rmtp` 参数仅在相对休眠中使用。在相对 `clock_nanosleep()` 函数中需要此参数来在函数被信号中断时重新发出函数调用，但在绝对 `clock_nanosleep()` 函数调用中不需要此参数；如果调用被信号中断，绝对 `clock_nanosleep()` 函数可以使用被中断调用中使用的相同 `rqtp` 参数再次调用。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `clock_getres()`
- `nanosleep()`
- `pthread_cond_clockwait()`
- `sleep()`

XBD `<time.h>`

## CHANGE HISTORY

首次发布于 Issue 6。源自 IEEE Std 1003.1j-2000。

### Issue 7

`clock_nanosleep()` 函数从时钟选择选项移动到基础规范。

应用 POSIX.1-2008 Technical Corrigendum 2, XSH/TC2-2008/0068 [909]。

