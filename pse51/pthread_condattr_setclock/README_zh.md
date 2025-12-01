# pthread_condattr_getclock, pthread_condattr_setclock

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
版权所有 © 2001-2024 The IEEE and The Open Group

## 名称

pthread_condattr_getclock, pthread_condattr_setclock — 获取和设置条件变量的时钟选择属性

## 概要

```c
#include <pthread.h>

int pthread_condattr_getclock(const pthread_condattr_t *restrict attr,
                             clockid_t *restrict clock_id);

int pthread_condattr_setclock(pthread_condattr_t *attr,
                             clockid_t clock_id);
```

## 描述

`pthread_condattr_getclock()` 函数应当从 attr 引用的属性对象中获取 *clock* 属性的值。

`pthread_condattr_setclock()` 函数应���在 attr 引用的已初始化属性对象中设置 *clock* 属性。如果使用引用 CPU 时间时钟的 *clock_id* 参数调用 `pthread_condattr_setclock()`，则该调用应当失败。

*clock* 属性是用于测量 `pthread_cond_timedwait()` 超时服务的时钟的时钟 ID。*clock* 属性的默认值应当引用系统时钟。*clock* 属性对 `pthread_cond_clockwait()` 函数没有影响。

如果传递给 `pthread_condattr_getclock()` 或 `pthread_condattr_setclock()` 的 *attr* 参数值没有引用已初始化的条件变量属性对象，则行为是未定义的。

## 返回值

如果成功，`pthread_condattr_getclock()` 函数应当返回零，并将 attr 的时钟属性值存储到 *clock_id* 参数引用的对象中。否则，应当返回错误码以指示错误。

如果成功，`pthread_condattr_setclock()` 函数应当返回零；否则，应当返回错误码以指示错误。

## 错误

`pthread_condattr_setclock()` 函数可能失败，如果：

- **[EINVAL]**
  - *clock_id* 指定的值没有引用已知时钟，或者是 CPU 时间时钟。

这些函数不应返回 **[EINTR]** 错误码。

---

*以下章节是提供信息的。*

## 示例

无。

## 应用程序用法

无。

## 原理

如果实现检测到传递给 `pthread_condattr_getclock()` 或 `pthread_condattr_setclock()` 的 *attr* 参数值没有引用已初始化的条件变量属性对象，建议该函数应当失败并报告 **[EINVAL]** 错误。

## 未来方向

无。

## 另请参见

- [`pthread_cond_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_clockwait.html)
- [`pthread_cond_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_destroy.html)
- [`pthread_condattr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_condattr_destroy.html)
- [`pthread_condattr_getpshared()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_condattr_getpshared.html)
- [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html)
- [`pthread_mutex_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_destroy.html)

XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## 变更历史

首次发布于 Issue 6。派生自 IEEE Std 1003.1j-2000。

### Issue 7

`pthread_condattr_getclock()` 和 `pthread_condattr_setclock()` 函数从时钟选择选项移至基础部分。

删除了针对未初始化条件变量属性对象的 **[EINVAL]** 错误；此条件导致未定义行为。

### Issue 8

应用了 Austin Group 缺陷 1216，添加了 [`pthread_cond_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_clockwait.html)。

*信息文本结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
版权所有 © 2001-2024 The IEEE and The Open Group，保留所有权利