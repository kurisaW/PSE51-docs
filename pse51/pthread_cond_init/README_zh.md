# pthread_cond_destroy, pthread_cond_init — 销毁和初始化条件变量

## 概要

```c
#include <pthread.h>

int pthread_cond_destroy(pthread_cond_t *cond);

int pthread_cond_init(pthread_cond_t *restrict cond,
                     const pthread_condattr_t *restrict attr);

pthread_cond_t cond = PTHREAD_COND_INITIALIZER;
```

## 描述

`pthread_cond_destroy()` 函数应销毁由 `cond` 指定的给定条件变量；该对象实际上变为未初始化状态。实现可能导致 `pthread_cond_destroy()` 将 `cond` 引用的对象设置为无效值。已销毁的条件变量对象可以使用 `pthread_cond_init()` 重新初始化；在销毁后以其他方式引用该对象的结果是未定义的。

销毁一个没有线程当前被阻塞的已初始化条件变量是安全的。尝试销毁其他线程当前被阻塞的条件变量会导致未定义行为。

`pthread_cond_init()` 函数应使用 `attr` 引用的属性初始化由 `cond` 引用的条件变量。如果 `attr` 为 NULL，应使用默认的条件变量属性；效果与传递默认条件变量属性对象的地址相同。成功初始化后，条件变量的状态应变为已初始化。

有关进一步要求，请参阅 [2.9.9 同步对象副本和替代映射](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_09)。

尝试初始化已初始化的条件变量会导致未定义行为。

在默认条件变量属性合适的情况下，可以使用宏 `PTHREAD_COND_INITIALIZER` 初始化条件变量。效果应等同于通过调用 `pthread_cond_init()` 并将参数 `attr` 指定为 NULL 进行动态初始化，只是不执行错误检查。

如果 `pthread_cond_destroy()` 的 `cond` 参数指定的值不引用已初始化的条件变量，则行为是未定义的。

如果 `pthread_cond_init()` 的 `attr` 参数指定的值不引用已初始化的条件变量属性对象，则行为是未定义的。

## 返回值

如果成功，`pthread_cond_destroy()` 和 `pthread_cond_init()` 函数应返回零；否则应返回错误号以指示错误。

## 错误

如果出现以下情况，`pthread_cond_init()` 函数应失败：

- **[EAGAIN]**
  系统缺乏必要的资源（内存除外）来初始化另一个条件变量。

- **[ENOMEM]**
  内存不足，无法初始化条件变量。

这些函数不应返回 **[EINTR]** 错误代码。

## 示例

条件变量可以在所有被阻塞的线程被唤醒后立即销毁。例如，考虑以下代码：

```c
struct list {
    pthread_mutex_t lm;
    ...
};

struct elt {
    key k;
    int busy;
    pthread_cond_t notbusy;
    ...
};

/* 查找列表元素并保留它。 */
struct elt *
list_find(struct list *lp, key k)
{
    struct elt *ep;

    pthread_mutex_lock(&lp->lm);
    while ((ep = find_elt(l, k) != NULL) && ep->busy)
        pthread_cond_wait(&ep->notbusy, &lp->lm);
    if (ep != NULL)
        ep->busy = 1;
    pthread_mutex_unlock(&lp->lm);
    return(ep);
}

delete_elt(struct list *lp, struct elt *ep)
{
    pthread_mutex_lock(&lp->lm);
    assert(ep->busy);
    ... 从列表中移除 ep ...
    ep->busy = 0;  /* 谨慎处理。 */
(A) pthread_cond_broadcast(&ep->notbusy);
    pthread_mutex_unlock(&lp->lm);
(B) pthread_cond_destroy(&ep->notbusy);
    free(ep);
}
```

在此示例中，条件变量及其列表元素可以在所有等待它的线程被唤醒（A 行）后立即释放（B 行），因为互斥锁和代码确保没有其他线程可以触摸要删除的元素。

## 应用程序使用

无。

## 原理

如果实现检测到 `pthread_cond_destroy()` 的 `cond` 参数指定的值不引用已初始化的条件变量，建议函数应失败并报告 **[EINVAL]** 错误。

如果实现检测到 `pthread_cond_destroy()` 或 `pthread_cond_init()` 的 `cond` 参数指定的值引用正在被另一个线程使用的条件变量（例如，在 `pthread_cond_wait()` 调用中），或者检测到 `pthread_cond_init()` 的 `cond` 参数指定的值引用已初始化的条件变量，建议函数应失败并报告 **[EBUSY]** 错误。

如果实现检测到 `pthread_cond_init()` 的 `attr` 参数指定的值不引用已初始化的条件变量属性对象，建议函数应失败并报告 **[EINVAL]** 错误。

另请参阅 `pthread_mutex_destroy()`。

## 未来方向

无。

## 参见

- [`pthread_cond_broadcast()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_broadcast.html)
- [`pthread_cond_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_clockwait.html)
- [`pthread_mutex_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_destroy.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## 变更历史

### 首次发布于 Issue 5
为了与 POSIX 线程扩展对齐而包含。

### Issue 6
- `pthread_cond_destroy()` 和 `pthread_cond_init()` 函数被标记为线程选项的一部分。
- 应用了 IEEE PASC Interpretation 1003.1c #34，更新了描述。
- 为了与 ISO/IEC 9899:1999 标准对齐，在 `pthread_cond_init()` 原型中添加了 **restrict** 关键字。

### Issue 7
- `pthread_cond_destroy()` 和 `pthread_cond_init()` 函数从线程选项移动到基础。
- 删除了针对未初始化条件变量和未初始化条件变量属性对象的 **[EINVAL]** 错误；此条件导致未定义行为。
- 删除了针对已使用或已初始化条件变量的 **[EBUSY]** 错误；此条件导致未定义行为。
- 应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0455 [70]。
- 应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0269 [972] 和 XSH/TC2-2008/0270 [910]。