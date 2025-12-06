# pthread_condattr_getclock, pthread_condattr_setclock

## SYNOPSIS (概要)

```c
#include <pthread.h>

int pthread_condattr_getclock(const pthread_condattr_t *restrict attr,
                             clockid_t *restrict clock_id);

int pthread_condattr_setclock(pthread_condattr_t *attr,
                             clockid_t clock_id);
```

## DESCRIPTION (描述)

`pthread_condattr_getclock()` 函数应从 *attr* 引用的属性对象中获取 *clock* 属性的值。

`pthread_condattr_setclock()` 函数应在 *attr* 引用的已初始化属性对象中设置 *clock* 属性。如果使用引用 CPU 时间时钟的 *clock_id* 参数调用 `pthread_condattr_setclock()`，该调用应失败。

*clock* 属性是用于测量 `pthread_cond_timedwait()` 超时服务的时钟的时钟 ID。*clock* 属性的默认值应引用系统时钟。*clock* 属性对 `pthread_cond_clockwait()` 函数没有影响。

如果 `pthread_condattr_getclock()` 或 `pthread_condattr_setclock()` 的 *attr* 参数指定的值不引用已初始化的条件变量属性对象，则行为未定义。

## RETURN VALUE (返回值)

如果成功，`pthread_condattr_getclock()` 函数应返回零，并将 *attr* 的时钟属性值存储到 *clock_id* 参数引用的对象中。否则，应返回错误号以指示错误。

如果成功，`pthread_condattr_setclock()` 函数应返回零；否则，应返回错误号以指示错误。

## ERRORS (错误)

`pthread_condattr_setclock()` 函数可能失败的情况：

- **[EINVAL]**
  - *clock_id* 指定的值不引用已知时钟，或者是 CPU 时间时钟。

这些函数不应返回 [EINTR] 错误码。

---

*以下章节为补充信息。*

## EXAMPLES (示例)

无。

## APPLICATION USAGE (应用程序用法)

无。

## RATIONALE (基本原理)

如果实现检测到 `pthread_condattr_getclock()` 或 `pthread_condattr_setclock()` 的 *attr* 参数指定的值不引用已初始化的条件变量属性对象，建议函数应失败并报告 [EINVAL] 错误。

## FUTURE DIRECTIONS (未来方向)

无。

## SEE ALSO (参见)

- `pthread_cond_clockwait()`
- `pthread_cond_destroy()`
- `pthread_condattr_destroy()`
- `pthread_condattr_getpshared()`
- `pthread_create()`
- `pthread_mutex_destroy()`
- XBD `<pthread.h>`

## CHANGE HISTORY (变更历史)

首次发布于 Issue 6。源自 IEEE Std 1003.1j-2000。

### Issue 7

`pthread_condattr_getclock()` 和 `pthread_condattr_setclock()` 函数从时钟选择选项移至基础规范。

移除了针对未初始化条件变量属性对象的 [EINVAL] 错误；此条件会导致未定义行为。

### Issue 8

应用 Austin Group 缺陷 1216，添加了 `pthread_cond_clockwait()`。

---
