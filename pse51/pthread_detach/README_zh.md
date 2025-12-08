# pthread_detach — 分离线程

## 概要

```c
#include <pthread.h>

int pthread_detach(pthread_t thread);
```

## 描述

`pthread_detach()` 函数应将线程 `thread` 从可结合状态（joinable）改变为分离状态（detached），向实现表明当线程终止时可以回收该线程的存储空间。如果 `thread` 尚未终止，`pthread_detach()` 不会导致其终止，但会防止该线程在终止时成为僵尸线程。

如果 `pthread_detach()` 的 `thread` 参数指定的值不引用可结合线程，则行为未定义。

## 返回值

如果调用成功，`pthread_detach()` 应返回 0；否则，应返回错误码以指示错误。

## 错误

`pthread_detach()` 函数不应返回 `[EINTR]` 错误码。

*以下章节为参考信息。*

## 示例

无。

## 应用程序用法

无。

## 基本原理

对于创建的每个线程，最终应调用 `pthread_join()` 或 `pthread_detach()` 函数，以便回收与线程相关的存储空间。

有人认为"分离"函数不是必需的；`detachstate` 线程创建属性就足够了，因为线程永远不需要动态分离。然而，至少在两种情况下会出现需求：

1. 在 `pthread_join()` 的取消处理程序中，几乎必须有一个 `pthread_detach()` 函数来分离 `pthread_join()` 正在等待的线程。没有它，处理程序将需要执行另一个 `pthread_join()` 来尝试分离线程，这既会无限期延迟取消处理，又会引入对 `pthread_join()` 的新调用，而这个新调用本身可能需要取消处理程序。在这种情况下，动态分离几乎是必需的。

2. 为了分离"初始线程"（在设置服务器线程的进程中可能需要这样做）。

如果实现检测到 `pthread_detach()` 的 `thread` 参数指定的值不引用可结合线程，建议函数应失败并报告 `[EINVAL]` 错误。

如果实现检测到在线程生命周期结束后使用线程 ID，建议函数应失败并报告 `[ESRCH]` 错误。

## 未来方向

无。

## 参见

- `pthread_join()`
- XBD `<pthread.h>`

## 变更历史

首次发布于 Issue 5。为与 POSIX 线程扩展对齐而包含。

### Issue 6

`pthread_detach()` 函数被标记为线程选项的一部分。

应用 IEEE Std 1003.1-2001/Cor 2-2004 项目 XSH/TC2/D6/95，更新错误部分，使 `[EINVAL]` 和 `[ESRCH]` 错误情况成为可选。

### Issue 7

`pthread_detach()` 函数从线程选项移至基础部分。

应用 Austin Group Interpretation 1003.1-2001 #142，移除 `[ESRCH]` 错误条件。

移除不可结合线程的 `[EINVAL]` 错误；此条件导致未定义行为。

### Issue 8

应用 Austin Group Defect 792，阐明分离活动线程可防止其在终止时成为僵尸线程。

应用 Austin Group Defect 1167，阐明在为线程调用 `pthread_detach()` 后，该线程不再可结合。

---
