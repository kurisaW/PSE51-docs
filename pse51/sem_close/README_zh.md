# sem_close — 关闭命名信号量

## SYNOPSIS

```c
#include <semaphore.h>

int sem_close(sem_t *sem);
```

## DESCRIPTION

`sem_close()` 函数应表示调用进程已完成使用由 `sem` 指示的命名信号量。对无名信号量（由 `sem_init()` 创建的信号量）调用 `sem_close()` 的效果是未定义的。`sem_close()` 函数应释放（即使其可用于此进程后续的 `sem_open()` 调用重用）系统为此进程使用此信号量分配的任何系统资源。如果由 `sem` 指示的信号量是使用文件描述符实现的，则文件描述符应被关闭。此进程后续使用由 `sem` 指示的信号量的效果是未定义的。如果调用进程中的任何线程当前在信号量上被阻塞，则行为是未定义的。如果信号量尚未通过成功的 `sem_unlink()` 调用移除，那么 `sem_close()` 对信号量的状态没有影响。如果在此信号量最近一次使用 O_CREAT 调用 `sem_open()` 后，已成功为 `name` 调用了 `sem_unlink()` 函数，那么当所有打开该信号量的进程关闭所有信号量句柄时，该信号量将不再可访问。

## RETURN VALUE

成功完成时，应返回零值。否则，应返回 -1 值并设置 `errno` 来指示错误。

## ERRORS

`sem_close()` 函数可能在以下情况下失败：

- **[EINVAL]**

  `sem` 参数不是有效的信号量描述符。

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

[`semctl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semctl.html), [`semget()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semget.html), [`semop()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semop.html), [`sem_init()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_init.html), [`sem_open()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_open.html), [`sem_unlink()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_unlink.html)

XBD [`<semaphore.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/semaphore.h.html)

## CHANGE HISTORY

### Issue 6

`sem_close()` 函数被标记为信号量选项的一部分。

[ENOSYS] 错误条件已被移除，因为如果实现不支持信号量选项，则不需要提供存根。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/113，更新 ERRORS 部分，使 [EINVAL] 错误变为可选的。

### Issue 7

`sem_close()` 函数从信号量选项移动到基础。

应用 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0523 [37]。

应用 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0317 [870]。

### Issue 8

应用 Austin Group Defect 368，添加了如果 `sem` 使用文件描述符实现，则 `sem_close()` 关闭文件描述符的要求。

应用 Austin Group Defect 1324，阐明了已取消链接的信号量不再可访问的情况。

---
