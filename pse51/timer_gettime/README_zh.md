# timer_getoverrun, timer_gettime, timer_settime — 每进程定时器

## SYNOPSIS 概要

```c
#include <time.h>

int timer_getoverrun(timer_t timerid);
int timer_gettime(timer_t timerid, struct itimerspec *value);
int timer_settime(timer_t timerid, int flags,
                 const struct itimerspec *restrict value,
                 struct itimerspec *restrict ovalue);
```

## DESCRIPTION 描述

`timer_gettime()` 函数应将指定定时器 `timerid` 到期前的剩余时间量和定时器的重载值存储到 `value` 参数指向的空间中。此结构的 `it_value` 成员应包含定时器到期前的时间量，如果定时器已解除武装则为零。即使定时器是用绝对时间武装的，此值也作为定时器到期前的间隔返回。`value` 的 `it_interval` 成员应包含由 `timer_settime()` 最后设置的重载值。

`timer_settime()` 函数应从 `value` 参数的 `it_value` 成员设置指定定时器 `timerid` 到下一次到期的时间，如果 `value` 的 `it_value` 成员非零，则武装该定时器。如果在调用 `timer_settime()` 时指定的定时器已经被武装，此调用应重置到下一次到期的时间为指定的 `value`。如果 `value` 的 `it_value` 成员为零，则定时器应被解除武装。对于具有挂起到期通知的定时器，解除武装或重置的效果是未指定的。

如果参数 `flags` 中未设置标志 TIMER_ABSTIME，`timer_settime()` 应表现为到下一次到期的时间设置为等于 `value` 的 `it_value` 成员指定的间隔。即，定时器应在调用发生后的 `it_value` 纳秒到期。如果参数 `flags` 中设置了标志 TIMER_ABSTIME，`timer_settime()` 应表现为到下一次到期的时间设置为等于 `value` 的 `it_value` 成员指定的绝对时间与 `timerid` 关联的时钟当前值之间的差值。即，当时钟达到 `value` 的 `it_value` 成员指定的值时，定时器应到期。如果指定的时间已经过去，函数应成功执行并发出到期通知。

定时器的重载值应设置为 `value` 的 `it_interval` 成员指定的值。当定时器以非零的 `it_interval` 武装时，指定了一个周期性（或重复性）定时器。

介于指定定时器分辨率的两个连续非负整数倍之间的时间值应向上舍入到较大的分辨率倍数。量化误差不应导致定时器在舍入时间值之前到期。

如果参数 `ovalue` 不为 NULL，`timer_settime()` 函数应在 `ovalue` 引用的位置存储一个值，表示定时器原本到期的先前时间量，如果定时器被解除武装则为零，同时包含先前的定时器重载值。定时器不应在其预定时间之前到期。

在任何时间点，对于给定的定时器，只有一个信号会被排队到进程。当一个信号仍处于挂起状态的定时器到期时，不会排队信号，并且会发生定时器超限运行（timer overrun）。当定时器到期信号被递送到或被进程接受时，`timer_getoverrun()` 函数应返回指定定时器的定时器到期超限运行计数。返回的超限运行计数包含在信号生成（排队）时间和递送或接受时间之间发生的额外定时器到期次数，最多但不包括实现定义的最大值 {DELAYTIMER_MAX}。如果此类额外到期次数大于或等于 {DELAYTIMER_MAX}，则超限运行计数应设置为 {DELAYTIMER_MAX}。`timer_getoverrun()` 返回的值适用于定时器的最近一次到期信号递送或接受。如果没有为定时器递送到期信号，`timer_getoverrun()` 的返回值是未指定的。

如果传递给 `timer_getoverrun()`、`timer_gettime()` 或 `timer_settime()` 的 `timerid` 参数指定的值不对应于由 `timer_create()` 返回但尚未被 `timer_delete()` 删除的定时器 ID，则行为是未定义的。

## RETURN VALUE 返回值

如果 `timer_getoverrun()` 函数成功，它应返回定时器到期超限运行计数，如上所述。

如果 `timer_gettime()` 或 `timer_settime()` 函数成功，应返回值 0。

