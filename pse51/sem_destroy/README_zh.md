# sem_destroy — 销毁无名信号量

## 概要

```c
#include <semaphore.h>

int sem_destroy(sem_t *sem);
```

## 描述

`sem_destroy()` 函数应当销毁由 `sem` 指示的无名信号量。如果无��信号量使用文件描述符实现，则文件描述符应当被关闭。只有使用 `sem_init()` 创建的信号量才能使用 `sem_destroy()` 销毁；使用有名信号量调用 `sem_destroy()` 的效果是未定义的。在 `sem` 被另一次 `sem_init()` 调用重新初始化之前，后续使用信号量 `sem` 的效果是未定义的。

在没有线程当前被阻塞的已初始化信号量上销毁它是安全的。在有其他线程当前被阻塞的信号量上销毁的效果是未定义的。

## 返回值

成功完成后，应返回零值。否则，应返回 -1 值并设置 `errno` 来指示错误。

## 错误

`sem_destroy()` 函数可能失败的情况：

- **[EINVAL]**
  `sem` 参数不是有效的信号量。

- **[EBUSY]**
  当前有进程在该信号量上阻塞。

## 示例

无。

## 应用用法

无。

## 基本原理

无。

## 未来方向

无���

## 参见

- [`semctl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semctl.html)
- [`semget()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semget.html)
- [`semop()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semop.html)
- [`sem_init()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_init.html)
- [`sem_open()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_open.html)

XBD [`<semaphore.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/semaphore.h.html)

## 变更历史

### 首次发布于 Issue 5
为与 POSIX 实时扩展对齐而包含。

### Issue 6
- `sem_destroy()` 函数被标记为信号量选项的一部分。
- 如果实现不支持信号量选项，不需要提供存根，因此 `[ENOSYS]` 错误条件已被移除。
- 应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/114，更新了错误部分，使 `[EINVAL]` 错误变为可选的。

### Issue 7
- `sem_destroy()` 函数从信号量选项移至基本规范。
- 应用了 POSIX.1-2008，技术勘误 1，XSH/TC1-2008/0524 [37]。

### Issue 8
- 应用了 Austin Group 缺陷 368，添加了一个要求：如果无名信号量使用文件描述符实现，`sem_destroy()` 会关闭文件描述符。

---
