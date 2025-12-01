# pthread_attr_destroy

## 名称

pthread_attr_destroy, pthread_attr_init - 销毁和初始化线程属性对象

## 概要

```c
#include <pthread.h>

int pthread_attr_destroy(pthread_attr_t *attr);
int pthread_attr_init(pthread_attr_t *attr);
```

## 描述

`pthread_attr_destroy()` 函数应当销毁一个线程属性对象。实现可能导致 `pthread_attr_destroy()` 将 `attr` 设置为实现定义的无效值。已销毁的 `attr` 属性对象可以使用 `pthread_attr_init()` 重新初始化；在对象被销毁后其他方式引用该对象的结果是未定义的。

`pthread_attr_init()` 函数应当使用给定实现使用的所有单个属性的默认值来初始化线程属性对象 `attr`。

当被 [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html) 使用时，生成的属性对象（可能通过设置单个属性值进行修改）定义了所创建线程的属性。单个属性对象可以在多个同时调用 [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html) 中使用。如果调用 `pthread_attr_init()` 时指定了已经初始化的 `attr` 属性对象，则结果是未定义的。

如果 `pthread_attr_destroy()` 的 `attr` 参数指定的值不引用已初始化的线程属性对象，则行为是未定义的。

## 返回值

成功完成后，这些函数应返回零；否则，应返回错误号以指示错误。

## 错误

这些函数可能在以下情况下失败：

- **EINVAL** - `attr` 指定的值不引用已初始化的线程属性对象（对于 `pthread_attr_destroy()`）。

- **EBUSY** - `attr` 指定的值引用已初始化的线程属性对象（对于 `pthread_attr_init()`）。

这些函数不应返回 `[EINTR]` 错误码。

## 示例

未提供示例。

## 应用程序使用

为线程、互斥锁和条件变量提供属性对象，作为支持这些领域未来可能标准化的一种机制，而不需要改变函数本身。

属性对象为线程的可配置方面提供了清晰的隔离。例如，"栈大小"是线程的一个重要属性，但不能以可移植的方式表示。在移植线程程序时，通常需要调整栈大小。使用属性对象可以通过允许将更改隔离在单个位置来帮助，而不是分散在每次线程创建的实例中。

属性对象可用于设置具有相似属性的线程"类"；例如，"具有大栈和高优先级的线程"或"具有最小栈的线程"。这些类可以在单个位置定义，然后在需要创建线程的任何地方引用。对"类"决策的更改变得直接明了，不需要对每个 [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html) 调用进行详细分析。

属性对象被定义为不透明类型，以帮助扩展性。如果这些对象被指定为结构，添加新属性将强制在扩展属性对象时重新编译所有多线程程序；如果不同程序组件由不同供应商提供，这可能是不可能的。

此外，不透明属性对象提供了提高性能的机会。参数有效性可以在设置属性时检查一次，而不是每次创建线程时检查。实现通常需要缓存创建成本高昂的内核对象。不透明属性对象提供了检测缓存对象因属性更改而变为无效的高效机制。

由于赋值在给定不透明类型上不一定定义，实现定义的默认值无法以可移植方式定义。这个问题的解决方案是允许属性对象由属性对象初始化函数动态初始化，以便默认值可以由实现自动提供。

## 基本原理

提供了以下建议作为所提供属性的建议替代方案：

1. 保持将通过按位包含或标志形成的参数传递给初始化例程（[`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html)、[`pthread_mutex_init()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_init.html)、[`pthread_cond_init()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_init.html)）的样式。包含标志的参数应该是不透明类型以便扩展。如果参数中没有设置标志，则对象以默认特征创建。实现可以指定实现定义的标志值和相关行为。

2. 如果需要对互斥锁和条件变量进行进一步专门化，实现可以指定对 `pthread_mutex_t` 和 `pthread_cond_t` 对象（而不是对属性对象）进行操作的附加过程。

这种解决方案的困难是：

1. 如果必须使用显式编码的按位包含或操作将位设置到位向量属性对象中，则位掩码不是不透明的。如果选项集超过 `int`，应用程序程序员需要知道每个位的位置。如果位通过封装（即获取和设置函数）设置��读取，那么位掩码只是当前定义的属性对象的实现，不应暴露给程序员。

2. 许多属性不是布尔值或非常小的整数值。例如，调度策略可以放在3位或4位中，但优先级需要5位或更多，从而在具有16位整数的机器上占用可能的16位中至少8位。因此，位掩码只能合理地控制是否设置了特定属性，它不能作为值本身的存储库。值需要指定为函数参数（不可扩展）、或通过设置结构字段（非不透明）、或通过获取和设置函数（使位掩码成为属性对象的冗余添加）。

栈大小被定义为可选属性，因为栈的概念本质上是机器相关的。例如，一些实现可能无法更改栈的大小，而其他实现可能不需要，因为栈页可能是不连续的，可以根据需要分配和释放。

属性机制在设计上很大程度上考虑了扩展性。对属性机制或对POSIX.1-2024卷中定义的任何属性对象的未来扩展必须谨慎进行，以免影响二进制兼容性。

属性对象，即使通过 [`malloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/malloc.html) 等动态分配函数分配，也可能在编译时固定其大小。这意味着，例如，在对 `pthread_attr_t` 有扩展的实现中，[`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html) 不能查看二进制应用程序假设有效的区域之外。这建议实现应该在属性对象中维护大小字段，以及可能的版本信息，如果要容纳不同方向（可能由不同供应商）的扩展。

如果实现检测到 `pthread_attr_destroy()` 的 `attr` 参数指定的值不引用已初始化的线程属性对象，建议函数应该失败并报告 `[EINVAL]` 错误。

如果实现检测到 `pthread_attr_init()` 的 `attr` 参数指定的值引用已初始化的线程属性对象，建议函数应该失败并报告 `[EBUSY]` 错误。

## 未来方向

无。

## 另请参阅

[`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html)、[`malloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/malloc.html)

POSIX.1-2024的Shell和实用程序卷，关于XSH系统接口选项的第2.9.2节

## 版权

本文的部分内容根据IEEE Std 1003.1-2024，信息技术标准——便携式操作系统接口（POSIX），The Open Group基础规范问题7，重新打印并电子复制，版权所有（C）2024，由电气和电子工程师协会Inc和The Open Group拥有。如果此版本与原始IEEE和The Open Group标准之间存在任何差异，应以原始IEEE和The Open Group标准为准。原始标准可以在线获取：https://pubs.opengroup.org/onlinepubs/9799919799/。

*信息性文本结束。*