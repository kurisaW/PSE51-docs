# pthread_attr_getinheritsched, pthread_attr_setinheritsched — 获取和设置 inheritsched 属性 (实时线程)

## 概要

```c
#include <pthread.h>

int pthread_attr_getinheritsched(const pthread_attr_t *restrict attr,
                                int *restrict inheritsched);

int pthread_attr_setinheritsched(pthread_attr_t *attr,
                                int inheritsched);
```

## 描述

`pthread_attr_getinheritsched()` 和 `pthread_attr_setinheritsched()` 函数分别用于获取和设置 `attr` 参数中的 `inheritsched` 属性。

当属性对象被 `pthread_create()` 使用时，`inheritsched` 属性决定了所创建线程的其他调度属性应如何设置。

`inheritsched` 支持的值为：

- **PTHREAD_INHERIT_SCHED**
  - 指定线程调度属性应从创建线程继承，此 `attr` 参数中的调度属性将被忽略。

- **PTHREAD_EXPLICIT_SCHED**
  - 指定线程调度属性应设置为此属性对象中的相应值。

符号 `PTHREAD_INHERIT_SCHED` 和 `PTHREAD_EXPLICIT_SCHED` 在 `<pthread.h>` 头文件中定义。

POSIX.1-2024 定义的以下线程调度属性受 `inheritsched` 属性影响：调度策略 (`schedpolicy`)、调度参数 (`schedparam`) 和调度竞争范围 (`contentionscope`)。

如果传递给 `pthread_attr_getinheritsched()` 或 `pthread_attr_setinheritsched()` 的 `attr` 参数值不指向已初始化的线程属性对象，则行为未定义。

## 返回值

如果成功，`pthread_attr_getinheritsched()` 和 `pthread_attr_setinheritsched()` 函数应返回零；否则，应返回错误编号以指示错误。

## 错误

`pthread_attr_setinheritsched()` 函数在以下情况下应失败：

- **[ENOTSUP]**
  - 尝试将属性设置为不支持的值。

`pthread_attr_setinheritsched()` 函数在以下情况下可能失败：

- **[EINVAL]**
  - `inheritsched` 的值无效。

这些函数不应返回 `[EINTR]` 错误码。

## 示例

无。

## 应用用法

设置这些属性后，可以使用 `pthread_create()` 创建具有指定属性的线程。使用这些例程不会影响当前运行的线程。

有关线程调度属性及其默认设置的更多详细信息，请参阅 [2.9.4 线程调度](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_04)。

## 原理

如果实现检测到传递给 `pthread_attr_getinheritsched()` 或 `pthread_attr_setinheritsched()` 的 `attr` 参数值不指向已初始化的线程属性对象，建议函数应失败并报告 `[EINVAL]` 错误。

## 未来方向

无。

## 参见

- [`pthread_attr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_destroy.html)
- [`pthread_attr_getscope()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getscope.html)
- [`pthread_attr_getschedpolicy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getschedpolicy.html)
- [`pthread_attr_getschedparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getschedparam.html)
- [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)
- XBD [`<sched.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sched.h.html)

## 变更历史

### 首次发布于 Issue 5。为与 POSIX 线程扩展保持一致而包含在内。
标记为实时线程特性组的一部分。

### Issue 6

`pthread_attr_getinheritsched()` 和 `pthread_attr_setinheritsched()` 函数被标记为线程和线程执行调度选项的一部分。

由于如果实现不支持线程执行调度选项，则不需要提供存根，因此移除了 `[ENOSYS]` 错误条件。

为与 ISO/IEC 9899:1999 标准保持一致，向 `pthread_attr_getinheritsched()` 原型添加了 `restrict` 关键字。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/75，澄清了描述中 `inheritsched` 的值，并在错误部分添加了两个可选的 `[EINVAL]` 错误，用于检查 `attr` 是否指向未初始化的线程属性对象。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/77，在应用用法部分添加了对 [2.9.4 线程调度](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_04) 的引用。

### Issue 7

`pthread_attr_getinheritsched()` 和 `pthread_attr_setinheritsched()` 函数仅被标记为线程执行调度选项的一部分，因为线程选项现在是基础的一部分。

移除了未初始化线程属性对象的 `[EINVAL]` 错误；此条件导致未定义行为。

应用 POSIX.1-2008，技术勘误 1，XSH/TC1-2008/0450 [314]。

应用 POSIX.1-2008，技术勘误 2，XSH/TC2-2008/0262 [757]。