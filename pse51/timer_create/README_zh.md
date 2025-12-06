# timer_create - 创建每进程定时器

## SYNOPSIS （概要）

```c
#include <signal.h>
#include <time.h>

int timer_create(clockid_t clockid,
                 struct sigevent *restrict evp,
                 timer_t *restrict timerid);
```

## DESCRIPTION （描述）

`timer_create()` 函数应使用指定的时钟 `clock_id` 作为计时基础来创建一个每进程定时器。`timer_create()` 函数应在 `timerid` 引用的位置返回一个类型为 `timer_t` 的定时器 ID，用于在定时器请求中标识该定时器。在定时器被删除之前，此定时器 ID 在调用进程内应是唯一的。特定的时钟 `clock_id` 在 `<time.h>` 中定义。返回的 ID 所对应的定时器在从 `timer_create()` 返回时应处于解除武装状态。

如果 `evp` 参数非 NULL，则指向一个 `sigevent` 结构。这个由应用程序分配的结构定义了定时器到期时按照信号生成和传递规范发生的异步通知。如果 `evp` 参数为 NULL，则效果相当于 `evp` 参数指向一个 `sigevent` 结构，其中 `sigev_notify` 成员具有值 SIGEV_SIGNAL，`sigev_signo` 具有默认信号编号，`sigev_value` 成员具有定时器 ID 的值。

每个实现应定义一组可用作每进程定时器计时基础的时钟。所有实现都应支持 CLOCK_REALTIME 和 CLOCK_MONOTONIC 作为 `clock_id` 的值。

每进程定时器不应通过 `fork()` 由子进程继承，并应通过 `exec` 被解除武装和删除。

如果定义了 _POSIX_CPUTIME，实现应支持表示调用进程 CPU 时间时钟的 `clock_id` 值。

如果定义了 _POSIX_THREAD_CPUTIME，实现应支持表示调用线程 CPU 时间时钟的 `clock_id` 值。

如果 `clock_id` 定义的值对应于不同于调用函数的进程或线程的进程或线程的 CPU 时间时钟，`timer_create()` 函数是否会成功是由实现定义的。

如果 `evp->sigev_notify` 是 SIGEV_THREAD 且 `sev->sigev_notify_attributes` 不为 NULL，如果 `sev->sigev_notify_attributes` 指向的属性具有通过调用 `pthread_attr_setstack()` 指定的线程栈地址，则如果信号生成多次，结果未指定。

## RETURN VALUE （返回值）

如果调用成功，`timer_create()` 应返回零，并将 `timerid` 引用的位置更新为一个 `timer_t`，该值可以传递给每进程定时器调用。如果发生错误，函数应返回值 -1 并设置 `errno` 以指示错误。如果发生错误，`timerid` 的值未定义。

## ERRORS （错误）

`timer_create()` 函数应在以下情况下失败：

- **[EAGAIN]**
  - 系统缺乏足够的信号排队资源来满足请求。
  - 调用进程已经创建了此实现允许的所有定时器。

- **[EINVAL]**
  - 指定的时钟 ID 未定义。

- **[ENOTSUP]**
  - 实现不支持创建附加到由 `clock_id` 指定的 CPU 时间时钟的定时器，且该时钟与不同于调用 `timer_create()` 的进程或线程的进程或线程相关联。

## EXAMPLES （示例）

无。

## APPLICATION USAGE （应用程序用法）

如果创建的定时器将 `evp->sigev_notify` 设置为 SIGEV_THREAD，并且 `evp->sigev_notify_attributes` 指向的属性具有通过调用 `pthread_attr_setstack()` 指定的线程栈地址，则专用于线程栈的内存无法恢复。原因是响应定时器到期而创建的线程是以分离方式创建的，或者如果线程属性的 `detachstate` 是 PTHREAD_CREATE_JOINABLE，则以未指定的方式创建。在这两种情况下，调用 `pthread_join()` 都是无效的，这导致无法确定创建的线程的生命周期，从而意味着栈内存无法重用。

## RATIONALE （原理）

### 周期性定时器溢出和资源分配

