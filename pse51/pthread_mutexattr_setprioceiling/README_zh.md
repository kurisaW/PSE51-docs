# pthread_mutexattr_getprioceiling, pthread_mutexattr_setprioceiling

**The Open Group Base Specifications Issue 8**
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## 名称

pthread_mutexattr_getprioceiling, pthread_mutexattr_setprioceiling — 获取和设置互斥属性对象的优先级上限属性 (**实时线程**)

## 概要

```c
#include <pthread.h>

int pthread_mutexattr_getprioceiling(const pthread_mutexattr_t *restrict attr,
                                     int *restrict prioceiling);

int pthread_mutexattr_setprioceiling(pthread_mutexattr_t *attr,
                                     int prioceiling);
```

## 描述

`pthread_mutexattr_getprioceiling()` 和 `pthread_mutexattr_setprioceiling()` 函数分别用于获取和设置由 `attr` 指向的互斥属性对象的优先级上限属性，该对象先前由 [`pthread_mutexattr_init()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_init.html) 函数创建。

`prioceiling` 属性包含已初始化互斥的优先级上限。`prioceiling` 的值在 SCHED_FIFO 定义的最大优先级范围内。

`prioceiling` 属性定义已初始化互斥的优先级上限，这是执行互斥保护的临界区的最低优先级级别。为避免优先级反转，互斥的优先级上限应设置为高于或等于可能锁定该互斥的所有线程的最高优先级。`prioceiling` 的值在 SCHED_FIFO 调度策略下定义的最大优先级范围内。

如果 `pthread_mutexattr_getprioceiling()` 或 `pthread_mutexattr_setprioceiling()` 的 `attr` 参数指定的值不引用已初始化的互斥属性对象，则行为未定义。

## 返回值

成功完成后，`pthread_mutexattr_getprioceiling()` 和 `pthread_mutexattr_setprioceiling()` 函数应返回零；否则，应返回错误编号以指示错误。

## 错误

这些函数可能在以下情况下失败：

- **[EINVAL]**
  `prioceiling` 指定的值无效。

- **[EPERM]**
  调用者没有执行该操作的权限。

这些函数不应返回 [EINTR] 错误代码。

---

*以下部分为参考信息。*

## 示例

无。

## 应用程序用法

无。

## 原理说明

如果实现检测到 `pthread_mutexattr_getprioceiling()` 或 `pthread_mutexattr_setprioceiling()` 的 `attr` 参数指定的值不引用已初始化的互斥属性对象，建议函数应失败并报告 [EINVAL] 错误。

## 未来方向

无。

## 另请参阅

[`pthread_cond_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_destroy.html), [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html), [`pthread_mutex_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_destroy.html)

XBD [<pthread.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## 变更历史

首次在 Issue 5 中发布。为与 POSIX 线程扩展对齐而包含。

标记为实时线程特性组的一部分。

### Issue 6

`pthread_mutexattr_getprioceiling()` 和 `pthread_mutexattr_setprioceiling()` 函数被标记为线程和线程优先级保护选项的一部分。

[ENOSYS] 错误条件已被移除，因为如果实现不支持线程优先级保护选项，则不需要提供存根。

[ENOTSUP] 错误条件已被移除，因为这些函数没有 `protocol` 参数。

为与 ISO/IEC 9899:1999 标准对齐，向 `pthread_mutexattr_getprioceiling()` 原型添加了 **restrict** 关键字。

### Issue 7

`pthread_mutexattr_getprioceiling()` 和 `pthread_mutexattr_setprioceiling()` 函数从线程选项移动到需要支持健壮互斥优先级保护选项或非健壮互斥优先级保护选项。

针对未初始化互斥属性对象的 [EINVAL] 错误已被移除；此条件导致未定义行为。

