# pthread_getcpuclockid

## SYNOPSIS（概要）

```c
#include <pthread.h>
#include <time.h>

int pthread_getcpuclockid(pthread_t thread_id, clockid_t *clock_id);
```

## DESCRIPTION（描述）

如果由 `thread_id` 指定的线程存在，`pthread_getcpuclockid()` 函数应将该线程的 CPU 时间时钟的时钟 ID 返回到 `clock_id` 中。

## RETURN VALUE（返回值）

成功完成后，`pthread_getcpuclockid()` 应返回零；否则，应返回错误号以指示错误。

## ERRORS（错误）

未定义错误。

---

*以下章节为参考信息。*

## EXAMPLES（示例）

无。

## APPLICATION USAGE（应用程序用法）

`pthread_getcpuclockid()` 函数是线程 CPU 时间时钟选项的一部分，不需要在所有实现中提供。

## RATIONALE（基本原理）

如果实现在线程生命周期结束后检测到线程 ID 的使用，建议该函数应失败并报告 [ESRCH] 错误。

## FUTURE DIRECTIONS（未来方向）

无。

## SEE ALSO（参见）

- [`clock_getcpuclockid()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/clock_getcpuclockid.html)
- [`clock_getres()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/clock_getres.html)
- [`timer_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/timer_create.html)

XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html), [`<time.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/time.h.html)

## CHANGE HISTORY（变更历史）

首次发布于 Issue 6。源自 IEEE Std 1003.1d-1999。

在 SYNOPSIS 中，不再需要包含 [`<sys/types.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_types.h.html)。

### Issue 7

由于线程选项现在是基础的一部分，`pthread_getcpuclockid()` 函数仅标记为线程 CPU 时间时钟选项的一部分。

应用 Austin Group Interpretation 1003.1-2001 #142，移除了 [ESRCH] 错误条件。

应用 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0275 [757]。

