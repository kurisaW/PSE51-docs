# malloc — 内存分配器

## 概要

```c
#include <stdlib.h>

void *malloc(size_t size);
```

## 描述

`malloc()` 函数应为指定字节大小为 `size` 的对象分配未使用的空间，该对象的值是未指定的。

通过连续调用 `malloc()` 分配的存储的顺序和连续性是未指定的。如果分配成功，返回的指针应适当对齐，以便可以将其赋给具有基本对齐要求的任何类型对象的指针，然后用于在分配的空间中访问此类对象（直到空间被显式释放或重新分配）。每次此类分配都会产生一个与任何其他对象不重叠的对象指针。返回的指针指向分配空间的开头（最低字节地址）。如果无法分配空间，应返回空指针。如果请求的空间大小为 0，其行为是实现定义的：要么返回空指针，要么行为如同大小为某个非零值，除非如果使用返回的指针访问对象，则行为是未定义的。

为确定数据竞争的存在，`malloc()` 的行为应如同它只访问可通过其参数访问的内存位置，而不访问其他静态持续时间存储。但是，该函数可以显式修改其分配的存储。调用 `aligned_alloc()`、`calloc()`、`free()`、`malloc()`、`posix_memalign()`、`reallocarray()` 和 `realloc()` 来分配或释放特定内存区域的调用应按单一总顺序发生（参见 [4.15.1 内存排序](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap04.html#tag_04_15_01)），并且每个此类释放调用应与此顺序中的下一次分配（如果有）同步。

## 返回值

成功完成时，`malloc()` 应返回指向分配空间的指针；如果 `size` 为 0，应用程序应确保不使用该指针访问对象。

否则，它应返回空指针并设置 `errno` 以指示错误。

## 错误

`malloc()` 函数在以下情况下应失败：

- **[ENOMEM]** 可用存储空间不足。

`malloc()` 函数在以下情况下可能失败：

- **[EINVAL]** `size` 为 0 且实现不支持大小为 0 的分配。

## 示例

无。

## 应用程序用法

无。

## 原理说明

某些实现将 `errno` 设置为 [EAGAIN] 以表示如果重试可能成功的内存分配失败，将 `errno` 设置为 [ENOMEM] 表示由于配置限制等原因不太可能成功的失败。[2.3 错误编号](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_03) 允许这种行为；当多个错误条件同时为真时，它们之间没有优先级。

## 未来方向

无。

## 另请参阅

- [`aligned_alloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/aligned_alloc.html)
- [`calloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/calloc.html)
- [`free()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/free.html)
- [`getrlimit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getrlimit.html)
- [`posix_memalign()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_memalign.html)
- [`realloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/realloc.html)

XBD [`<stdlib.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

标记了超出 ISO C 标准的扩展。

以下对 POSIX 实现的新要求源于与单一 UNIX 规范的对齐：

- 在 RETURN VALUE 部分，添加了设置 `errno` 以指示错误的要求。
- 添加了 [ENOMEM] 错误条件。

### Issue 7

应用了 POSIX.1-2008 技术勘误 2，XSH/TC2-2008/0203 [526]。

### Issue 8

应用了 Austin Group 缺陷 374，更改了与大小为 0 的分配相关的 RETURN VALUE 和 ERRORS 部分。

应用了 Austin Group 缺陷 1302，使此函数与 ISO/IEC 9899:2018 标准对齐。

应用了 Austin Group 缺陷 1387 和 1489，更改了 RATIONALE 部分。

---
