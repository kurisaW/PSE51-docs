# calloc — 内存分配器

## 概要

```c
#include <stdlib.h>

void *calloc(size_t nelem, size_t elsize);
```

## 描述

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`calloc()` 函数应为数组分配未使用的空间，该数组包含 `nelem` 个元素，每个元素的字节大小为 `elsize`。分配的空间应初始化为所有位均为 0。

通过连续调用 `calloc()` 分配的存储空间的顺序和连续性是未指定的。如果分配成功，返回的指针应适当对齐，以便可以将其赋值给具有基本对齐要求的任何类型对象的指针，然后用于访问在分配空间中的此类对象或此类对象的数组（直到空间被显式释放或重新分配）。每次这样的分配都应产生一个指向与其他任何对象不相交的对象的指针。返回的指针应指向分配空间的起始位置（最低字节地址）。如果无法分配空间，应返回空指针。如果请求的空间大小为 0，其行为是实现定义的：要么返回空指针，要么行为如同大小为某个非零值，但如果使用返回的指针访问对象，则行为未定义。

为确定数据竞争的存在性，`calloc()` 的行为应如同仅访问通过其参数可访问的内存位置，而不访问其他静态持续时间存储。但是，该函数可以可见地修改其分配的存储。对 `aligned_alloc()`、`calloc()`、`free()`、`malloc()`、[ADV] `posix_memalign()`、[CX] `reallocarray()` 和 `realloc()` 的调用，如果这些调用分配或释放特定的内存区域，则应在单一总顺序中发生（参见 4.15.1 内存排序），并且每个这样的释放调用应与该顺序中的下一次分配（如果有）同步。

## 返回值

成功完成时，`calloc()` 应返回指向分配空间的指针；如果 `nelem` 或 `elsize` 为 0，应用程序应确保不使用该指针访问对象。

否则，它应返回空指针 [CX] 并设置 `errno` 以指示错误。

## 错误

`calloc()` 函数在以下情况下可能失败：

- **[ENOMEM]** [CX] 可用内存不足，包括 `nelem` * `elsize` 会溢出的情况。

`calloc()` 函数在以下情况下可能失败：

- **[EINVAL]** [CX] `nelem` 或 `elsize` 为 0 且实现不支持大小为 0 的分配。

---

*以下部分为参考信息。*

## 示例

无。

## 应用程序用法

现在不再要求实现支持包含 `<malloc.h>`。

## 基本原理

参见 `malloc()` 的基本原理。

## 未来方向

无。

## 另请参见

- [`aligned_alloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/aligned_alloc.html)
- [`free()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/free.html)
- [`malloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/malloc.html)
- [`realloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/realloc.html)

XBD [`<stdlib.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## 更改历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

标记了超出 ISO C 标准的扩展。

以下对 POSIX 实现的新要求源于与单一 UNIX 规范的对齐：

- 如果发生内存不足情况，设置 `errno` 和 [ENOMEM] 错误条件是强制性的。

### Issue 7

应用了 POSIX.1-2008，技术勘误 2，XSH/TC2-2008/0053 [526]。

### Issue 8

应用了 Austin Group 缺陷 374，更改了关于大小为 0 的分配的返回值和错误部分。

应用了 Austin Group 缺陷 1218，更改了 [ENOMEM] 错误。

应用了 Austin Group 缺陷 1302，使此函数与 ISO/IEC 9899:2018 标准对齐。

应用了 Austin Group 缺陷 1387，更改了基本原理部分。

---

