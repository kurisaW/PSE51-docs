# pthread_getspecific, pthread_setspecific — 线程特定数据管理

## 概要

```c
#include <pthread.h>

void *pthread_getspecific(pthread_key_t key);
int pthread_setspecific(pthread_key_t key, const void *value);
```

## 描述

`pthread_getspecific()` 函数应返回当前绑定到指定 `key` 的值，该操作代表调用线程执行。

`pthread_setspecific()` 函数应将线程特定的 `value` 与通过先前调用 `pthread_key_create()` 获得的 `key` 关联。不同的线程可能将不同的值绑定到同一个键。这些值通常是指向动态分配内存块的指针，这些内存块已保留供调用线程使用。

使用不是从 `pthread_key_create()` 获得的 `key` 值调用 `pthread_getspecific()` 或 `pthread_setspecific()`，或在 `key` 已被 `pthread_key_delete()` 删除后调用这些函数，其效果是未定义的。

`pthread_getspecific()` 和 `pthread_setspecific()` 都可以从线程特定数据析构函数中调用。对正在销毁的线程特定数据键调用 `pthread_getspecific()` 应返回值 NULL，除非该值在析构函数启动后通过调用 `pthread_setspecific()` 被更改。从线程特定数据析构例程调用 `pthread_setspecific()` 可能导致存储丢失（在至少 PTHREAD_DESTRUCTOR_ITERATIONS 次析构尝试后）或无限循环。

两个函数都可以实现为宏。

## 返回值

`pthread_getspecific()` 函数应返回与给定 `key` 关联的线程特定数据值。如果没有线程特定数据值与 `key` 关联，则应返回值 NULL。

如果成功，`pthread_setspecific()` 函数应返回零；否则，应返回错误号以指示错误。

## 错误

`pthread_getspecific()` 不返回任何错误。

`pthread_setspecific()` 函数在以下情况下应失败：

- **[ENOMEM]**
  - 没有足够的内存来将非 NULL 值与键关联。

`pthread_setspecific()` 函数不应返回 [EINTR] 错误代码。

## 示例

无。

## 应用用法

无。

## 原理

`pthread_getspecific()` 的性能和易用性对于依赖在线程特定数据中维护状态的函数至关重要。由于它不需要检测任何错误，并且由于唯一可检测的错误是使用无效键，`pthread_getspecific()` 函数的设计优先考虑速度和简单性，而不是错误报告。

如果实现检测到 `pthread_setspecific()` 的 `key` 参数指定的值不是从 `pthread_key_create()` 获得的键值，或者引用了已被 `pthread_key_delete()` 删除的键，建议函数应失败并报告 [EINVAL] 错误。

## 未来方向

无。

## 另请参见

- `pthread_key_create()`
- `<pthread.h>`

## 变更历史

### 首次发布于 Issue 5
包含是为了与 POSIX 线程扩展保持一致。

### Issue 6
- `pthread_getspecific()` 和 `pthread_setspecific()` 函数被标记为线程选项的一部分。
- 应用了 IEEE PASC Interpretation 1003.1c #3 (Part 6)，更新了描述部分。
- 应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/96，更新了错误部分，使 [ENOMEM] 错误情况从"将值与键关联"更改为"将非 NULL 值与键关联"。

### Issue 7
- 应用了 Austin Group Interpretation 1003.1-2001 #063，更新了错误部分。
- `pthread_getspecific()` 和 `pthread_setspecific()` 函数从线程选项移动到基础部分。
- 删除了对于不是从 `pthread_key_create()` 获得或已被 `pthread_key_delete()` 删除的键值的 [EINVAL] 错误；这种情况会导致未定义行为。

---
