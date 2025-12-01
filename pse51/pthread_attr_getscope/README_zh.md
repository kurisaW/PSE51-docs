# pthread_attr_getscope, pthread_attr_setscope

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## 名称

pthread_attr_getscope, pthread_attr_setscope — 获取和设置竞争范围属性 (**实时线程**)

## 概要

```c
#include <pthread.h>

int pthread_attr_getscope(const pthread_attr_t *restrict attr,
                         int *restrict contentionscope);

int pthread_attr_setscope(pthread_attr_t *attr, int contentionscope);
```

## 描述

`pthread_attr_getscope()` 和 `pthread_attr_setscope()` 函数分别用于获取和设置 *attr* 对象中的 *contentionscope* 属性。

*contentionscope* 属性可以取值为 `PTHREAD_SCOPE_SYSTEM`，表示系统调度竞争范围；或者 `PTHREAD_SCOPE_PROCESS`，表示进程调度竞争范围。符号 `PTHREAD_SCOPE_SYSTEM` 和 `PTHREAD_SCOPE_PROCESS` 在 `<pthread.h>` 头文件中定义。

如果传递给 `pthread_attr_getscope()` 或 `pthread_attr_setscope()` 的 *attr* 参数值不引用已初始化的线程属性对象，则行为未定义。

## 返回值

如果成功，`pthread_attr_getscope()` 和 `pthread_attr_setscope()` 函数应返回零；否则，应返回错误码以指示错误。

## 错误

`pthread_attr_setscope()` 函数在以下情况下应失败：

- **[ENOTSUP]**
  尝试将属性设置为不支持的值。

`pthread_attr_setscope()` 函数在以下情况下可能失败：

- **[EINVAL]**
  *contentionscope* 的值无效。

这些函数不应返回 **[EINTR]** 错误码。

---

*以下各节为提供参考信息。*

## 示例

无。

## 应用用法

设置这些属性后，可以使用 `pthread_create()` 创建具有指定属性的线程。使用这些例程不会影响当前正在运行的线程。

关于线程调度属性及其默认设置的更多详细信息，请参阅 [2.9.4 线程调度](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_04)。

## 基本原理

如果实现检测到传递给 `pthread_attr_getscope()` 或 `pthread_attr_setscope()` 的 *attr* 参数值不引用已初始化的线程属性对象，建议函数应失败并报告 **[EINVAL]** 错误。

## 未来方向

无。

## 另请参阅

- [`pthread_attr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_destroy.html)
- [`pthread_attr_getinheritsched()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getinheritsched.html)
- [`pthread_attr_getschedpolicy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getschedpolicy.html)
- [`pthread_attr_getschedparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getschedparam.html)
- [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html)

XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html), [`<sched.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sched.h.html)

## 变更历史

首次发布于 Issue 5。为了与 POSIX 线程扩展对齐而包含在内。

标记为实时线程特性组的一部分。

### Issue 6

`pthread_attr_getscope()` 和 `pthread_attr_setscope()` 函数被标记为线程和线程执行调度选项的一部分。

如果实现不支持线程执行调度选项，则不需要提供存根，因此删除了 **[ENOSYS]** 错误条件。

为了与 ISO/IEC 9899:1999 标准对齐，向 `pthread_attr_getscope()` 原型添加了 **`restrict`** 关键字。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/81，在应用用法部分添加了对 [2.9.4 线程调度](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_04) 的引用。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/82，更新错误部分以包含当 *attr* 引用未初始化线程属性对象时的可选错误。

### Issue 7

`pthread_attr_getscope()` 和 `pthread_attr_setscope()` 函数仅被标记为线程执行调度选项的一部分，因为线程选项现在是基础的一部分。

删除了未初始化线程属性对象的 **[EINVAL]** 错误；此条件导致未定义行为。

应用了 POSIX.1-2008，技术勘误 1，XSH/TC1-2008/0453 [314]。

应用了 POSIX.1-2008，技术勘误 2，XSH/TC2-2008/0264 [757]。

*参考信息结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group，保留所有权利