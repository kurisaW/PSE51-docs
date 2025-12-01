# sem_open — 初始化并打开命名信号量

## 概要

```c
#include <semaphore.h>

sem_t *sem_open(const char *name, int oflag, ...);
```

## 描述

`sem_open()` 函数应在命名信号量和进程之间建立连接。命名信号量可以使用文件描��符实现。在以信号量名称 `name` 调用 `sem_open()` 后，进程可以使用该调用返回的信号量句柄引用与 `name` 关联的信号量。此信号量可用于后续调用 [`sem_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_clockwait.html)、[`sem_close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_close.html)、[`sem_post()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_post.html)、[`sem_timedwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_timedwait.html)、[`sem_trywait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_trywait.html) 和 [`sem_wait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_wait.html)。该信号量保持对此进程可用，直到通过成功调用 [`sem_close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_close.html)、[`_exit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/_exit.html) 或 [exec](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html) 函数之一关闭信号量。

`oflag` 参数控制信号量是被创建还是仅被 `sem_open()` 调用访问。以下标志位可以在 `oflag` 中设置：

### O_CREAT

如果信号量尚不存在，此标志用于创建信号量。如果设置了 O_CREAT 且信号量已存在，则 O_CREAT 无效，O_EXCL 下注明的除外。否则，`sem_open()` 创建命名信号量。O_CREAT 标志需要第三个和第四个参数：`mode`（类型为 `mode_t`）和 `value`（类型为 `unsigned`）。信号量以 `value` 作为初始值创建。信号量的有效初始值小于或等于 {SEM_VALUE_MAX}。

信号量的用户 ID 应设置为进程的有效用户 ID。信号量的组 ID 应设置为进程的有效组 ID；但是，如果 `name` 参数在文件系统中可见，组 ID 可以设置为包含目录的组 ID。信号量的权限位设置为 `mode` 参数的值，进程文件模式创建掩码中设置的位除外。当指定了文件权限位之外的 `mode` 位时，效果是未指定的。

在使用 O_CREAT 标志通过 `sem_open()` 创建名为 `name` 的信号量后，其他进程可以通过调用 `sem_open()` 并使用相同的 `name` 值连接到该信号量。

### O_EXCL

如果设置了 O_EXCL 和 O_CREAT，且信号量 `name` 已存在，则 `sem_open()` 失败。对于其他执行带有 O_EXCL 和 O_CREAT 设置的 `sem_open()` 的进程，检查信号量是否存在以及信号量不存在时创建信号量的操作是原子的。如果设置了 O_EXCL 但未设置 O_CREAT，则效果是未定义的。

如果在 `oflag` 参数中指定了 O_CREAT 和 O_EXCL 之外的标志，则效果是未指定的。

`name` 参数指向命名信号量对象的字符串。名称是否出现在文件系统中并对以路径名为参数的函数可见是未指定的。`name` 参数符合路径名的构造规则，但 `name` 中除前导 `<slash>` 字符外的 `<slash>` 字符的解释是实现定义的，并且 `name` 参数的长度限制是实现定义的，不必与路径名限制 {PATH_MAX} 和 {NAME_MAX} 相同。如果 `name` 以 `<slash>` 字符开头，则调用 `sem_open()` 且具有相同 `name` 值的进程应引用同一信号量对象，只要该名称尚未被删除。如果 `name` 不以 `<slash>` 字符开头，则效果是实现定义的。

如果一个进程对相同的 `name` 值进行了多次成功的 `sem_open()` 调用，期间没有对 `name` 进行 [`sem_unlink()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_unlink.html) 调用，并且该信号量的至少一个打开句柄尚未通过 [`sem_close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_close.html) 调用关闭，则对于每次此类成功调用是返回相同句柄还是唯一句柄是实现定义的。

对信号量副本的引用会产生未定义结果。

## 返回值

