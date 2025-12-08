# sem_init — 初始化匿名信号量

## 概要

```c
#include <semaphore.h>

int sem_init(sem_t *sem, int pshared, unsigned value);
```

## 描述

`sem_init()` 函数应当初始化由 `sem` 引用的匿名信号量。初始化后的信号量值应当为 `value`。在成功调用 `sem_init()` 之后，该信号量可在后续的 `sem_clockwait()`、`sem_destroy()`、`sem_post()`、`sem_timedwait()`、`sem_trywait()` 和 `sem_wait()` 调用中使用。此信号量在被销毁前应保持可用状态。匿名信号量可使用文件描述符实现。

如果 `pshared` 参数具有非零值，则信号量在进程间共享；在这种情况下，任何可以访问信号量 `sem` 的进程都可以使用 `sem` 执行 `sem_clockwait()`、`sem_destroy()`、`sem_post()`、`sem_timedwait()`、`sem_trywait()` 和 `sem_wait()` 操作。

如果 `pshared` 参数为零，则信号量在同一进程的线程间共享；此进程中的任何线程都可以使用 `sem` 执行 `sem_clockwait()`、`sem_destroy()`、`sem_post()`、`sem_timedwait()`、`sem_trywait()` 和 `sem_wait()` 操作。

关于进一步要求，请参见 [2.9.9 同步对象副本和替代映射](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_09)。

尝试初始化已经初始化的信号量会导致未定义行为。

## 返回值

成功完成时，`sem_init()` 函数应当初始化 `sem` 中的信号量并返回 0。否则，它应当返回 -1 并设置 `errno` 来指示错误。

## 错误

`sem_init()` 函数在以下情况下应当失败：

- **[EINVAL]**
  `value` 参数超过 {SEM_VALUE_MAX}。

- **[ENOSPC]**
  初始化信号量所需的资源已耗尽，或者已达到信号量限制 ({SEM_NSEMS_MAX})。

- **[EPERM]**
  进程缺乏初始化信号量的适当权限。

`sem_init()` 函数在以下情况下可能会失败：

- **[EMFILE]**
  进程可用的所有文件描述符当前都已打开。

- **[ENFILE]**
  系统中当前打开的文件数量已达到最大允许数量。

## 示例

无。

## 应用程序使用

无。

## 基本原理

无。

## 未来方向

无。

## 另请参见

- [`sem_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_clockwait.html)
- [`sem_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_destroy.html)
- [`sem_post()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_post.html)
- [`sem_trywait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_trywait.html)
- [`<semaphore.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/semaphore.h.html)

## 更改历史

### 首次发布于 Issue 5
为与 POSIX 实时扩展对齐而包含。

### Issue 6
- `sem_init()` 函数被标记为信号量选项的一部分。
- 如果实现不支持信号量选项，则不需要提供存根，因此移除了 [ENOSYS] 错误条件。
- 为与 IEEE Std 1003.1d-1999 对齐，在另请参见部分添加了 `sem_timedwait()` 函数。
- 应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/116，更新描述以添加 `sem_timedwait()` 函数，以与 IEEE Std 1003.1d-1999 对齐。

### Issue 7
- 应用了 SD5-XSH-ERN-176。
- `sem_init()` 函数从信号量选项移动到基础规范。
- 应用了 POSIX.1-2008，技术勘误 1，XSH/TC1-2008/0526 [37]。
- 应用了 POSIX.1-2008，技术勘误 2，XSH/TC2-2008/0318 [972]。

### Issue 8
- 应用了 Austin Group 缺陷 368，添加了匿名信号量可使用文件描述符实现的声明，并添加了 [EMFILE] 和 [ENFILE] 错误。
- 应用了 Austin Group 缺陷 1216，添加了 `sem_clockwait()`。

---
