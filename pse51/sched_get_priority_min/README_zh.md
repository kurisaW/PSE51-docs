# sched_get_priority_max, sched_get_priority_min

## SYNOPSIS（概要）

```c
#include <sched.h>

int sched_get_priority_max(int policy);
int sched_get_priority_min(int policy);
```

## DESCRIPTION（描述）

`sched_get_priority_max()` 和 `sched_get_priority_min()` 函数应分别返回由 `policy` 指定的调度策略所对应的最大值或最小值。

`policy` 的值应为 `<sched.h>` 中定义的调度策略值之一。

## RETURN VALUE（返回值）

如果成功，`sched_get_priority_max()` 和 `sched_get_priority_min()` 函数应分别返回相应的最大值或最小值。如果不成功，它们应返回值 -1 并设置 `errno` 来指示错误。

## ERRORS（错误）

`sched_get_priority_max()` 和 `sched_get_priority_min()` 函数在以下情况下应失败：

- **EINVAL**
  - `policy` 参数的值不表示已定义的调度策略。

---

*以下部分为补充信息。*

## EXAMPLES（示例）

无。

## APPLICATION USAGE（应用程序使用）

无。

## RATIONALE（基本原理）

无。

## FUTURE DIRECTIONS（未来方向）

无。

## SEE ALSO（参见）

- [`sched_getparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_getparam.html)
- [`sched_setparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_setparam.html)
- [`sched_getscheduler()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_getscheduler.html)
- [`sched_rr_get_interval()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_rr_get_interval.html)
- [`sched_setscheduler()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sched_setscheduler.html)

XBD [`<sched.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sched.h.html)

## CHANGE HISTORY（变更历史）

### 首次发布于 Issue 5
为与 POSIX 实时扩展对齐而包含。

### Issue 6
这些函数被标记为进程调度选项的一部分。

`[ENOSYS]` 错误条件已被移除，因为如果实现不支持进程调度选项，不需要提供存根。

`[ESRCH]` 错误条件已被移除，因为这些函数不接受 `pid` 参数。

应用 IEEE Std 1003.1-2001/Cor 1-2002，项目 XSH/TC1/D6/52，将 SYNOPSIS 中的 PS 边距代码更改为 PS|TPS。

---
