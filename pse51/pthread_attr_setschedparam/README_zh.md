# pthread_attr_getschedparam, pthread_attr_setschedparam — 获取和设置调度参数属性

## SYNOPSIS

```c
#include <pthread.h>

int pthread_attr_getschedparam(const pthread_attr_t *restrict attr,
                               struct sched_param *restrict param);

int pthread_attr_setschedparam(pthread_attr_t *restrict attr,
                               const struct sched_param *restrict param);
```

## DESCRIPTION

`pthread_attr_getschedparam()` 和 `pthread_attr_setschedparam()` 函数分别用于获取和设置 `attr` 参数中的调度参数属性。`param` 结构体的内容在 `<sched.h>` 头文件中定义。对于 SCHED_FIFO 和 SCHED_RR 策略，`param` 的唯一必需成员是 `sched_priority`。

[TSP] 对于 SCHED_SPORADIC 策略，`param` 结构体的必需成员是 `sched_priority`、`sched_ss_low_priority`、`sched_ss_repl_period`、`sched_ss_init_budget` 和 `sched_ss_max_repl`。指定的 `sched_ss_repl_period` 需要大于或等于指定的 `sched_ss_init_budget` 函数才能成功；如果不是，则函数将失败。`sched_ss_max_repl` 的值必须在包含范围 [1,{SS_REPL_MAX}] 内函数才能成功；如果不是，函数将失败。对于 `sched_ss_repl_period` 和 `sched_ss_init_budget` 值是按照此函数提供的方式存储还是四舍五入以与所用时钟的分辨率对齐，这是未指定的。

如果 `pthread_attr_getschedparam()` 或 `pthread_attr_setschedparam()` 的 `attr` 参数指定的值不引用已初始化的线程属性对象，则行为未定义。

## RETURN VALUE

如果成功，`pthread_attr_getschedparam()` 和 `pthread_attr_setschedparam()` 函数应返回零；否则，应返回错误编号以指示错误。

## ERRORS

`pthread_attr_setschedparam()` 函��应在以下情况失败：

- **ENOTSUP** - 尝试将属性设置为不支持的值。

`pthread_attr_setschedparam()` 函数可能在以下情况失败：

- **EINVAL** - `param` 的值无效。

这些函数不应返回 [EINTR] 错误代码。

## EXAMPLES

无。

## APPLICATION USAGE

设置这些属性后，可以使用 `pthread_create()` 创建具有指定属性的线程。使用这些例程不会影响当前运行的线程。

## RATIONALE

如果实现检测到 `pthread_attr_getschedparam()` 或 `pthread_attr_setschedparam()` 的 `attr` 参数指定的值不引用已初始化的线程属性对象，建议函数应失败并报告 [EINVAL] 错误。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `pthread_attr_destroy()`
- `pthread_attr_getscope()`
- `pthread_attr_getinheritsched()`
- `pthread_attr_getschedpolicy()`
- `pthread_create()`
- `<pthread.h>`
- `<sched.h>`

## CHANGE HISTORY

### 首次发布于 Issue 5
为与 POSIX 线程扩展对齐而包含。

### Issue 6
- `pthread_attr_getschedparam()` 和 `pthread_attr_setschedparam()` 函数被标记为 Threads 选项的一部分。
- 为与 IEEE Std 1003.1d-1999 对齐添加了 SCHED_SPORADIC 调度策略。
- 为与 ISO/IEC 9899:1999 标准对齐，向 `pthread_attr_getschedparam()` 和 `pthread_attr_setschedparam()` 原型添加了 `restrict` 关键字。
- 应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/78，更新了 ERRORS 部分以包含当 `attr` 引用未初始化线程属性对象时的可选错误。

### Issue 7
- `pthread_attr_getschedparam()` 和 `pthread_attr_setschedparam()` 函数从 Threads 选项移动到 Base。
- 应用了 Austin Group Interpretation 1003.1-2001 #119，阐明了 `sched_ss_repl_period` 和 `sched_ss_init_budget` 值的准确性要求。
- 删除了未初始化线程属性对象的 [EINVAL] 错误；此条件导致未定义行为。
- 应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0451 [314]。

---

*来源：The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*
*版权所有 © 2001-2024 The IEEE and The Open Group*