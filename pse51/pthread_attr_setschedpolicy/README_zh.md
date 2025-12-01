# pthread_attr_getschedpolicy

## NAME

pthread_attr_getschedpolicy, pthread_attr_setschedpolicy — 获取和设置调度策略属性 (**实时线程**)

## SYNOPSIS

```c
#include <pthread.h>

int pthread_attr_getschedpolicy(const pthread_attr_t *restrict attr,
                               int *restrict policy);
int pthread_attr_setschedpolicy(pthread_attr_t *attr, int policy);
```

## DESCRIPTION

`pthread_attr_getschedpolicy()` 和 `pthread_attr_setschedpolicy()` 函数分别用于获取和设置 `attr` 参数中的 `schedpolicy` 属性。

`policy` 参数支持的值应包括 SCHED_FIFO、SCHED_RR 和 SCHED_OTHER，这些值在 `<sched.h>` 头文件中定义。当使用调度策略 SCHED_FIFO、SCHED_RR 或 SCHED_SPORADIC 执行的线程在等待互斥锁时，当互斥锁解锁时，它们应按优先级顺序获取互斥锁。

如果传递给 `pthread_attr_getschedpolicy()` 或 `pthread_attr_setschedpolicy()` 的 `attr` 参数值不引用已初始化的线程属性对象，则其行为是未定义的。

## RETURN VALUE

如果成功，`pthread_attr_getschedpolicy()` 和 `pthread_attr_setschedpolicy()` 函数应返回零；否则，应返回错误编号以指示错误。

## ERRORS

`pthread_attr_setschedpolicy()` 函数在以下情况下应失败：

- **[ENOTSUP]**
  尝试将属性设置为不支持的值。

`pthread_attr_setschedpolicy()` 函数在以下情况下可能失败：

- **[EINVAL]**
  `policy` 的值无效。

这些函数不应返回错误代码 [EINTR]。

---

*以下部分为参考信息。*

## EXAMPLES

无。

## APPLICATION USAGE

在设置这些属性后，可以使用 `pthread_create()` 函数创建具有指定属性的线程。使用这些例程不会影响当前正在运行的线程。

有关线程调度属性及其默认设置的更多详细信息，请参见 [2.9.4 Thread Scheduling](../functions/V2_chap02.html#tag_16_09_04)。

## RATIONALE

如果实现检测到传递给 `pthread_attr_getschedpolicy()` 或 `pthread_attr_setschedpolicy()` 的 `attr` 参数值不引用已初始化的线程属性对象，建议函数应该失败并报告 [EINVAL] 错误。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`pthread_attr_destroy()`](../functions/pthread_attr_destroy.html)
- [`pthread_attr_getscope()`](../functions/pthread_attr_getscope.html)
- [`pthread_attr_getinheritsched()`](../functions/pthread_attr_getinheritsched.html)
- [`pthread_attr_getschedparam()`](../functions/pthread_attr_getschedparam.html)
- [`pthread_create()`](../functions/pthread_create.html)

XBD [`<pthread.h>`](../basedefs/pthread.h.html), [`<sched.h>`](../basedefs/sched.h.html)

## CHANGE HISTORY

首次发布于 Issue 5。为与 POSIX 线程扩展对齐而包含在内。

标记为实时线程特性组的一部分。

### Issue 6

`pthread_attr_getschedpolicy()` 和 `pthread_attr_setschedpolicy()` 函数被标记为线程和线程执行调度选项的一部分。

如果实现不支持线程执行调度选项，不需要提供存根，因此移除了 [ENOSYS] 错误条件。

为与 IEEE Std 1003.1d-1999 对齐，添加了 SCHED_SPORADIC 调度策略。

为与 ISO/IEC 9899:1999 标准对齐，在 `pthread_attr_getschedpolicy()` 原型中添加了 **restrict** 关键字。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/79，在 APPLICATION USAGE 部分添加了对 [2.9.4 Thread Scheduling](../functions/V2_chap02.html#tag_16_09_04) 的引用。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/80，更新了 ERRORS 部分，为 `attr` 引用未初始化线程属性对象的情况包含了可选错误。

### Issue 7

`pthread_attr_getschedpolicy()` 和 `pthread_attr_setschedpolicy()` 函数仅被标记为线程执行调度选项的一部分，因为线程选项现在是 Base 的一部分。

为未初始化线程属性对象的 [EINVAL] 错误被移除；这种情况会导致未定义行为。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0452 [314]。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0263 [757]。

---

*End of informative text.*

---

*UNIX® 是 The Open Group 的注册商标。*
*POSIX™ 是 IEEE 的商标。*
*版权所有 © 2001-2024 The IEEE and The Open Group，保留所有权利*