# sem_getvalue

## SYNOPSIS

```c
#include <semaphore.h>

int sem_getvalue(sem_t *restrict sem, int *restrict sval);
```

## DESCRIPTION

`sem_getvalue()` 函数应将 `sval` 参数引用的位置更新为 `sem` 引用的信号量的值，而不影响信号量的状态。更新后的值表示在调用期间某个未指定时刻发生的实际信号量值，但不一定是返回给调用进程时的实际信号量值。

如果 `sem` 被锁定，那么 `sval` 指向的对象要么被设置为零，要么被设置为一个负数，其绝对值表示在调用期间某个未指定时刻等待信号量的进程数。

## RETURN VALUE

成功完成后，`sem_getvalue()` 函数应返回零值。否则，它应返回 -1 值并设置 `errno` 来指示错误。

## ERRORS

`sem_getvalue()` 函数可能会失败，原因如下：

- **[EINVAL]** —— `sem` 参数不引用有效的信号量。

---

*以下为补充信息部分。*

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

��。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`semctl()`](semctl.html)
- [`semget()`](semget.html)
- [`semop()`](semop.html)
- [`sem_clockwait()`](sem_clockwait.html)
- [`sem_post()`](sem_post.html)
- [`sem_trywait()`](sem_trywait.html)

XBD [`<semaphore.h>`](../basedefs/semaphore.h.html)

## CHANGE HISTORY

### 首次发布于 Issue 5。
为了与 POSIX 实时扩展对齐而包含。

### Issue 6

`sem_getvalue()` 函数被标记为信号量选项的一部分。

如果实现不支持信号量选项，则不需要提供存根，因此 [ENOSYS] 错误条件已被移除。

为了与 IEEE Std 1003.1d-1999 对齐，[`sem_timedwait()`](sem_timedwait.html) 函数被添加到 SEE ALSO 部分。

为了与 ISO/IEC 9899:1999 标准对齐，`sem_getvalue()` 原型中添加了 **restrict** 关键字。

应用了 IEEE Std 1003.1-2001/Cor 1-2002，项目 XSH/TC1/D6/54。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/115，更新了 ERRORS 部分，使得 [EINVAL] 错误变为可选的。

### Issue 7

`sem_getvalue()` 函数从信号量选项移动到 Base。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0525 [37]。

---
