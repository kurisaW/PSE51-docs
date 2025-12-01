# timer_getoverrun, timer_gettime, timer_settime — 每进程定时器

## 概要

```c
#include <time.h>

int timer_getoverrun(timer_t timerid);
int timer_gettime(timer_t timerid, struct itimerspec *value);
int timer_settime(timer_t timerid, int flags,
                  const struct itimerspec *restrict value,
                  struct itimerspec *restrict ovalue);
```

## 描述

`timer_gettime()` 函数应将指定定时器 `timerid` 到期之前的时间量和定时器的重载值存储到 `value` 参数指向的空间中。该结构的 `it_value` 成员应包含定时器到期之前的时间量，如果定时器已解除武装则为零。即使定时器是以绝对时间武装的，该值也作为定时器到期前的间隔返回。`value` 的 `it_interval` 成员应包含由 `timer_settime()` 最后设置的重载值。

`timer_settime()` 函数应从 `value` 参数的 `it_value` 成员设置由 `timerid` 指定的定时器的下次到期时间，如果 `value` 的 `it_value` 成员非零，则武装定时器。如果在调用 `timer_settime()` 时指定的定时器已经武装，此调用应将下次到期的时间重置为指定的 `value`。如果 `value` 的 `it_value` 成员为零，则定时器应被解除武装。解除武装或重置具有挂起到期通知的定时器的效果是未指定的。

如果标志 TIMER_ABSTIME 未在参数 `flags` 中设置，`timer_settime()` 的行为应如同下次到期的时间被设置为等于 `value` 的 `it_value` 成员指定的间隔。也就是说，定时器应在调用发出后的 `it_value` 纳秒后到期。如果标志 TIMER_ABSTIME 在参数 `flags` 中设置，`timer_settime()` 的行为应如同下次到期的时间被设置为等于 `value` 的 `it_value` 成员指定的绝对时间与 `timerid` 关联的时钟当前值之间的差值。也就是说，当时钟达到 `value` 的 `it_value` 成员指定的值时，定时器应到期。如果指定的时间已经过去，函数应成功并发出到期通知。

定时器的重载值应设置为 `value` 的 `it_interval` 成员指定的值。当定时器以非零的 `it_interval` 武装时，指定了周期性（或重复性）定时器。

介于指定定时器分辨率的两个连续非负整数倍之间的时间值应向上舍入到较大的分辨率倍数。量化误差不应导致定时器在舍入时间值之前到期。

如果参数 `ovalue` 不为 NULL，`timer_settime()` 函数应将表示定时器原来到期前时间量的值（如果定时器已解除武装则为零）以及原来的定时器重载值一起存储在 `ovalue` 引用的位置。定时器不应在预定时间之前到期。

在任何时间点，对于给定的定时器，只有一个信号会被排队到进程。当一个信号仍在挂起中的定时器到期时，不会排队信号，并且会发生定时器超限。当定时器到期信号传递给或被进程接受时，`timer_getoverrun()` 函数应返回指定定时器的定时器到期超限计数。返回的超限计数包含在信号生成（排队）时间和传递或接受时间之间发生的额外定时器到期次数，最多但不包括实现定义的最大值 {DELAYTIMER_MAX}。如果此类额外到期次数大于或等于 {DELAYTIMER_MAX}，则超限计数应设置为 {DELAYTIMER_MAX}。`timer_getoverrun()` 返回的值适用于定时器最近的到期信号传递或接受。如果没有为定时器传递到期信号，`timer_getoverrun()` 的返回值是未指定的。

如果传递给 `timer_getoverrun()`、`timer_gettime()` 或 `timer_settime()` 的 `timerid` 参数指定的值不对应于由 `timer_create()` 返回但尚未被 `timer_delete()` 删除的定时器 ID，则行为是未定义的。

## 返回值

如果 `timer_getoverrun()` 函数成功，它应返回定时器到期超限计数，如上所述。

如果 `timer_gettime()` 或 `timer_settime()` 函数成功，应返回值 0。

