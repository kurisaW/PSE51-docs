# pthread_mutex_destroy

## 概要

```c
#include <pthread.h>

int pthread_mutex_destroy(pthread_mutex_t *mutex);
int pthread_mutex_init(pthread_mutex_t *restrict mutex,
                      const pthread_mutexattr_t *restrict attr);

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
```

## 描述

`pthread_mutex_destroy()` 函数应销毁由 `mutex` 引用的互斥锁对象；互斥锁对象实际上变为未初始化状态。实现可能使 `pthread_mutex_destroy()` 将由 `mutex` 引用的对象设置为无效值。

已销毁的互斥锁对象可以使用 `pthread_mutex_init()` 重新初始化；在销毁后以其他方式引用该对象的结果是未定义的。

销毁一个已初始化且未锁定的互斥锁是安全的。尝试销毁一个已锁定的互斥锁，或者另一个线程正尝试锁定的互斥锁，或者另一个线程正在 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 调用中使用的互斥锁，会导致未定义行为。

`pthread_mutex_init()` 函数应使用由 `attr` 指定的属性初始化由 `mutex` 引用的互斥锁。如果 `attr` 为 NULL，则使用默认的互斥锁属性；效果应与传递默认��斥锁属性对象的地址相同。成功初始化后，互斥锁的状态变为已初始化且未锁定。

 further requirements。

尝试初始化一个已经初始化的互斥锁会导致未定义行为。

在默认互斥锁属性合适的情况下，可以使用宏 `PTHREAD_MUTEX_INITIALIZER` 来初始化互斥锁。效果应等同于通过调用参数 `attr` 指定为 NULL 的 `pthread_mutex_init()` 进行动态初始化，但不执行错误检查。

如果传递给 `pthread_mutex_destroy()` 的 `mutex` 参数值不引用已初始化的互斥锁，则行为未定义。

如果传递给 `pthread_mutex_init()` 的 `attr` 参数值不引用已初始化的互斥锁属性对象，则行为未定义。

## 返回值

如果成功，`pthread_mutex_destroy()` 和 `pthread_mutex_init()` 函数应返回零；否则，应返回错误号以指示错误。

## 错误

`pthread_mutex_destroy()` 函数可能在以下情况失败：

- **EBUSY** - 实现检测到尝试销毁由 `mutex` 引用的对象，而该对象已被锁定或被引用（例如，正在被另一个线程在 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 调用中使用）。
- **EINVAL** - 由 `mutex` 指定的值不引用已初始化的互斥锁对象。

`pthread_mutex_init()` 函数可能在以下情况失败：

- **EBUSY** - 实现检测到尝试重新初始化由 `mutex` 引用的对象，该对象是之前已初始化但尚未销毁的互斥锁。
- **EINVAL** - 由 `attr` 指定的值不引用已初始化的互斥锁属性对象，或者由 `mutex` 指定的值不引用已初始化的互斥锁。
- **ENOMEM** - 内存不足，无法初始化互斥锁。

这些函数不应返回错误代码 [EINTR]。

## 示例

### 静态初始化

```c
static pthread_mutex_t foo_mutex = PTHREAD_MUTEX_INITIALIZER;

void foo()
{
    pthread_mutex_lock(&foo_mutex);
    /* 执行工作。 */
    pthread_mutex_unlock(&foo_mutex);
}
```

### 动态初始化

```c
static pthread_once_t foo_once = PTHREAD_ONCE_INIT;
static pthread_mutex_t foo_mutex;

void foo_init()
{
    pthread_mutex_init(&foo_mutex, NULL);
}

void foo()
{
    pthread_once(&foo_once, foo_init);
    pthread_mutex_lock(&foo_mutex);
    /* 执行工作。 */
    pthread_mutex_unlock(&foo_mutex);
}
```

## 应用程序用法

如果传递给 `pthread_mutex_destroy()` 的 `mutex` 参数值不引用已初始化的互斥锁，则行为未定义。

如果传递给 `pthread_mutex_init()` 的 `attr` 参数值不引用已初始化的互斥锁属性对象，则行为未定义。

## 基本原理

### 可能的替代实现

POSIX.1-2024 支持几种互斥锁的替代实现。实现可以直接将锁存储在 `pthread_mutex_t` 类型的对象中。或者，实现可以在堆中存储锁，而仅在互斥锁对象中存储指针、句柄或唯一 ID。每种实现都有其优势，或者在特定的硬件配置下可能是必需的。为了编写不受此选择影响的可移植代码，POSIX.1-2024 不为此类型定义赋值或相等性，并使用术语"初始化"来加强（更具限制性的）概念，即锁实际上可能驻留在互斥锁对象本身中。

请注意，这避免了对互斥锁或条件变量类型的过度规范，并促使该类型的不透明性。

允许但不要求实现使 `pthread_mutex_destroy()` 将非法值存储到互斥锁中。这可能有助于检测尝试锁定（或以其他方式引用）已销毁互斥锁的错误程序。

### 支持错误检查和性能之间的权衡

许多可能发生的错误情况不需要被实现检测到，以便让实现根据其特定应用程序和执行环境的需求在性能和错误检查程度之间进行权衡。作为一般规则，要求检测由系统引起的条件（如内存不足），但由错误编码的应用程序引起的条件（如未能提供足够的同步以防止互斥锁在使用时被删除）被指定为导致未定义行为。

### 互斥锁和条件变量的静态初始化符

为静态分配的同步对象提供静态初始化允许具有私有静态同步变量的模块避免运行时初始化测试和开销。此外，它简化了自初始化模块的编码。此类模块在 C 库中很常见，其中出于各种原因，设计要求自初始化而不是要求调用显式的模块初始化函数。

### 销毁互斥锁

互斥锁可以在解锁后立即销毁。但是，由于尝试销毁已锁定的互斥锁，或者另一个线程正尝试锁定的互斥锁，或者另一个线程正在 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 调用中使用的互斥锁，会导致未定义行为，因此必须注意确保没有其他线程可能引用该互斥锁。

### 健壮互斥锁

要求实现为进程共享属性设置为 `PTHREAD_PROCESS_SHARED` 的互斥锁提供健壮互斥锁。当进程共享属性设置为 `PTHREAD_PROCESS_PRIVATE` 时，允许但不要求实现提供健壮互斥锁。

## 另请参见

- [`pthread_cond_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_clockwait.html)
- [`pthread_cond_timedwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_timedwait.html)
- [`pthread_cond_wait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_wait.html)
- [`pthread_mutex_lock()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_lock.html)
- [`pthread_mutex_unlock()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_unlock.html)
- [`pthread_once()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_once.html)
- [基础定义，`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## 更改历史

源自 POSIX 线程扩展（1003.1c-1995）。

*信息性文本结束。*