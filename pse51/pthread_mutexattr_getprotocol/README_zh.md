# pthread_mutexattr_getprotocol

## 概要

```c
#include <pthread.h>

int pthread_mutexattr_getprotocol(const pthread_mutexattr_t *restrict attr,
                                  int *restrict protocol);

int pthread_mutexattr_setprotocol(pthread_mutexattr_t *attr,
                                  int protocol);
```

## 描述

`pthread_mutexattr_getprotocol()` 和 `pthread_mutexattr_setprotocol()` 函数分别用于获取和设置由 `attr` 指向的互斥锁属性对象的协议属性，该对象先前由 `pthread_mutexattr_init()` 函数创建。

`protocol` 属性定义了在使用互斥锁时要遵循的协议。`protocol` 的值可以是以下之一：

- **PTHREAD_PRIO_INHERIT** （优先级继承）
- **PTHREAD_PRIO_NONE** （无优先级）
- **PTHREAD_PRIO_PROTECT** （优先级保护）

这些值在 `<pthread.h>` 头文件中定义。该属性的默认值应为 PTHREAD_PRIO_NONE。

### 协议行为

当一个线程拥有具有 PTHREAD_PRIO_NONE 协议属性的互斥锁时，其优先级和调度不应受到其互斥锁所有权的影响。

#### PTHREAD_PRIO_INHERIT

当一个线程因为拥有一个或多个具有 PTHREAD_PRIO_INHERIT 协议属性的互斥锁而阻塞较高优先级线程时，它应该在其优先级与等待该线程拥有的任何用此协议初始化的互斥锁的最高优先级线程的优先级中的较高者上执行。

当一个线程调用 `pthread_mutex_lock()` 时，互斥锁用具有值 PTHREAD_PRIO_INHERIT 的协议属性初始化，当调用线程因为互斥锁被另一个线程拥有而被阻塞时，该拥有线程应继承调用线程的优先级级别，只要它继续拥有该互斥锁。实现应将其执行优先级更新为其分配的优先级和所有其继承的优先级的最大值。此外，如果这个拥有线程本身在另一个具有值 PTHREAD_PRIO_INHERIT 的协议属性的互斥锁上被阻塞，同样的优先级继承效应应以递归方式传播到另一个拥有线程。

#### PTHREAD_PRIO_PROTECT

当一个线程拥有一个或多个用 PTHREAD_PRIO_PROTECT 协议初始化的互斥锁时，它应该在其优先级与该线程拥有的所有用此属性初始化的互斥锁的最高优先级上限中的较高者上执行，无论是否有其他线程被这些互斥锁中的任何一个阻塞。

### 多个互斥锁

如果一个线程同时拥有几个用不同协议初始化的互斥锁，它应该在这些协议各自会获得的优先级中的最高优先级上执行。

## 返回值

成功完成后，`pthread_mutexattr_getprotocol()` 和 `pthread_mutexattr_setprotocol()` 函数应返回零；否则，应返回错误编号以指示错误。

## 错误

`pthread_mutexattr_setprotocol()` 函数在以下情况下应失败：

- **ENOTSUP** - 由 `protocol` 指定的值是不支持的值。

`pthread_mutexattr_getprotocol()` 和 `pthread_mutexattr_setprotocol()` 函数在以下情况下可能失败：

- **EINVAL** - 由 `attr` 或 `protocol` 指定的值无效。

## 示例

规范中未提供示例。

## 应用程序使用

`pthread_mutexattr_getprotocol()` 和 `pthread_mutexattr_setprotocol()` 函数是线程选项以及线程优先级保护或线程优先级继承选项的一部分。

这些函数需要支持非健壮互斥锁优先级保护选项、非健壮互斥锁优先级继承选项、健壮互斥锁优先级保护选项或健壮互斥锁优先级继承选项中的至少一个。

## 基本原理

协议属性允许应用程序控制互斥锁的优先级行为，以防止优先级反转场景，其中较低优先级的线程持有较高优先级线程所需的互斥锁。

### 默认值

协议属性的默认值是 PTHREAD_PRIO_NONE，这意味着互斥锁所有权不影响线程优先级或调度。

### 优先级继承

优先级继承（PTHREAD_PRIO_INHERIT）在较高优先级线程等待互斥锁时临时提升持有互斥锁的线程的优先级。这有助于防止无界的优先级反转。

### 优先级保护

优先级保护（PTHREAD_PRIO_PROTECT）为每个互斥锁设置优先级上限。拥有此类互斥锁的线程在其自身优先级或其拥有的所有用此协议初始化的互斥锁的最高优先级上限中的较高者上执行。

## 未来方向

无。

## 另请参阅

- `pthread_mutexattr_init()`
- `pthread_mutex_lock()`
- `<pthread.h>`

## 版权

本文本的部分内容摘录并转载自 IEEE Std 1003.1-2017、The Open Group 基本规范第 7 版的电子形式，版权归电气和电子工程师协会（Institute of Electrical and Electronics Engineers, Inc）和 The Open Group 所有，版权所有 © 2017。如果本版本与原始 IEEE 和 The Open Group 标准之间存在任何差异，应以原始 IEEE 和 The Open Group 标准为准。原始标准可在线获取：http://www.opengroup.org/unix/online.html。

本页中出现的任何排版或格式错误很可能是在源文件转换为 man 页面格式过程中引入的。要报告此类错误，请参见 https://www.kernel.org/doc/man-pages/reporting_bugs.html。