# pthread_cond_broadcast, pthread_cond_signal — 广播或信号通知条件

## SYNOPSIS

```c
#include <pthread.h>

int pthread_cond_broadcast(pthread_cond_t *cond);
int pthread_cond_signal(pthread_cond_t *cond);
```

## DESCRIPTION

这些函数应解除阻塞在条件变量上的线程。

`pthread_cond_broadcast()` 函数应作为单个原子操作，确定是否有线程阻塞在指定的条件变量 `cond` 上，并解除所有这些线程的阻塞。

`pthread_cond_signal()` 函数应作为单个原子操作，确定是否有线程阻塞在指定的条件变量 `cond` 上，并解除至少一个这些线程的阻塞。

如果有多个线程阻塞在条件变量上，调度策略应决定解除线程阻塞的顺序。当每个因 `pthread_cond_broadcast()` 或 `pthread_cond_signal()` 而解除阻塞的线程从其 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 调用返回时，该线程应拥有其调用 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 时关联的互斥锁。被解除阻塞的线程应根据调度策略（如果适用）竞争互斥锁，如同每个线程都调用了 `pthread_mutex_lock()`。

`pthread_cond_broadcast()` 或 `pthread_cond_signal()` 函数可由线程调用，无论该线程当前是否拥有调用 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 的线程在等待期间与条件变量关联的互斥锁；但是，如果需要可预测的调度行为，那么调用 `pthread_cond_broadcast()` 或 `pthread_cond_signal()` 的线程应锁定该互斥锁。

如果 `pthread_cond_broadcast()` 或 `pthread_cond_signal()` 确定没有线程阻塞在 `cond` 上，这些函数应不起作用。

如果传递给 `pthread_cond_broadcast()` 或 `pthread_cond_signal()` 的 `cond` 参数值不引用已初始化的条件变量，则行为未定义。

## RETURN VALUE

如果成功，`pthread_cond_broadcast()` 和 `pthread_cond_signal()` 函数应返回零；否则，应返回错误编号以指示错误。

## ERRORS

这些函数不应返回 `[EINTR]` 错误码。

---

## EXAMPLES

无。

## APPLICATION USAGE

当共享变量状态以多个线程可以继续执行其任务的方式发生改变时，使用 `pthread_cond_broadcast()` 函数。考虑单生产者/多消费者问题，其中生产者可以在消费者一次访问一个项目的列表上插入多个项目。通过调用 `pthread_cond_broadcast()` 函数，生产者会通知所有可能正在等待的消费者，从而在多处理器上获得更高的吞吐量。此外，`pthread_cond_broadcast()` 使得实现读写锁更容易。当写者释放其锁时，需要 `pthread_cond_broadcast()` 函数来唤醒所有等待的读者。最后，两阶段提交算法可以使用此广播函数通知所有客户端即将到来的事务提交。

在异步调用的信号处理程序中使用 `pthread_cond_signal()` 函数是不安全的。即使它是安全的，在布尔 `pthread_cond_wait()` 的测试之间仍存在无法有效消除的竞争条件。

因此，互斥锁和条件变量不适合通过在信号处理程序中运行的代码发信号来释放等待的线程。

## RATIONALE

如果实现检测到传递给 `pthread_cond_broadcast()` 或 `pthread_cond_signal()` 的 `cond` 参数值不引用已初始化的条件变量，建议函数应失败并报告 `[EINVAL]` 错误。

### 条件信号的多次唤醒

在多处理器上，`pthread_cond_signal()` 的实现可能无法避免解除阻塞多个阻塞在条件变量上的线程。例如，考虑以下 `pthread_cond_wait()` 和 `pthread_cond_signal()` 的部分实现，由两个线程按给定顺序执行。一个线程试图等待条件变量，另一个线程并发执行 `pthread_cond_signal()`，而第三个线程已经在等待。

```c
pthread_cond_wait(mutex, cond):
    value = cond->value;           /* 1 */
    pthread_mutex_unlock(mutex);   /* 2 */
    pthread_mutex_lock(cond->mutex); /* 10 */
    if (value == cond->value) {    /* 11 */
        me->next_cond = cond->waiter;
        cond->waiter = me;
        pthread_mutex_unlock(cond->mutex);
        unable_to_run(me);
    } else
        pthread_mutex_unlock(cond->mutex); /* 12 */
    pthread_mutex_lock(mutex);     /* 13 */

pthread_cond_signal(cond):
    pthread_mutex_lock(cond->mutex); /* 3 */
    cond->value++;                     /* 4 */
    if (cond->waiter) {                /* 5 */
        sleeper = cond->waiter;        /* 6 */
        cond->waiter = sleeper->next_cond; /* 7 */
        able_to_run(sleeper);          /* 8 */
    }
    pthread_mutex_unlock(cond->mutex); /* 9 */
```

结果是多个线程可能从其 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 调用中返回，这是由于一次 `pthread_cond_signal()` 调用造成的。这种效应称为"伪唤醒"。请注意，这种情况是自我修正的，因为被这样唤醒的线程数量是有限的；例如，在上述事件序列之后，下一个调用 `pthread_cond_wait()` 的线程会被阻塞。

虽然这个问题可以被解决，但对于仅偶尔发生的边缘条件损失的效率是不可接受的，特别是考虑到无论如何都必须检查与条件变量关联的谓词。纠正此问题会不必要地降低这个所有高级同步操作基本构建块的并发度。

允许伪唤醒的一个额外好处是应用程序被迫在条件等待周围编码谓词测试循环。这也使应用程序能够容忍应用程序其他部分可能编码的同一条件变量上的多余条件广播或信号。因此，产生的应用程序更加健壮。因此，POSIX.1-2024 明确记录了可能发生伪唤醒。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `pthread_cond_clockwait()`
- `pthread_cond_destroy()`
- XBD 4.15.2 内存同步
- `<pthread.h>`

## CHANGE HISTORY

### 首次发布于 Issue 5
包含用于与 POSIX 线程扩展对齐。

### Issue 6
`pthread_cond_broadcast()` 和 `pthread_cond_signal()` 函数被标记为线程选项的一部分。

添加了 APPLICATION USAGE 部分。

### Issue 7
`pthread_cond_broadcast()` 和 `pthread_cond_signal()` 函数从线程选项移动到基础。

删除了未初始化条件变量的 `[EINVAL]` 错误；此条件导致未定义行为。

### Issue 8
应用 Austin Group 缺陷 609，添加了原子性要求。

应用 Austin Group 缺陷 1216，添加了 `pthread_cond_clockwait()`。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*