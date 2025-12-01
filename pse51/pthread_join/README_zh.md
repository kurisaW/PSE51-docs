# pthread_join — 等待线程终止

## 概要

```c
#include <pthread.h>

int pthread_join(pthread_t thread, void **value_ptr);
```

## 描述

`pthread_join()` 函数应当挂起调用线程的执行，直到目标线程终止，除非目标线程已经终止。当成功调用 `pthread_join()` 且 `value_ptr` 参数不为 NULL 时，终止线程传递给 `pthread_exit()` 的值应当在 `value_ptr` 引用的位置中可用。当 `pthread_join()` 成功返回时，目标线程已被终止。同时指定相同目标线程的多次 `pthread_join()` 调用的结果是未定义的。如果调用 `pthread_join()` 的线程被取消，则目标线程不应被分离。

僵尸线程是否计入 {PTHREAD_THREADS_MAX} 是未指定的。

如果传递给 `pthread_join()` 的 thread 参数值不指向可连接的线程，则行为是未定义的。

如果传递给 `pthread_join()` 的 thread 参数值指向调用线程，则行为是未定义的。

如果 thread 指向使用 `thrd_create()` 创建的线程，且该线程通过从其启动例程返回而终止，或已经终止，则 `pthread_join()` 的行为是未定义的。如果 thread 指向通过调用 `thrd_exit()` 终止的线程，或已经终止的线程，则 `pthread_join()` 的行为是未定义的。

## 返回值

如果成功，`pthread_join()` 函数应当返回零；否则，应当返回错误编号以指示错误。

## 错误

`pthread_join()` 函数可能在以下情况下失败：

- **[EDEADLK]** 检测到死锁。

`pthread_join()` 函数不应返回 [EINTR] 错误码。

## 示例

线程创建和删除的示例如下：

```c
typedef struct {
    int *ar;
    long n;
} subarray;

void *
incer(void *arg)
{
    long i;

    for (i = 0; i < ((subarray *)arg)->n; i++)
        ((subarray *)arg)->ar[i]++;
}

int main(void)
{
    int        ar[1000000];
    pthread_t  th1, th2;
    subarray   sb1, sb2;

    sb1.ar = &ar[0];
    sb1.n  = 500000;
    (void) pthread_create(&th1, NULL, incer, &sb1);

    sb2.ar = &ar[500000];
    sb2.n  = 500000;
    (void) pthread_create(&th2, NULL, incer, &sb2);

    (void) pthread_join(th1, NULL);
    (void) pthread_join(th2, NULL);
    return 0;
}
```

## 应用程序用法

无。

## 原理

`pthread_join()` 函数是一个便利功能，在多线程应用程序中已被证明是有用的。确实，如果没有提供此函数，程序员可以通过向 `start_routine()` 传递额外状态作为参数来模拟此函数。终止线程将设置一个标志来指示终止并广播作为该状态一部分的条件；连接线程将在该条件变量上等待。虽然这种技术允许线程等待更复杂的条件（例如，等待多个线程终止），但等待单个线程终止被认为是有广泛用途的。此外，包含 `pthread_join()` 函数绝不妨碍程序员编写此类复杂等待。因此，虽然不是原语，但在 POSIX.1-2024 中包含 `pthread_join()` 被认为是有价值的。

`pthread_join()` 函数提供了一个简单的机制，允许应用程序等待线程终止。线程终止后，应用程序可以选择清理线程使用的资源。例如，在 `pthread_join()` 返回后，可以回收任何应用程序提供的栈存储。

对于每个以 detachstate 属性设置为 PTHREAD_CREATE_JOINABLE 创建的线程，最终应调用 `pthread_join()` 或 `pthread_detach()` 函数，以便回收与线程关联的存储。

`pthread_join()` 和取消之间的交互是明确定义的，原因如下：

- `pthread_join()` 函数，与所有其他非异步取消安全函数一样，只能在延迟取消类型下调用。
- 在禁用取消状态下不能发生取消。

因此，只需要考虑默认的取消状态。如指定的那样，`pthread_join()` 调用要么被取消，要么成功，但不能两者都有。对于应用程序来说，区别是明显的，因为要么运行取消处理程序，要么 `pthread_join()` 返回。由于 `pthread_join()` 是在延迟取消状态下调用的，因此不存在竞争条件。

如果实现检测到传递给 `pthread_join()` 的 thread 参数值不指向可连接的线程，建议函数应失败并报告 [EINVAL] 错误。

如果实现检测到传递给 `pthread_join()` 的 thread 参数值指向调用线程，建议函数应失败并报告 [EDEADLK] 错误。

如果实现检测到在线程 ID 生命周期结束后使用该线程 ID，建议函数应失败并报告 [ESRCH] 错误。

`pthread_join()` 函数不能用于获取使用 `thrd_create()` 创建并通过从其启动例程返回而终止的线程的退出状态，也不能用于获取通过调用 `thrd_exit()` 终止的线程的退出状态，因为此类线程具有 **int** 退出状态，而不是 `pthread_join()` 通过其 value_ptr 参数返回的 **void \*** 类型。

## 未来方向

无。

## 另请参阅

- `pthread_create()`
- `thrd_create()`
- `thrd_exit()`
- `wait()`

* XBD 4.15.2 内存同步，`<pthread.h>`

## 更改历史

**首次发布于 Issue 5。** 为与 POSIX 线程扩展对齐而包含。

**Issue 6**

`pthread_join()` 函数被标记为线程选项的一部分。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/97，更新 ERRORS 部分，使 [EINVAL] 错误成为可选，并从中删除"实现已检测到"字样。

**Issue 7**

`pthread_join()` 函数从线程选项移动到基础。

应用 Austin Group 解释 1003.1-2001 #142，删除 [ESRCH] 错误条件。

删除非可连接线程的 [EINVAL] 错误；此条件导致未定义行为。

删除调用线程的 [EDEADLK] 错误；此条件导致未定义行为。

**Issue 8**

应用 Austin Group 缺陷 792，将"已退出但未连接的线程"更改为"僵尸线程"。

应用 Austin Group 缺陷 1302，更新页面以考虑 `<threads.h>` 接口的添加。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*版权所有 © 2001-2024 The IEEE and The Open Group*