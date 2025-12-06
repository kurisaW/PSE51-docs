# sem_unlink - 删除命名信号量

## 概要

```c
#include <semaphore.h>

int sem_unlink(const char *name);
```

## 描述

`sem_unlink()` 函数应删除由字符串 `name` 命名的信号量。如果名为 `name` 的信号量当前被其他进程引用，那么 `sem_unlink()` 对信号量的状态不应产生影响。如果在调用 `sem_unlink()` 时有一个或多个进程打开该信号量，则信号量的销毁将被推迟，直到所有对信号量的引用都通过调用 `sem_close()`、`_exit()` 或 `exec` 被销毁。在调用 `sem_unlink()` 后，调用 `sem_open()` 重新创建或重新连接信号量将引用一个新的信号量。`sem_unlink()` 调用不会阻塞直到所有引用都被销毁；它应立即返回。

## 返回值

成功完成时，`sem_unlink()` 函数应返回值 0。否则，信号量不应改变，函数应返回值 -1 并设置 `errno` 以指示错误。

## 错误

在以下情况下，`sem_unlink()` 函数应失败：

- **[EACCES]** - 拒绝解除链接命名信号量的权限。
- **[ENOENT]** - 命名信号量不存在。

在以下情况下，`sem_unlink()` 函数可能失败：

- **[ENAMETOOLONG]** - `name` 参数的长度在不支持 XSI 选项的系统上超过 {_POSIX_PATH_MAX}，在 XSI 系统上超过 {_XOPEN_PATH_MAX}，或者包含的路径名组件在不支持 XSI 选项的系统上长于 {_POSIX_NAME_MAX}，在 XSI 系统上长于 {_XOPEN_NAME_MAX}。使用包含与先前成功的 `sem_open()` 调用中使用的相同信号量名称的 `name` 参数调用 `sem_unlink()` 不应产生 [ENAMETOOLONG] 错误。

## 示例

无。

## 应用用法

无。

## 基本原理

无。

## 未来方向

未来版本可能要求 `sem_open()` 和 `sem_unlink()` 函数具有类似于普通文件系统操作的语义。

## 另请参阅

- [`semctl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semctl.html)
- [`semget()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semget.html)
- [`semop()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semop.html)
- [`sem_close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_close.html)
- [`sem_open()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_open.html)
- XBD [`<semaphore.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/semaphore.h.html)

## 变更历史

### 第 5 版首次发布
为了与 POSIX 实时扩展保持一致而包含。

### 第 6 版
- `sem_unlink()` 函数被标记为信号量选项的一部分。
- [ENOSYS] 错误条件已被删除，因为如果实现不支持信号量选项，则无需提供存根。

### 第 7 版
- 应用 Austin Group 解释 1003.1-2001 #077，将 [ENAMETOOLONG] 从"应失败"错误改为"可能失败"错误。
- 应用 Austin Group 解释 1003.1-2001 #141，添加未来方向。
- `sem_unlink()` 函数从信号量选项移至基础部分。
- 应用 POSIX.1-2008，技术勘误表 1，XSH/TC1-2008/0531 [37]。

---
