# pthread_key_delete

## 名称

pthread_key_delete - 删除线程特定数据键

## 概要

```c
#include <pthread.h>

int pthread_key_delete(pthread_key_t key);
```

## 描述

`pthread_key_delete()` 函数应删除一个之前由 [`pthread_key_create()`](pthread_key_create.html) 返回的线程特定数据键。与 `key` 关联的线程特定数据值在调用 `pthread_key_delete()` 时不必为 NULL。释放任何应用程序存储或执行与已删除键或任何线程中关联的线程特定数据相关的数据结构的清理操作是应用程序的责任；此清理可以在调用 `pthread_key_delete()` 之前或之后进行。在调用 `pthread_key_delete()` 之后任何使用 `key` 的尝试都会导致未定义行为。

`pthread_key_delete()` 函数应可从析构函数内部调用。`pthread_key_delete()` 不会调用任何析构函数。任何可能与 `key` 关联的析构函数在线程退出时都不再被调用。

## 返回值

成功完成后，`pthread_key_delete()` 应返回零；否则，应返回错误编号以指示错误。

## 错误

`pthread_key_delete()` 函数可能在以下情况下失败：

- **EINVAL** - `key` 指定的值不是从 `pthread_key_create()` 获得的键值，或引用了已被 `pthread_key_delete()` 删除的键。

## 示例

*未提供示例。*

## 应用程序用法

包含线程特定数据键删除功能是为了允许释放与未使用的线程特定数据键关联的资源。未使用的线程特定数据键可能在多种情况下出现，例如当分配了键的动态加载模块被卸载时。

符合标准的应用程序负责执行与要删除的键关联的数据结构所需的任何清理操作，包括线程特定数据值引用的数据。`pthread_key_delete()` 不执行此类清理。特别地，不调用析构函数。这种职责划分有几个原因：

1. 用于在线程退出时释放线程特定数据的关联析构函数只有在分配线程特定数据的线程中调用时才能保证正确工作。（析构函数本身可能使用线程特定数据。）因此，它们不能用于在键删除时释放其他线程中的线程特定数据。试图在键删除时让其他线程调用它们需要异步中断其他线程。但由于被中断的线程可能处于任意状态，包括持有析构函数运行所需的锁，这种方法会失败。一般来说，没有安全的机制可以让实现在键删除时释放线程特定数据。

2. 即使有安全释放与要删除的键关联的线程特定数据的方法，这样做也需要实现能够枚举具有非 NULL 数据的线程，并可能阻止在键删除过程中创建更多线程特定数据。这种特殊情况会在正常情况下导致额外的同步，而这是不必要的。

应用程序要知道删除键是安全的，它必须知道所有可能曾经使用该键的线程都不会再次尝试使用它。例如，如果所有客户端线程都调用了清理过程，声明它们完成了正在关闭的模块的工作（可能通过将引用计数设置为零），它就可以知道这一点。

## 基本原理

如果实现检测到传递给 `pthread_key_delete()` 的 `key` 参数值不是从 `pthread_key_create()` 获得的键值，或引用了已被 `pthread_key_delete()` 删除的键，建议函数应失败并报告 `[EINVAL]` 错误。

## 未来方向

*无。*

## 另请参阅

[`pthread_key_create()`](pthread_key_create.html)

## XSI

*无。*

## 版权

本文部分内容是从 IEEE Std 1003.1-2017 for POSIX.1-2017, Standard for Information Technology -- Portable Operating System Interface (POSIX), The Open Group Base Specifications Issue 7, 2018 Edition 中以电子形式转载和复制的，版权归属于电气和电子工程师学会有限公司和 The Open Group (C) 2018。如果此版本与原始 IEEE 和 The Open Group 标准之间存在任何差异，应以原始 IEEE 和 The Open Group 标准为准。原始标准可在线获取：http://www.opengroup.org/unix/online.html 。

本页上出现的任何印刷或格式错误很可能是在将源文件转换为手册页格式时引入的。要报告此类错误，请参见 https://www.kernel.org/doc/man-pages/reporting_bugs.html 。