如果任何这些函数发生错误，应返回值 -1，并设置 `errno` 以指示错误。

## 错误

`timer_settime()` 函数在以下情况下应失败：

- **[EINVAL]**
  `value` 结构指定了小于零或大于等于 10 亿的纳秒值，并且该结构的 `it_value` 成员没有指定零秒和零纳秒。

`timer_settime()` 函数在以下情况下可能失败：

- **[EINVAL]**
  `value` 的 `it_interval` 成员不为零，并且定时器是通过创建新线程（`sigev_sigev_notify` 为 SIGEV_THREAD）创建通知的，并且在 `sigev_notify_attributes` 指向的线程属性中设置了固定堆栈地址。

## 应用程序用法

当定时器到期通过创建新线程来发信号时，使用固定堆栈地址是有问题的。由于不能假定为一个到期创建的线程在定时器的下次到期之前已经完成，因此可能发生两个线程同时使用相同内存作为堆栈的情况。这是无效的，并会产生未定义的结果。

## 基本原理

实际的时钟以有限速率滴答，100 赫兹和 1000 赫兹是常见的速率。此滴答速率的倒数是时钟分辨率，也称为时钟粒度，无论哪种情况都表示为时间持续时间，对于这些常见速率分别为 10 毫秒和 1 毫秒。实际时钟的粒度意味着，如果快速连续两次读取给定时钟，可能两次获得相同的时间值；并且定时器必须等待理论到期时间之后的下一个时钟滴答，以确保定时器永远不会提前返回。还要注意，时钟的粒度可能比用于设置和获取时间及间隔值的数据格式的分辨率明显粗糙。还要注意，一些实现可能选择调整时间和/或间隔值以精确匹配底层时钟的滴答。

POSIX.1-2024 本卷定义了允许应用程序确定实现支持的时钟分辨率的函数，并要求实现文档化定时器和 `nanosleep()` 支持的分辨率（如果它们与支持的时钟分辨率不同）。这更多的是采购问题，而不是运行时应用程序问题。

如果实现检测到传递给 `timer_getoverrun()`、`timer_gettime()` 或 `timer_settime()` 的 `timerid` 参数指定的值不对应于由 `timer_create()` 返回但尚未被 `timer_delete()` 删除的定时器 ID，建议函数应失败并报告 [EINVAL] 错误。

## 未来方向

无。

## 另请参阅

- `clock_getres()`
- `timer_create()`
- `<time.h>`

## 更改历史

首次在 Issue 5 中发布。包含用于与 POSIX 实时扩展对齐。

### Issue 6

`timer_getoverrun()`、`timer_gettime()` 和 `timer_settime()` 函数被标记为定时器选项的一部分。

[ENOSYS] 错误条件已被删除，因为如果实现不支持定时器选项，则不需要提供存根。

[EINVAL] 错误条件更新为包括以下内容："并且该结构的 `it_value` 成员没有指定零秒和零纳秒。"此更改是为了 IEEE PASC 解释 1003.1 #89。

`timer_getoverrun()` 的描述更新为阐明："如果没有为定时器传递到期信号，或者不支持实时信号扩展，`timer_getoverrun()` 的返回值是未指定的"。

为了与 ISO/IEC 9899:1999 标准对齐，`restrict` 关键字添加到 `timer_settime()` 原型中。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/140，更新错误部分，使强制性的 [EINVAL] 错误（"`timerid` 参数不对应于由 `timer_create()` 返回但尚未被 `timer_delete()` 删除的 ID"）变为可选的。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/141，更新错误部分以包括当定时器使用设置为 SIGEV_THREAD 的通知方法创建时的可选 [EINVAL] 错误。还添加了应用程序用法文本。

### Issue 7

`timer_getoverrun()`、`timer_gettime()` 和 `timer_settime()` 函数从定时器选项移动到基础。

与实时信号扩展选项相关的功能移动到基础。

应用了 POSIX.1-2008，技术勘误 2，XSH/TC2-2008/0370 [659]。