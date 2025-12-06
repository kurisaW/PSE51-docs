# sched_get_priority_max, sched_get_priority_min

## SYNOPSIS（概要）

```c
[PS|TPS] #include <sched.h>

int sched_get_priority_max(int policy);
int sched_get_priority_min(int policy);
```

## DESCRIPTION（描述）

`sched_get_priority_max()` 和 `sched_get_priority_min()` 函数应分别返回由 `policy` 指定的调度策略的相应最大值或最小值。

`policy` 的值应为 `<sched.h>` 中定义的调度策略值之一。

## RETURN VALUE（返回值）

如果成功，`sched_get_priority_max()` 和 `sched_get_priority_min()` 函数应分别返回相应的最大值或最小值。如果不成功，它们应返回值 -1 并设置 `errno` 以指示错误。

## ERRORS（错误）

如果出现以下情况，`sched_get_priority_max()` 和 `sched_get_priority_min()` 函数应失败：

- **EINVAL**
  - `policy` 参数的值不表示已定义的调度策略。

*以下部分为参考信息。*

## EXAMPLES（示例）

无。

## APPLICATION USAGE（应用程序使用）

无。

## RATIONALE（基本原理）

无。

## FUTURE DIRECTIONS（未来方向）

无。

## SEE ALSO（参见）

- [`sched_getparam()`](sched_getparam.html)
- [`sched_setparam()`](sched_setparam.html)
- [`sched_getscheduler()`](sched_getscheduler.html)
- [`sched_rr_get_interval()`](sched_rr_get_interval.html)
- [`sched_setscheduler()`](sched_setscheduler.html)

XBD `<sched.h>`

## CHANGE HISTORY（变更历史）

首次在 Issue 5 中发布。为与 POSIX 实时扩展保持一致而包含。

### Issue 6

这些函数被标记为进程调度选项的一部分。

如果实现不支持进程调度选项，则不需要提供存根，因此已删除 [ENOSYS] 错误条件。

由于这些函数不接受 `pid` 参数，因此已删除 [ESRCH] 错误条件。

应用 IEEE Std 1003.1-2001/Cor 1-2002，项目 XSH/TC1/D6/52，将 SYNOPSIS 中的 PS 边际代码更改为 PS|TPS。

