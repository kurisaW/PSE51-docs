# sched_rr_get_interval — 获取执行时间限制 (REALTIME)

## 概要

```c
#include <sched.h>

int sched_rr_get_interval(pid_t pid, struct timespec *interval);
```

## 描述

`sched_rr_get_interval()` 函数应更新由 `interval` 参数引用的 `timespec` 结构体，使其包含由 `pid` 指定的进程的当前执行时间限制（即时间量程）。如果 `pid` 为零，则应返回调用进程的当前执行时间限制。

## 返回值

如果成功，`sched_rr_get_interval()` 函数应返回零。否则，它应返回值 -1 并设置 `errno` 来指示错误。

## 错误

`sched_rr_get_interval()` 函数在以下情况下应失败：

- **[ESRCH]** - 找不到与 `pid` 指定的进程相对应的进程。

## 示例

无。

## 应用程序用法

无。

## 基本原理

无。

## 未来方向

无。

## 另请参见

- [`sched_getparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_getparam.html)
- [`sched_get_priority_max()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_get_priority_max.html)
- [`sched_getscheduler()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_getscheduler.html)
- [`sched_setparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_setparam.html)
- [`sched_setscheduler()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_setscheduler.html)

XBD [`<sched.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sched.h.html)

## 变更历史

### 在 Issue 5 中首次发布。为与 POSIX 实时扩展保持一致而包含在内。

### Issue 6

`sched_rr_get_interval()` 函数被标记为进程调度选项的一部分。

`[ENOSYS]` 错误条件已被移除，因为如果实现不支持进程调度选项，则无需提供存根。

应用 IEEE Std 1003.1-2001/Cor 1-2002, XSH/TC1/D6/53，将概要中的 PS 边际代码更改为 PS|TPS。

---
