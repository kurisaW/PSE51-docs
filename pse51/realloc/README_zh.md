# realloc, reallocarray — 内存重新分配器

## 概要

```c
#include <stdlib.h>

void *realloc(void *ptr, size_t size);

[CX] void *reallocarray(void *ptr, size_t nelem, size_t elsize);
```

## 描述

对于 `realloc()`：[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非预期的。POSIX.1-2024 本卷遵循 ISO C 标准。

`realloc()` 函数应该释放 `ptr` 指向的旧对象，并返回一个指向具有 `size` 指定大小的新对象的指针。新对象的内容在释放前应与旧对象的内容相同，直到新旧大小中的较小者为止。新对象中超出旧对象大小的任何字节都具有不确定的值。

[CX] `reallocarray()` 函数应等同于调用 `realloc(ptr, nelem * elsize)`，不同之处在于乘法溢出应为错误。

如果 `ptr` 是空指针，`realloc()` [CX] 或 `reallocarray()` 应等同于为指定大小调用 `malloc()`。否则，如果 `ptr` 与之前由 `aligned_alloc()`、`calloc()`、`malloc()`、[ADV] `posix_memalign()`、`realloc()`、[CX] `reallocarray()` 或 POSIX.1-2024 中如同通过 `malloc()` 分配内存的函数返回的指针不匹配，或者如果空间已经通过调用 `free()`、[CX] `reallocarray()` 或 `realloc()` 被释放，则行为未定义。

如果 `size` 非零且新对象的内存未分配，则旧对象不应被释放。

通过连续调用 `realloc()` [CX] 或 `reallocarray()` 分配的存储的顺序和连续性是未指定的。如果分配成功，返回的指针应适当对齐，以便可以将其赋值给具有基本对齐要求的任何类型对象的指针，然后用于访问分配空间中的此类对象（直到空间被显式释放或重新分配）。每次此类分配应产生一个与任何其他对象不相交的对象的指针。返回的指针应指向分配空间的起始位置（最低字节地址）。如果无法分配空间，应返回空指针。

为了确定数据竞争的存在，`realloc()` [CX] 和 `reallocarray()` 的行为应如同只访问通过其参数可访问的内存位置，而不访问其他静态持续存储。然而，函数可以明显修改其分配的存储。对特定内存区域进行分配或释放的 `aligned_alloc()`、`calloc()`、`free()`、`malloc()`、[ADV] `posix_memalign()`、[CX] `reallocarray()` 和 `realloc()` 调用应发生在单一总顺序中（参见 4.15.1 内存排序），并且每个此类释放调用应与此顺序中的下一个分配（如果有）同步。

## 返回值

成功完成时，`realloc()` [CX] 和 `reallocarray()` 应返回指向新对象的指针（该值可以与指向旧对象的指针相同），如果新对象未分配，则返回空指针。

[OB] 如果 `size` 为 0，[OB CX] 或 `nelem` 或 `elsize` 为 0，[OB] 则：

如果没有足够的可用内存，`realloc()` [CX] 和 `reallocarray()` 应返回空指针 [CX] 并将 `errno` 设置为 [ENOMEM]。

## 错误

如果出现以下情况，`realloc()` [CX] 和 `reallocarray()` 函数应失败：

**[ENOMEM]**
[CX] 可用内存不足。

[CX] 如果出现以下情况，`reallocarray()` 函数应失败：

**[ENOMEM]**
计算 `nelem * elsize` 会溢出。

如果出现以下情况，`realloc()` [CX] 和 `reallocarray()` 函数可能失败：

**[EINVAL]**
[CX] 请求的分配大小为 0 且实现不支持大小为 0 的分配。

## 示例

无。

## 应用程序用法

ISO C 标准规定了 `realloc(p, 0)` 调用在因新对象的内存未分配而返回空指针时是否释放 p 指向的空间是由实现定义的。POSIX.1 反而要求如果返回空指针且空间未被释放，实现应设置 `errno`，而 POSIX 应用程序应仅在 `errno` 被更改时才释放空间。

## 基本原理

参见 `malloc()` 的基本原理。

## 未来方向

ISO C 标准指出，使用等于零的 `size` 参数调用 `realloc()` 是一个已过时的特性。此特性可能在标准的未来版本中被移除。

## 另请参见

- `aligned_alloc()`
- `calloc()`
- `free()`
- `malloc()`
- `<stdlib.h>`

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

标记了超出 ISO C 标准的扩展。

以下对 POSIX 实现的新要求源于与单一 UNIX 规范的对齐：

- 在 RETURN VALUE 部分，如果没有足够的可用内存，添加了将 `errno` 设置为 [ENOMEM]。
- 添加了 [ENOMEM] 错误条件。

### Issue 7

应用了 POSIX.1-2008、技术勘误 1、XSH/TC1-2008/0495 [400]、XSH/TC1-2008/0496 [400]、XSH/TC1-2008/0497 [400] 和 XSH/TC1-2008/0498 [400]。

应用了 POSIX.1-2008、技术勘误 2、XSH/TC2-2008/0309 [526] 和 XSH/TC2-2008/0310 [526,688]。

### Issue 8

应用了 Austin Group 缺陷 374，添加了 [EINVAL] 错误。

应用了 Austin Group 缺陷 1218，添加了 `reallocarray()`。

应用了 Austin Group 缺陷 1302，使 `realloc()` 函数与 ISO/IEC 9899:2018 标准对齐。

应用了 Austin Group 缺陷 1387，更改了基本原理部分。

---
