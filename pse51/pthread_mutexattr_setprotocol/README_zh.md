# pthread_mutexattr_setprotocol, pthread_mutexattr_getprotocol

获取和设置互斥锁属性对象的协议属性。

## 函数概要

```c
#include <pthread.h>

int pthread_mutexattr_setprotocol(pthread_mutexattr_t *attr, int protocol);
int pthread_mutexattr_getprotocol(const pthread_mutexattr_t *restrict attr, int *restrict protocol);
```

## 参数

- `attr` - 指向先前由 `pthread_mutexattr_init()` 函数创建的互斥锁属性对象的指针
- `protocol` - 协议属性的值（对于设置函数）
- `protocol` - 指向存储协议属性的整数的指针（对于获取函数）

## 返回值

成功时，函数返回 0。失败时，返回错误编号以指示错误。

## 错误

- `ENOSYS` - 实现不支持���定的协议值
- `ENOTSUP` - 实现不支持指定的协议值
- `EINVAL` - `protocol` 的值无效

## 描述

`pthread_mutexattr_getprotocol()` 和 `pthread_mutexattr_setprotocol()` 函数分别获取和设置由 `attr` 指向的互斥锁属性对象的协议属性，该对象先前由 `pthread_mutexattr_init()` 函数创建。

协议属性定义了使用互斥锁时要遵循的协议。`protocol` 的值可以是以下之一：

- `PTHREAD_PRIO_INHERIT` - 优先级继承协议
- `PTHREAD_PRIO_NONE` - 无特殊协议（默认值）
- `PTHREAD_PRIO_PROTECT` - 优先级保护协议

这些值在 `<pthread.h>` 头文件中定义。属性的默认值应为 `PTHREAD_PRIO_NONE`。

### 协议行为详解

#### PTHREAD_PRIO_NONE
当线程拥有具有 `PTHREAD_PRIO_NONE` 协议属性的互斥锁时，其优先级和调度不应受到其互斥锁所有权的影响。

#### PTHREAD_PRIO_INHERIT
当线程由于拥有一个或多个使用 `PTHREAD_PRIO_INHERIT` 协议属性初始化的互斥锁而阻塞更高优先级的线程时，它应以其优先级或等待该线程拥有的、使用此协议初始化的任何互斥锁的最高优先级线程的优先级中较高的那个优先级执行。

当线程调用 `pthread_mutex_lock()` 时，互斥锁使用 `PTHREAD_PRIO_INHERIT` 协议属性值初始化，当调用线程因互斥锁被另一个线程拥有而被阻塞时，该拥有者线程应继承调用线程的优先级级别，只要它继续拥有该互斥锁。实现应将其执行优先级更新为其分配的优先级和所有继承优先级的最大值。此外，如果此拥有者线程本身在另一个具有 `PTHREAD_PRIO_INHERIT` 协议属性值的互斥锁上被阻塞，相同的优先级继承效果应以递归方式传播到该其他拥有者线程。

#### PTHREAD_PRIO_PROTECT
当线程拥有一个或多个使用 `PTHREAD_PRIO_PROTECT` 协议初始化的互斥锁时，它应以其优先级或该线程拥有的、使用此属性初始化的所有互斥锁的最高优先级上限中较高的那个优先级执行，无论是否有其他线程被这些互斥锁阻塞。

### 多协议场景
如果线程同时拥有几个使用不同协议初始化的互斥锁，它应以其通过这些协议各自获得的优先级中最高的优先级执行。

## 注意事项

如果 `pthread_mutexattr_getprotocol()` 或 `pthread_mutexattr_setprotocol()` 的 `attr` 参数指定的值不引用已初始化的互斥锁属性对象，则行为未定义。

## 一致性

这些函数属于 POSIX.1-2008 标准。

这些函数是线程选项以及线程优先级保护或线程优先级继承选项的一部分。

## 参见

- `pthread_mutexattr_init()`
- `pthread_mutex_lock()`
- `pthread_mutex_init()`
- `<pthread.h>`

## 版权

© IEEE 2003, 2023 - All rights reserved.

---
