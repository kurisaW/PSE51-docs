# pthread_mutex_getprioceiling, pthread_mutex_setprioceiling

## SYNOPSIS

```c
#include <pthread.h>

int pthread_mutex_getprioceiling(const pthread_mutex_t *restrict mutex,
                                int *restrict prioceiling);

int pthread_mutex_setprioceiling(pthread_mutex_t *restrict mutex,
                                int prioceiling, int *restrict old_ceiling);
```

## DESCRIPTION

`pthread_mutex_getprioceiling()` 函数应返回互斥锁的当前优先级上限。

`pthread_mutex_setprioceiling()` 函数应尝试锁定互斥锁，如同调用 `pthread_mutex_lock()` 一样，但锁定互斥锁的过程不需要遵循优先级保护协议。在获取互斥锁后，它应更改互斥锁的优先级上限，然后如同调用 `pthread_mutex_unlock()` 一样释放互斥锁。当更改成功时，优先级上限的前一个值应返回到 `old_ceiling` 中。

如果 `pthread_mutex_setprioceiling()` 函数失败，互斥锁优先级上限不应被更改。

## RETURN VALUE

如果成功，`pthread_mutex_getprioceiling()` 和 `pthread_mutex_setprioceiling()` 函数应返回零；否则，应返回错误号以指示错误。

## ERRORS

这些函数在以下情况下应失败：

- **[EINVAL]**
  `mutex` 的协议属性是 PTHREAD_PRIO_NONE。

- **[EPERM]**
  实现需要适当的权限来执行操作，但调用者没有适当的权限。

`pthread_mutex_setprioceiling()` 函数在以下情况下应失败：

- **[EAGAIN]**
  由于 `mutex` 的递归锁最大数量已被超过，无法获取互斥锁。

- **[EAGAIN]**
  互斥锁是健壮互斥锁，并且可用的健壮互斥锁拥有的系统资源将被超出。

- **[EDEADLK]**
  互斥锁类型是 PTHREAD_MUTEX_ERRORCHECK，并且当前线程已经拥有该互斥锁。

- **[EINVAL]**
  互斥锁创建时协议属性值为 PTHREAD_PRIO_PROTECT，并且调用线程的优先级高于互斥锁的当前优先级上限，且实现在锁定互斥锁的过程中遵循优先级保护协议。

- **[ENOTRECOVERABLE]**
  互斥锁是健壮互斥锁，并且互斥锁保护的状态不可恢复。

- **[EOWNERDEAD]**
  互斥锁是健壮互斥锁，包含前一个拥有线程的进程在持有互斥锁时终止。调用线程应获取互斥锁，由新所有者负责使状态一致（参见 `pthread_mutex_lock()`）。

`pthread_mutex_setprioceiling()` 函数在以下情况下可能失败：

- **[EDEADLK]**
  检测到死锁条件。

- **[EINVAL]**
  `prioceiling` 请求的优先级超出范围。

- **[EOWNERDEAD]**
  互斥锁是健壮互斥锁，前一个拥有线程在持有互斥锁时终止。调用线程应获取互斥锁，由新所有者负责使状态一致（参见 `pthread_mutex_lock()`）。

这些函数不应返回 [EINTR] 错误代码。

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `pthread_mutex_clocklock()`
- `pthread_mutex_destroy()`
- `pthread_mutex_lock()`
- `<pthread.h>`

## CHANGE HISTORY

### 首次发布于 Issue 5
为了与 POSIX 线程扩展对齐而包含。标记为实时线程特性组的一部分。

### Issue 6
- `pthread_mutex_getprioceiling()` 和 `pthread_mutex_setprioceiling()` 函数标记为线程和线程优先级保护选项的一部分。
- 移除了 [ENOSYS] 错误条件。
- 为与 IEEE Std 1003.1d-1999 对齐，在 SEE ALSO 部分添加了 `pthread_mutex_timedlock()` 函数。
- 为与 ISO/IEC 9899:1999 标准对齐，在 `pthread_mutex_getprioceiling()` 和 `pthread_mutex_setprioceiling()` 原型中添加了 `restrict` 关键字。

### Issue 7
- 应用了 SD5-XSH-ERN-39。
- 应用了 Austin Group Interpretation 1003.1-2001 #052，添加 [EDEADLK] 作为"可能失败"错误。
- 应用了 SD5-XSH-ERN-158，更新 ERRORS 部分以包含当 `mutex` 的协议属性为 PTHREAD_PRIO_NONE 时的"应失败"错误情况。
- `pthread_mutex_getprioceiling()` 和 `pthread_mutex_setprioceiling()` 函数从线程选项移至需要支持健壮互斥锁优先级保护选项或非健壮互斥锁优先级保护选项。
- 更新了 DESCRIPTION 和 ERRORS 部分，以正确考虑所有各种互斥锁类型。

### Issue 8
- 应用了 Austin Group Defect 354，添加了超出可用的健壮互斥锁拥有的系统资源时的 [EAGAIN] 错误。

---