成功完成后，`sem_open()` 函数应返回信号量的地址。否则，应返回 SEM_FAILED 值并设置 `errno` 以指示错误。符号 SEM_FAILED 在 [`<semaphore.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/semaphore.h.html) 头文件中定义。`sem_open()` 的成功返回不应返回 SEM_FAILED 值。

## 错误

如果出现以下任何条件，`sem_open()` 函数应返回 SEM_FAILED 并将 `errno` 设置为相应的值：

- **[EACCES]** 命名信号量存在且 `oflag` 指定的权限被拒绝，或者命名信号量不存在且创建命名信号量的权限被拒绝。
- **[EEXIST]** 设置了 O_CREAT 和 O_EXCL 且命名信号量已存在。
- **[EINTR]** `sem_open()` 操作被信号中断。
- **[EINVAL]** 给定名称不支持 `sem_open()` 操作，或者在 `oflag` 中指定了 O_CREAT 且 `value` 大于 {SEM_VALUE_MAX}。
- **[ENOENT]** 未设置 O_CREAT 且命名信号量不存在。
- **[ENOMEM]** 创建新命名信号量的内存不足。
- **[ENOSPC]** 存储设备上没有足够的空间来创建新命名信号量。

如果出现以下任何条件，`sem_open()` 函数可能返回 SEM_FAILED 并将 `errno` 设置为相应的值：

- **[EMFILE]** 此进程当前使用的信号量描述符或文件描述符过多。
- **[ENAMETOOLONG]** 在不支持 XSI 选项 [XSI] 的系统上，`name` 参数的长度超过 {_POSIX_PATH_MAX}，在 XSI 系统上超过 {_XOPEN_PATH_MAX}，或者在非 XSI 系统上路径名组件长度超过 {_POSIX_NAME_MAX}，在 XSI 系统上超过 {_XOPEN_NAME_MAX}。
- **[ENFILE]** 系统中当前打开的信号量描述符或文件描述符过多。

## 示例

无。

## 应用程序用法

无。

## 原理

早期草案要求 `sem_open()` 函数返回类型为 `sem_t *` 的错误返回值 -1，这不能保证在实现之间可移植。修订后的文本提供符号错误代码 SEM_FAILED 以消除类型冲突。

## 未来方向

未来版本可能要求 `sem_open()` 和 [`sem_unlink()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_unlink.html) 函数具有类似于普通文件系统操作的语义。

## 另请参阅

[`semctl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semctl.html)、[`semget()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semget.html)、[`semop()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semop.html)、[`sem_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_clockwait.html)、[`sem_close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_close.html)、[`sem_post()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_post.html)、[`sem_trywait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_trywait.html)、[`sem_unlink()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_unlink.html)

XBD [`<semaphore.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/semaphore.h.html)

## 更改历史

首次在 Issue 5 中发布。包含用于与 POSIX 实时扩展对齐。

### Issue 6

`sem_open()` 函数被标记为信号量选项的一部分。

[ENOSYS] 错误条件已被移除，因为如果实现不支持信号量选项，则不需要提供存根。

[`sem_timedwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_timedwait.html) 函数已添加到另请参阅部分，以与 IEEE Std 1003.1d-1999 对齐。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/117，更新描述以添加 [`sem_timedwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_timedwait.html) 函数，以与 IEEE Std 1003.1d-1999 对齐。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/118，更新描述以描述在调用 `sem_open()` 时返回相同信号量地址的条件。添加了"并且至少一次先前的成功 `sem_open()` 调用未与 [`sem_close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_close.html) 调用匹配"的文字。

### Issue 7

应用了 Austin Group Interpretation 1003.1-2001 #066，更新了 [ENOSPC] 错误情况并添加了 [ENOMEM] 错误情况。

应用了 Austin Group Interpretation 1003.1-2001 #077，阐明了 `name` 参数并添加了 [ENAMETOOLONG] 作为"可能失败"错误。

应用了 Austin Group Interpretation 1003.1-2001 #141，添加了未来方向。

应用了 SD5-XSH-ERN-170，更新描述以阐明设置信号量用户 ID 和组 ID 的措辞。

`sem_open()` 函数从信号量选项移动到基础。

应用了 POSIX.1-2008，Technical Corrigendum 1，XSH/TC1-2008/0527 [37]。

### Issue 8

应用了 Austin Group Defect 368，添加了命名信号量可以使用文件描述符实现的语句并更改了错误部分。

应用了 Austin Group Defect 1216，添加了 [`sem_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_clockwait.html)。

应用了 Austin Group Defect 1324，当对相同的 `name` 值进行多次成功的 `sem_open()` 调用时，是返回相同句柄还是唯一句柄成为实现定义的。