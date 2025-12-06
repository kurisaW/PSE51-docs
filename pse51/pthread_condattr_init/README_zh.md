# pthread_condattr_destroy, pthread_condattr_init

## 概要

```c
#include <pthread.h>

int pthread_condattr_destroy(pthread_condattr_t *attr);
int pthread_condattr_init(pthread_condattr_t *attr);
```

## 描述

`pthread_condattr_destroy()` 函数应销毁一个条件变量属性对象；该对象实际上变为未初始化状态。实现可以导致 `pthread_condattr_destroy()` 将 `attr` 引用的对象设置为无效值。已销毁的 `attr` 属性对象可以使用 `pthread_condattr_init()` 重新初始化；在对象被销毁后以其他方式引用该对象的结果是未定义的。

`pthread_condattr_init()` 函数应使用实现定义的所有属性的默认值初始化条件变量属性对象 `attr`。

如果在调用 `pthread_condattr_init()` 时指定了一个已经初始化的 `attr` 属性对象，则结果未定义。

在条件变量属性对象已用于初始化一个或多个条件变量后，任何影响该属性对象的函数（包括销毁）都不应影响任何先前已初始化的条件变量。

POSIX.1-2024 本卷要求两个属性：**时钟** 属性和 **进程共享** 属性。

附加属性、它们的默认值以及用于获取和设置这些属性值的关联函数的名称是由实现定义的。

如果传递给 `pthread_condattr_destroy()` 的 `attr` 参数的值不引用已初始化的条件变量属性对象，则行为未定义。

## 返回值

如果成功，`pthread_condattr_destroy()` 和 `pthread_condattr_init()` 函数应返回零；否则，应返回错误码以指示错误。

## 错误

`pthread_condattr_init()` 函数可能在以下情况下失败：

- **[ENOMEM]**
  - 内存不足，无法初始化条件变量属性对象。

这些函数不应返回 **[EINTR]** 错误码。

---

*以下章节为信息性内容。*

## 示例

无。

## 应用用法

无。

## 原理

为条件变量定义 **进程共享** 属性的原因与为互斥锁定义该属性的原因相同。

如果实现检测到传递给 `pthread_condattr_destroy()` 的 `attr` 参数的值不引用已初始化的条件变量属性对象，建议函数应失败并报告 **[EINVAL]** 错误。

另见 [`pthread_attr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_destroy.html) 和 [`pthread_mutex_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_destroy.html)。

## 未来方向

无。

## 参见

[`pthread_attr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_destroy.html), [`pthread_cond_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_destroy.html), [`pthread_condattr_getpshared()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_condattr_getpshared.html), [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html), [`pthread_mutex_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_destroy.html)

XBD **[\<pthread.h\>]**](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## 变更历史

首次发布于 Issue 5。为与 POSIX 线程扩展对齐而包含。

### Issue 6

`pthread_condattr_destroy()` 和 `pthread_condattr_init()` 函数被标记为线程选项的一部分。

### Issue 7

`pthread_condattr_destroy()` 和 `pthread_condattr_init()` 函数从线程选项移动到基础规范。

对于未初始化的条件变量属性对象的 **[EINVAL]** 错误被移除；此条件导致未定义行为。

*信息性文本结束。*

---
