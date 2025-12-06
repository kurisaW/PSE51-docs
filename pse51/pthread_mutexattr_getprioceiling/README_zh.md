# pthread_mutexattr_getprioceiling, pthread_mutexattr_setprioceiling

## SYNOPSIS（概要）

```c
#include <pthread.h>

int pthread_mutexattr_getprioceiling(const pthread_mutexattr_t *restrict attr,
                                   int *restrict prioceiling);

int pthread_mutexattr_setprioceiling(pthread_mutexattr_t *attr,
                                   int prioceiling);
```

## DESCRIPTION（描述）

`pthread_mutexattr_getprioceiling()` 和 `pthread_mutexattr_setprioceiling()` 函数分别用于获取和设置由 `attr` 指向的互斥锁属性对象的优先级上限属性，该对象先前由 `pthread_mutexattr_init()` 函数创建。

`prioceiling` 属性包含已初始化互斥锁的优先级上限。`prioceiling` 的值在 SCHED_FIFO 定义的最大优先级范围内。

`prioceiling` 属性定义了已初始化互斥锁的优先级上限，这是受互斥锁保护的临界区执行的最低优先级级别。为避免优先级反转（priority inversion），互斥锁的优先级上限应设置为高于或等于可能锁定该互斥锁的所有线程的最高优先级。`prioceiling` 的值在 SCHED_FIFO 调度策略下定义的最大优先级范围内。

如果传递给 `pthread_mutexattr_getprioceiling()` 或 `pthread_mutexattr_setprioceiling()` 的 `attr` 参数值不指向已初始化的互斥锁属性对象，则行为未定义。

## RETURN VALUE（返回值）

成功完成时，`pthread_mutexattr_getprioceiling()` 和 `pthread_mutexattr_setprioceiling()` 函数应返回零；否则，应返回错误编号以指示错误。

## ERRORS（错误）

这些函数可能失败，如果：

- **[EINVAL]**
  `prioceiling` 指定的值无效。

- **[EPERM]**
  调用者没有执行该操作的权限。

这些函数不应返回 [EINTR] 错误码。

*以下部分为提供信息之用。*

## EXAMPLES（示例）

无。

## APPLICATION USAGE（应用程序用法）

无。

## RATIONALE（基本原理）

如果实现检测到传递给 `pthread_mutexattr_getprioceiling()` 或 `pthread_mutexattr_setprioceiling()` 的 `attr` 参数值不指向已初始化的互斥锁属性对象，建议函数应失败并报告 [EINVAL] 错误。

## FUTURE DIRECTIONS（未来方向）

无。

## SEE ALSO（另见）

- `pthread_cond_destroy()`
- `pthread_create()`
- `pthread_mutex_destroy()`
- `<pthread.h>`

## CHANGE HISTORY（变更历史）

首次在 Issue 5 中发布。为与 POSIX 线程扩展对齐而包含。

标记为实时线程特性组的一部分。

### Issue 6

`pthread_mutexattr_getprioceiling()` 和 `pthread_mutexattr_setprioceiling()` 函数被标记为线程和线程优先级保护选项的一部分。

[ENOSYS] 错误条件已被移除，因为如果实现不支持线程优先级保护选项，不需要提供存根（stubs）。

[ENOTSUP] 错误条件已被移除，因为这些函数没有 `protocol` 参数。

为与 ISO/IEC 9899:1999 标准对齐，向 `pthread_mutexattr_getprioceiling()` 原型添加了 `restrict` 关键字。

### Issue 7

`pthread_mutexattr_getprioceiling()` 和 `pthread_mutexattr_setprioceiling()` 函数从线程选项移动到需要支持健壮互斥锁优先级保护选项或非健壮互斥锁优先级保护选项。

对于未初始化互斥锁属性对象的 [EINVAL] 错误被移除；此条件导致未定义行为。

---

*提供信息文本结束。*

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved