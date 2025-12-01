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

`inheritsched` 支持的值包括：

- **PTHREAD_INHERIT_SCHED**
  指定线程调度属性应继承自创建线程，而此 `attr` 参数中的调度属性应被忽略。

- **PTHREAD_EXPLICIT_SCHED**
  指定线程调度属性应设置为此属性对象中的相应值。

符号 PTHREAD_INHERIT_SCHED 和 PTHREAD_EXPLICIT_SCHED 在 `<pthread.h>` 头文件中定义。

由 POSIX.1-2024 定义的以下线程调度属性受 `inheritsched` 属性影响：调度策略 (`schedpolicy`)、调度参数 (`schedparam`) 和调度竞争范围 (`contentionscope`)。

如果传递给 `pthread_attr_getinheritsched()` 或 `pthread_attr_setinheritsched()` 的 `attr` 参数值不引用已初始化的线程属性对象，则行为未定义。

## 返回值

如果成功，`pthread_attr_getinheritsched()` 和 `pthread_attr_setinheritsched()` 函数应返回零；否则，应返回错误号以指示错误。

## 错误

`pthread_attr_setinheritsched()` 函数在以下情况可能失败：

- **[ENOTSUP]**
  试图将属性设置为不支持的值。

`pthread_attr_setinheritsched()` 函数在以下情况可能失败：

- **[EINVAL]**
  `inheritsched` 的值无效。

这些函数不应返回 `[EINTR]` 错误码。

## 示例

无。

## 应用程序使用

设置这些属性后，可以使用 `pthread_create()` 创建具有指定属性的线程。使用这些例程不会影响当前运行的线程。

有关线程调度属性及其默认设置的更多详细信息，请参见 [2.9.4 线程调度](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_04)。

## 原理

如果实现检测到传递给 `pthread_attr_getinheritsched()` 或 `pthread_attr_setinheritsched()` 的 `attr` 参数值不引用已初始化的线程属性对象，建议函数应失败并报告 `[EINVAL]` 错误。

## 未来方向

无。

## 另请参见

- `pthread_attr_destroy()`
- `pthread_attr_getscope()`
- `pthread_attr_getschedpolicy()`
- `pthread_attr_getschedparam()`
- `pthread_create()`
- `<pthread.h>`
- `<sched.h>`

## 变更历史

首次发布于 Issue 5。为与 POSIX 线程扩展对齐而包含。

标记为实时线程特性组的一部分。

### Issue 6

`pthread_attr_getinheritsched()` 和 `pthread_attr_setinheritsched()` 函数被标记为线程和线程执行调度选项的一部分。

`[ENOSYS]` 错误条件已被移除，因为如果实现不支持线程执行调度选项，则无需提供存根。

`restrict` 关键字被添加到 `pthread_attr_getinheritsched()` 原型中，以与 ISO/IEC 9899:1999 标准对齐。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/75，阐明了描述中 `inheritsched` 的值，并向错误部分添加了两个可选的 `[EINVAL]` 错误，用于检查 `attr` 是否引用未初始化的线程属性对象。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/77，在应用程序使用部分添加了对 [2.9.4 线程调度](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_04) 的引用。

### Issue 7

`pthread_attr_getinheritsched()` 和 `pthread_attr_setinheritsched()` 函数仅被标记为线程执行调度选项的一部分，因为线程选项现在是基础的一部分。

针对未初始化线程属性对象的 `[EINVAL]` 错误已被移除；这种情况会导致未定义行为。

应用了 POSIX.1-2008，技术更正 1，XSH/TC1-2008/0450 [314]。

应用了 POSIX.1-2008，技术更正 2，XSH/TC2-2008/0262 [757]。