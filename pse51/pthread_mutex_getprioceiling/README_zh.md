# pthread_mutex_getprioceiling, pthread_mutex_setprioceiling

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## 名称（NAME）

pthread_mutex_getprioceiling, pthread_mutex_setprioceiling — 获取和设置互斥锁的优先级上限（**实时线程**）

## 概要（SYNOPSIS）

```c
[RPP|TPP]
#include <pthread.h>

int pthread_mutex_getprioceiling(const pthread_mutex_t *restrict mutex,
                                int *restrict prioceiling);

int pthread_mutex_setprioceiling(pthread_mutex_t *restrict mutex,
                                int prioceiling, int *restrict old_ceiling);
```

## 描述（DESCRIPTION）

`pthread_mutex_getprioceiling()` 函数应返回互斥锁的当前优先级上限。

`pthread_mutex_setprioceiling()` 函数应尝试锁定互斥锁，如同调用 `pthread_mutex_lock()` 一样，但锁定互斥锁的过程无需遵循优先级保护协议。在获取互斥锁后，它应更改互斥锁的优先级上限，然后如同调用 `pthread_mutex_unlock()` 一样释放互斥锁。当更改成功时，优先级上限的先前值应在 `old_ceiling` 中返回。

如果 `pthread_mutex_setprioceiling()` 函数失败，互斥锁优先级上限不应被更改。

## 返回值（RETURN VALUE）

如果成功，`pthread_mutex_getprioceiling()` 和 `pthread_mutex_setprioceiling()` 函数应返回零；否则，应返回错误号以指示错误。

## 错误（ERRORS）

这些函数应在以下情况失败：

- **[EINVAL]**
  `mutex` 的协议属性是 PTHREAD_PRIO_NONE。

- **[EPERM]**
  实现需要适当的权限来执行操作，而调用者没有适当的权限。

`pthread_mutex_setprioceiling()` 函数应在以下情况失败：

- **[EAGAIN]**
  由于已超过 `mutex` 的最大递归锁定次数，无法获取互斥锁。

- **[EAGAIN]**
  互斥锁是健壮互斥锁，且可用于拥有健壮互斥锁的系统资源将被超出。

- **[EDEADLK]**
  互斥锁类型是 PTHREAD_MUTEX_ERRORCHECK，且当前线程已经拥有该互斥锁。

- **[EINVAL]**
  互斥锁创建时协议属性值为 PTHREAD_PRIO_PROTECT，且调用线程的优先级高于互斥锁的当前优先级上限，并且实现在锁定互斥锁的过程中遵循优先级保护协议。

- **[ENOTRECOVERABLE]**
  互斥锁是健壮互斥锁，且受互斥锁保护的状态不可恢复。

- **[EOWNERDEAD]**
  互斥锁是健壮互斥锁，且包含先前拥有线程的进程在持有互斥锁时终止。互斥锁应由调用线程获取，并由新所有者负责使状态一致（参见 `pthread_mutex_lock()`）。

`pthread_mutex_setprioceiling()` 函数可能在以下情况失败：

- **[EDEADLK]**
  检测到死锁条件。

- **[EINVAL]**
  由 `prioceiling` 请求的优先级超出范围。

- **[EOWNERDEAD]**
  互斥锁是健壮互斥锁，且先前拥有线程在持有互斥锁时终止。互斥锁应由调用线程获取，并由新所有者负责使状态一致（参见 `pthread_mutex_lock()`）。

这些函数不应返回 [EINTR] 错误代码。

---

## 示例（EXAMPLES）

无。

## 应用程序用法（APPLICATION USAGE）

无。

## 基本原理（RATIONALE）

无。

## 未来方向（FUTURE DIRECTIONS）

无。

## 另请参见（SEE ALSO）

- `pthread_mutex_clocklock()`
- `pthread_mutex_destroy()`
- `pthread_mutex_lock()`
- XBD `<pthread.h>`

## 变更历史（CHANGE HISTORY）

### 首次发布于 Issue 5
为与 POSIX 线程扩展对齐而包含。
标记为实时线程特性组的一部分。

### Issue 6
`pthread_mutex_getprioceiling()` 和 `pthread_mutex_setprioceiling()` 函数被标记为线程和线程优先级保护选项的一部分。
删除了 [ENOSYS] 错误条件。
为与 IEEE Std 1003.1d-1999 对齐，将 `pthread_mutex_timedlock()` 函数添加到另请参见部分。
为与 ISO/IEC 9899:1999 标准对齐，向 `pthread_mutex_getprioceiling()` 和 `pthread_mutex_setprioceiling()` 原型添加了 **restrict** 关键字。

### Issue 7
应用了 SD5-XSH-ERN-39。
应用了 Austin Group Interpretation 1003.1-2001 #052，添加 [EDEADLK] 作为"可能失败"错误。
应用了 SD5-XSH-ERN-158，更新错误部分以包含当 `mutex` 的协议属性为 PTHREAD_PRIO_NONE 时的"应失败"错误情况。
`pthread_mutex_getprioceiling()` 和 `pthread_mutex_setprioceiling()` 函数从线程选项移至需要支持健壮互斥锁优先级保护选项或非健壮互斥锁优先级保护选项。
更新描述和错误部分以正确处理所有各种互斥锁类型。

### Issue 8
应用了 Austin Group Defect 354，为超出可用于拥有健壮互斥锁的系统资源添加 [EAGAIN] 错误。