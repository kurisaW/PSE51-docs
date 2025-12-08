# mlock, munlock — 锁定或解锁进程地址空间范围 (REALTIME)

## 概要

```c
#include <sys/mman.h>

int mlock(const void *addr, size_t len);
int munlock(const void *addr, size_t len);
```

## 描述

`mlock()` 函数应使包含进程地址空间任何部分的整个页面（从地址 `addr` 开始，持续 `len` 字节）常驻内存，直到被解锁、进程退出或 `exec` 另一个进程映像为止。实现可能要求 `addr` 是 {PAGESIZE} 的倍数。

`munlock()` 函数应解锁包含进程地址空间任何部分的整个页面（从地址 `addr` 开始，持续 `len` 字节），无论进程对指定范围内的任何页面调用了多少次 `mlock()`。实现可能要求 `addr` 是 {PAGESIZE} 的倍数。

如果指定给 `munlock()` 调用范围内的任何页面也被映射到其他进程的地址空间中，那么另一个进程在这些页面上建立的任何锁定都不会受到此进程调用 `munlock()` 的影响。如果指定给 `munlock()` 调用的范围内的任何页面也被映射到调用进程地址空间中指定范围之外的其他部分，那么通过其他映射在这些页面上建立的任何锁定也不会受到此次调用的影响。

成功从 `mlock()` 返回时，指定范围内的页面应被锁定并常驻内存。成功从 `munlock()` 返回时，指定范围内的页面应相对于进程的地址空间被解锁。已解锁页面的内存常驻性未指定。

使用 `mlock()` 锁定进程内存需要适当的权限。

## 返回值

成功完成时，`mlock()` 和 `munlock()` 函数应返回零值。否则，进程地址空间中的任何锁定都不会被更改，函数应返回 -1 值并设置 `errno` 来指示错误。

## 错误

`mlock()` 和 `munlock()` 函数在以下情况下应失败：

- **[ENOMEM]**
  `addr` 和 `len` 参数指定的地址范围的某些或全部不对应于进程地址空间中的有效映射页面。

`mlock()` 函数在以下情况下应失败：

- **[EAGAIN]**
  进行调用时，操作所标识的某些或全部内存无法被锁定。

`mlock()` 和 `munlock()` 函数在以下情况下可能失败：

- **[EINVAL]**
  `addr` 参数不是 {PAGESIZE} 的倍数。

`mlock()` 函数在以下情况下可能失败：

- **[ENOMEM]**
  锁定指定范围映射的页面将超过进程可锁定内存量的实现定义限制。

- **[EPERM]**
  调用进程没有执行所请求操作的适当权限。

## 示例

无。

## 应用程序用法

无。

## 基本原理

无。

## 未来方向

无。

## 参见

- [`exec`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html)
- [`_exit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exit.html)
- [`fork()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fork.html)
- [`mlockall()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/mlockall.html)
- [`munmap()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/munmap.html)
- [`<sys/mman.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_mman.h.html)

## 变更历史

首次发布于 Issue 5。为与 POSIX 实时扩展保持一致而包含。

### Issue 6

`mlock()` 和 `munlock()` 函数被标记为范围内存锁定选项的一部分。

[ENOSYS] 错误条件已被移除，因为如果实现不支持范围内存锁定选项，则无需提供存根。

---
