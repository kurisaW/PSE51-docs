# sem_trywait, sem_wait — 锁定信号量

## SYNOPSIS（概要）

```c
#include <semaphore.h>

int sem_trywait(sem_t *sem);
int sem_wait(sem_t *sem);
```

## DESCRIPTION（描述）

`sem_trywait()` 函数仅在信号量当前未被锁定时才锁定 `sem` 所引用的信号量；也就是说，仅当信号量值当前为正数时才锁定。否则，该函数不应锁定该信号量。

`sem_wait()` 函数通过在该信号量上执行信号量锁定操作来锁定 `sem` 所引用的信号量。如果信号量值当前为零，那么调用线程在锁定该信号量或调用被信号中断之前，不应从 `sem_wait()` 调用中返回。

成功返回时，信号量的状态应为已锁定，并且应保持锁定状态，直到 `sem_post()` 函数被执行并成功返回。

`sem_wait()` 函数可被信号传递中断。

## RETURN VALUE（返回值）

如果调用进程在 `sem` 指定的信号量上成功执行了信号量锁定操作，`sem_trywait()` 和 `sem_wait()` 函数应返回零。如果调用不成功，信号量的状态应保持不变，函数应返回值 -1 并设置 `errno` 以指示错误。

## ERRORS（错误）

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

## APPLICATION USAGE（应用程序使用）

使用这些函数的应用程序可能会受到优先级反转的影响，如 XBD 3.275 优先级反转中所述。

## SEE ALSO（另请参见）

- [`semctl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semctl.html)
- [`semget()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semget.html)
- [`semop()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semop.html)
- [`sem_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_clockwait.html)
- [`sem_post()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_post.html)

XBD：
- [3.275 优先级反转](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap03.html#tag_03_275)
- [4.15.2 内存同步](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap04.html#tag_04_15_02)
- [<semaphore.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/semaphore.h.html)

## CHANGE HISTORY（变更历史）

**Issue 6**
- `sem_trywait()` 和 `sem_wait()` 函数被标记为信号量选项的一部分。
- 如果实现不支持信号量选项，则不需要提供存根，因此移除了 [ENOSYS] 错误条件。
- 为了与 IEEE Std 1003.1d-1999 对齐，在另请参见部分添加了 `sem_timedwait()` 函数。
- 应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/121，更新了错误部分，使 [EINVAL] 错误变为可选的。

**Issue 7**
- 应用了 SD5-XSH-ERN-54，从"应失败"错误情况中移除了 `sem_wait()` 函数。
- `sem_trywait()` 和 `sem_wait()` 函数从信号量选项移动到基本定义。
- 应用了 POSIX.1-2008，技术勘误 1，XSH/TC1-2008/0530 [37]。

---