如果这些函数中的任何一个发生错误，应返回值 -1，并设置 `errno` 以指示错误。

## ERRORS 错误

`timer_settime()` 函数应在以下情况下失败：

- **[EINVAL]** `value` 结构指定的纳秒值小于零或大于或等于 1000 百万，且该结构的 `it_value` 成员未指定零秒和零纳秒。

`timer_settime()` 函数可能在以下情况下失败：

- **[EINVAL]** `value` 的 `it_interval` 成员不为零，且定时器是通过创建新线程（`sigev_sigev_notify` 为 SIGEV_THREAD）来创建通知的，并且 `sigev_notify_attributes` 指向的线程属性中设置了固定栈地址。

## APPLICATION USAGE 应用程序使用

当定时器到期通过创建新线程来发出信号时，使用固定栈地址是有问题的。由于不能假设为一个到期创建的线程在定时器的下一次到期之前已经完成，可能会发生两个线程同时使用相同内存作为栈的情况。这是无效的，并产生未定义的结果。

## RATIONALE 基本原理

实际的时钟以有限的速率滴答，100 赫兹和 1000 赫兹的速率很常见。此滴答速率的倒数是时钟分辨率，也称为时钟粒度，在任一情况下都表示为时间持续时间，对于这些常见速率分别为 10 毫秒和 1 毫秒。实际时钟的粒度意味着如果快速连续两次读取给定时钟，可能两次获得相同的时间值；并且定时器必须等待理论到期时间之后的下一个时钟滴答，以确保定时器永远不会过早返回。还要注意的是，时钟的粒度可能比用于设置和获取时间及间隔值的数据格式的分辨率明显粗糙。还要注意的是，一些实现可能选择调整时间和/或间隔值以精确匹配底层时钟的滴答。

POSIX.1-2024 卷定义了允许应用程序确定实现支持的时钟分辨率的函数，并要求实现记录定时器和 `nanosleep()` 支持的分辨率（如果它们与支持的时钟分辨率不同）。这更多是一个采购问题，而不是运行时应用程序问题。

如果实现检测到传递给 `timer_getoverrun()`、`timer_gettime()` 或 `timer_settime()` 的 `timerid` 参数指定的值不对应于由 `timer_create()` 返回但尚未被 `timer_delete()` 删除的定时器 ID，建议该函数应该失败并报告 [EINVAL] 错误。

## SEE ALSO 参见

- [`clock_getres()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/clock_getres.html)
- [`timer_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/timer_create.html)
- [`<time.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/time.h.html)

## CHANGE HISTORY 更改历史

首次发布于 Issue 5。为与 POSIX 实时扩展对齐而包含在内。

### Issue 6

`timer_getoverrun()`、`timer_gettime()` 和 `timer_settime()` 函数被标记为定时器选项的一部分。

[ENOSYS] 错误条件已被删除，因为如果实现不支持定时器选项，不需要提供存根。

[EINVAL] 错误条件已更新，包含以下内容："并且该结构的 `it_value` 成员未指定零秒和零纳秒。"此更改是针对 IEEE PASC Interpretation 1003.1 #89。

`timer_getoverrun()` 的描述已更新，以阐明："如果定时器没有递送到期信号，或者不支持实时信号扩展，`timer_getoverrun()` 的返回值是未指定的"。

`restrict` 关键字被添加到 `timer_settime()` 原型中，以与 ISO/IEC 9899:1999 标准对齐。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/140，更新 ERRORS 部分，使强制的 [EINVAL] 错误（"`timerid` 参数不对应于由 `timer_create()` 返回但尚未被 `timer_delete()` 删除的 ID"）变为可选。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/141，更新 ERRORS 部分，为通知方法设置为 SIGEV_THREAD 的定时器情况包含可选的 [EINVAL] 错误。还添加了应用程序使用文本。

### Issue 7

`timer_getoverrun()`、`timer_gettime()` 和 `timer_settime()` 函数从定时器选项移至基础部分。

与实时信号扩展选项相关的功能移至基础部分。

应用 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0370 [659]。