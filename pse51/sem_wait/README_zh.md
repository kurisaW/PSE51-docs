# sem_trywait, sem_wait — 锁定信号量

## 概要

```c
#include <semaphore.h>

int sem_trywait(sem_t *sem);
int sem_wait(sem_t *sem);
```

## 描述

`sem_trywait()` 函数只有在信号量当前未被锁定时才锁定由 `sem` 引用的信号量；也就是说，只有当信号量当前值为正数时才锁定。否则，该函数不会锁定信号量。

`sem_wait()` 函数通过对该信号量执行信号量锁定操作来锁定由 `sem` 引用的信号量。如果信号量当前值为零，那么调用线程在能够锁定信号量或调用被信号中断之前，不会从 `sem_wait()` 调用中返回。

成功返回时，信号量的状态应被锁定，并将保持锁定状态，直到 `sem_post()` 函数被执行并成功返回。

`sem_wait()` 函数可被信号的传送中断。

## 返回值

如果调用进程成功对指定信号量 `sem` 执行了信号量锁定操作，`sem_trywait()` 和 `sem_wait()` 函数应返回零。如果调用不成功，信号量的状态应保持不变，函数应返回值 -1，并设置 `errno` 来指示错误。

## 错误

`sem_trywait()` 函数在以下情况下应失败：

- **[EAGAIN]**
  信号量已被锁定，因此无法通过 `sem_trywait()` 操作立即锁定。

`sem_trywait()` 和 `sem_wait()` 函数在以下情况下可能失败：

- **[EDEADLK]**
  检测到死锁条件。

- **[EINTR]**
  信号中断了此函数。

- **[EINVAL]**
  `sem` 参数不引用有效的信号量。

---

## 应用程序使用

使用这些函数的应用程序可能会受到优先级反转的影响，如 XBD 3.275 优先级反转中所述。

## 原理

无。

## 未来方向

无。

## 参见

- [`semctl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semctl.html)
- [`semget()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semget.html)
- [`semop()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semop.html)
- [`sem_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_clockwait.html)
- [`sem_post()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_post.html)

XBD 3.275 优先级反转，4.15.2 内存同步，`<semaphore.h>`

## 变更历史

### 首次发布于 Issue 5
包含以与 POSIX 实时扩展对齐。

### Issue 6
- `sem_trywait()` 和 `sem_wait()` 函数被标记为信号量选项的一部分。
- [ENOSYS] 错误条件已被移除，因为如果实现不支持信号量选项，则无需提供存根。
- `sem_timedwait()` 函数被添加到参见部分，以与 IEEE Std 1003.1d-1999 对齐。
- 应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/121，更新错误部分，使 [EINVAL] 错误变为可选。

### Issue 7
- 应用 SD5-XSH-ERN-54，将 `sem_wait()` 函数从"应失败"错误情况中移除。
- `sem_trywait()` 和 `sem_wait()` 函数从信号量选项移至基础部分。
- 应用 POSIX.1-2008，技术勘误 1，XSH/TC1-2008/0530 [37]。

---