指定的定时器设施可能在此选项支持的实现上传递实时信号（即排队信号）。由于实时应用程序不能承受丢失异步事件（如定时器到期或异步 I/O 完成）的通知，因此必须能够确保在事件发生时有足够的资源来传递信号。通常，这不是问题，因为请求与后续信号生成之间存在一一对应的关系。如果请求无法分配信号传递资源，它可以通过 [EAGAIN] 错误使调用失败。

周期性定时器是特例。单个请求可以生成未指定数量的信号。如果请求进程能以生成信号的速度服务信号，从而使信号传递资源可用于传递后续周期性定时器到期信号，这就不是问题。但通常，这无法保证——周期性定时器信号的处理可能"溢出"；即后续周期性定时器到期可能在当前待处理信号被传递之前发生。

另外，对于信号，根据 POSIX.1-1990 标准，如果生成了待处理信号的后续出现，是否为每次出现都传递一个信号是由实现定义的。这对某些实时应用程序来说是不够的。因此需要一种机制来允许应用程序检测有多少定时器到期被延迟，而不需要无限量的系统资源来存储延迟的到期。

指定的设施提供了溢出计数。溢出计数定义为在定时器到期信号生成时间和信号传递时间之间发生的额外定时器到期数量。如果信号捕获函数关心溢出，可以在入口时检索此计数。使用此方法，周期性定时器只需要一个"信号排队资源"，该资源可以在调用 `timer_create()` 函数时分配。

定义了一个函数来检索溢出计数，以便应用程序不需要分配静态存储来包含计数，实现也不需要在定时器到期时异步更新此存储。但对于某些高频周期性应用程序，每次定时器到期时的额外系统调用开销可能过于昂贵。如定义的函数允许实现在用户空间中维护与 `timerid` 相关联的溢出计数。`timer_getoverrun()` 函数然后可以实现为使用 `timerid` 参数（可能只是指向包含计数器的用户空间结构的指针）来定位溢出计数的宏，没有系统调用开销。其他不太关心此类应用程序的实现可以通过在系统结构中维护计数来避免用户空间的异步更新，代价是额外的系统调用来获取计数。

### 定时器到期信号参数

实时信号功能支持传递给扩展信号处理器的应用程序特定数据。此值由应用程序与要传递的信号编号一起在 `sigevent` 结构中明确指定。应用程序定义的值的类型可以是整数常量或指针。这种值的显式规范，而不是总是发送定时器 ID，是基于现有实践选择的。

实时应用程序（在非 POSIX 系统或实时扩展 POSIX 系统上）通常使用事件处理器的参数作为 switch 语句的 case 标签或指向应用程序定义数据结构的指针。由于 timer_id 由 `timer_create()` 函数动态分配，它们不能用于这些功能而不在信号处理器中产生额外的应用程序开销；例如，搜索保存的定时器 ID 数组以将 ID 与常量或应用程序数据结构关联。

## FUTURE DIRECTIONS （未来方向）

无。

## SEE ALSO （另请参阅）

- `clock_getres()`
- `timer_delete()`
- `timer_getoverrun()`
- `<signal.h>`
- `<time.h>`

## CHANGE HISTORY （变更历史）

首次在 Issue 5 中发布。包含用于与 POSIX 实时扩展对齐。

### Issue 6

`timer_create()` 函数被标记为 Timers 选项的一部分。

[ENOSYS] 错误条件已被删除，因为如果实现不支持 Timers 选项，不需要提供存根。

添加了 CPU 时间时钟以与 IEEE Std 1003.1d-1999 对齐。

DESCRIPTION 部分已更新以与 IEEE Std 1003.1j-2000 对齐，在 Monotonic Clock 选项下添加了对 CLOCK_MONOTONIC 时钟的要求。

`restrict` 关键字已添加到 `timer_create()` 原型以与 ISO/IEC 9899:1999 标准对齐。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/138，更新了 DESCRIPTION 和 APPLICATION USAGE 部分以描述通知方法设置为 SIGEV_THREAD 时创建定时器的情况。

### Issue 7

`timer_create()` 函数已从 Timers 选项移至 Base。

### Issue 8

应用了 Austin Group 缺陷 1116，删除了对本标准早期版本中存在的 Realtime Signals Extension 选项的引用。

应用了 Austin Group 缺陷 1346，要求支持 Monotonic Clock。

---
