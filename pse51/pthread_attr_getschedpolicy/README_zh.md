# pthread_attr_getschedpolicy, pthread_attr_setschedpolicy

## SYNOPSIS（概要）

```c
#include <pthread.h>

int pthread_attr_getschedpolicy(const pthread_attr_t *restrict attr,
                               int *restrict policy);

int pthread_attr_setschedpolicy(pthread_attr_t *attr, int policy);
```

## DESCRIPTION（描述）

`pthread_attr_getschedpolicy()` 和 `pthread_attr_setschedpolicy()` 函数分别用于获取和设置 attr ��数中的调度策略属性。

支持的 policy 值应包括 SCHED_FIFO、SCHED_RR 和 SCHED_OTHER，这些值在 `<sched.h>` 头文件中定义。当以调度策略 SCHED_FIFO、SCHED_RR 或 SCHED_SPORADIC 执行的线程在互斥锁上等待时，它们应在互斥锁解锁时按优先级顺序获取互斥锁。

如果传递给 `pthread_attr_getschedpolicy()` 或 `pthread_attr_setschedpolicy()` 的 attr 参数值不引用已初始化的线程属性对象，则行为未定义。

## RETURN VALUE（返回值）

如果成功，`pthread_attr_getschedpolicy()` 和 `pthread_attr_setschedpolicy()` 函数应返回零；否则，应返回错误码以指示错误。

## ERRORS（错误）

`pthread_attr_setschedpolicy()` 函数在以下情况下可能失败：

- **ENOTSUP**
  - 尝试将属性设置为不支持的值。

`pthread_attr_setschedpolicy()` 函数在以下情况下可能失败：

- **EINVAL**
  - policy 的值无效。

这些函数不应返回 [EINTR] 错误码。

## EXAMPLES（示例）

无。

## APPLICATION USAGE（应用程序使用）

设置这些属性后，可以使用 `pthread_create()` 创建具有指定属性的线程。使用这些例程不会影响当前正在运行的线程。

有关线程调度属性及其默认设置的更多详细信息，请参阅 [2.9.4 线程调度](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_04)。

## RATIONALE（基本原理）

如果实现检测到传递给 `pthread_attr_getschedpolicy()` 或 `pthread_attr_setschedpolicy()` 的 attr 参数值不引用已初始化的线程属性对象，建议函数应失败并报告 [EINVAL] 错误。

## FUTURE DIRECTIONS（未来方向）

无。

## SEE ALSO（参见）

- [`pthread_attr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_destroy.html)
- [`pthread_attr_getscope()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getscope.html)
- [`pthread_attr_getinheritsched()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getinheritsched.html)
- [`pthread_attr_getschedparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getschedparam.html)
- [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html)

XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html), [`<sched.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sched.h.html)

## CHANGE HISTORY（变更历史）

首次在 Issue 5 中发布。为了与 POSIX 线程扩展对齐而包含。

标记为实时线程特性组的一部分。

### Issue 6

`pthread_attr_getschedpolicy()` 和 `pthread_attr_setschedpolicy()` 函数被标记为线程和线程执行调度选项的一部分。

[ENOSYS] 错误条件已被移除，因为如果实现不支持线程执行调度选项，则不需要提供存根。

为了与 IEEE Std 1003.1d-1999 对齐，添加了 SCHED_SPORADIC 调度策略。

为了与 ISO/IEC 9899:1999 标准对齐，向 `pthread_attr_getschedpolicy()` 原型添加了 **restrict** 关键字。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/79，在 APPLICATION USAGE 部分添加了对 [2.9.4 线程调度](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_04) 的引用。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/80，更新 ERRORS 部分以包含当 attr 引用未初始化线程属性对象时的可选错误。

### Issue 7

`pthread_attr_getschedpolicy()` 和 `pthread_attr_setschedpolicy()` 函数仅被标记为线程执行调度选项的一部分，因为线程选项现在已成为基础的一部分。

对于未初始化线程属性对象的 [EINVAL] 错误已被移除；此条件导致未定义行为。

应用了 POSIX.1-2008，技术勘误 1，XSH/TC1-2008/0452 [314]。

应用了 POSIX.1-2008，技术勘误 2，XSH/TC2-2008/0263 [757]。