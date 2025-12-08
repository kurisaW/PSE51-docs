# pthread_setschedprio

## 概要

```c
#include <pthread.h>

int pthread_setschedprio(pthread_t thread, int prio);
```

## 描述

`pthread_setschedprio()` 函数应将由 `thread` 给出的线程ID的线程的调度优先级设置为 `prio` 给出的值。关于此函数调用如何影响线程在新优先级下的线程列表中排序的描述，请参见调度策略。

如果 `pthread_setschedprio()` 函数失败，目标线程的调度优先级不应被更改。

## 返回值

如果成功，`pthread_setschedprio()` 函数应返回零；否则，应返回错误号以指示错误。

## 错误

`pthread_setschedprio()` 函数可能在以下情况下失败：

- **[EINVAL]**
  `prio` 的值对于指定线程的调度策略无效。

- **[EPERM]**
  调用者没有适当的权限来设置指定线程的调度优先级。

`pthread_setschedprio()` 函数不应返回 [EINTR] 错误码。

## 示例

无。

## 应用程序用法

无。

## 原理

`pthread_setschedprio()` 函数为应用程序提供了一种暂时提高其优先级然后再次降低优先级的方法，而不会产生让位于相同优先级的其他线程的不良副作用。如果应用程序要实现自己的限制优先级反转的策略，如优先级继承或优先级上限，这是必需的。如果实现不支持线程优先级保护或线程优先级继承选项，这种能力尤其重要，但即使支持这些选项，如果应用程序要为其他资源（如信号量）限制优先级继承，也需要此功能。

标准开发者认为，虽然从概念上讲，通过修改 `pthread_setschedparam()` 的规范来解决这个问题可能更可取，但进行此类更改为时已晚，因为可能需要更改某些实现。因此，引入了这个新函数。

如果实现在线程生命期结束后检测到线程ID的使用，建议该函数应失败并报告 [ESRCH] 错误。

## 未来方向

无。

## 另请参见

- [调度策略](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_08_04_01)
- [`pthread_getschedparam()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_getschedparam.html)
- [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## 变更历史

首次在 Issue 6 中发布。作为对 IEEE PASC Interpretation 1003.1 #96 的响应而包含。

### Issue 7

`pthread_setschedprio()` 函数仅被标记为线程执行调度选项的一部分，因为线程选项现在是基础的一部分。

应用了 Austin Group Interpretation 1003.1-2001 #069，更新了 [EPERM] 错误。

应用了 Austin Group Interpretation 1003.1-2001 #142，移除了 [ESRCH] 错误条件。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0466 [314]。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0296 [757]。