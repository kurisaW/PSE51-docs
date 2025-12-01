# munmap — 取消内存页映射

## SYNOPSIS 概要

```c
#include <sys/mman.h>

int munmap(void *addr, size_t len);
```

## DESCRIPTION 描述

`munmap()` 函数应当移除那些包含从 `addr` 开始、持续 `len` 字节的进程地址空间任何部分的整个页面的任何映射。对这些页面的进一步引用应当导致向进程生成 SIGSEGV 信号。如果在指定的地址范围内没有映射，那么 `munmap()` 将不起任何作用。

实现可能要求 `addr` 是 `sysconf()` 返回的页面大小的倍数。

如果要移除的映射是私有的，那么在此地址范围内所做的任何修改都将被丢弃。

[ML|MLR] 与此地址范围关联的任何内存锁（参见 `mlock()` 和 `mlockall()`）都应当被移除，就像通过适当的 `munlock()` 调用一样。

[TYM] 如果从类型化内存对象中移除映射导致内存池的相应地址范围除了通过可分配映射（即使用 POSIX_TYPED_MEM_MAP_ALLOCATABLE 标志打开的类型化内存对象的映射）之外，对系统中的任何进程都不可访问，那么该内存池的范围应当被解除分配，并可能变得可用于满足未来的类型化内存分配请求。

从使用 POSIX_TYPED_MEM_MAP_ALLOCATABLE 标志打开的类型化内存对象中移除的映射不应当以任何方式影响该类型化内存用于分配的可用性。

如果映射不是通过 `mmap()` 调用建立的，则此函数的行为是未指定的。

## RETURN VALUE 返回值

成功完成后，`munmap()` 应返回 0；否则，应返回 -1 并设置 `errno` 以指示错误。

## ERRORS 错误

`munmap()` 函数应在以下情况下失败：

- **[EINVAL]** 范围 [`addr`,`addr`+`len`) 内的地址超出了进程地址空间的有效范围。
- **[EINVAL]** `len` 参数为 0。

`munmap()` 函数可能在以下情况下失败：

- **[EINVAL]** `addr` 参数不是 `sysconf()` 返回的页面大小的倍数。

## EXAMPLES 示例

无。

## APPLICATION USAGE 应用程序用法

无。

## RATIONALE 基本原理

`munmap()` 函数对应于 SVR4，就像 `mmap()` 函数一样。

应用程序可能已经对包含共享内存的区域应用了进程内存锁定。如果发生了这种情况，`munmap()` 调用会忽略这些锁，并在必要时导致这些锁被移除。

大多数实现要求 `addr` 是 `sysconf()` 返回的页面大小的倍数。

## FUTURE DIRECTIONS 未来方向

无。

## SEE ALSO 另请参见

- `mlock()`
- `mlockall()`
- `mmap()`
- `posix_typed_mem_open()`
- `sysconf()`
- XBD `<sys/mman.h>`

## CHANGE HISTORY 变更历史

首次发布于 Issue 4，Version 2。

### Issue 5

从 X/OPEN UNIX 扩展移至 BASE。

与 POSIX Realtime Extension 中的 `munmap()` 对齐如下：

- DESCRIPTION 进行了大量重写。
- 不再允许生成 SIGBUS 错误。

### Issue 6

`munmap()` 函数被标记为内存映射文件和共享内存对象选项的一部分。

POSIX 实现的以下新要求来源于与 Single UNIX Specification 的对齐：

- DESCRIPTION 更新为声明实现要求 `addr` 是页面大小的倍数。
- 添加了 [EINVAL] 错误条件。

为与 IEEE Std 1003.1j-2000 对齐进行了以下更改：

- 类型化内存对象的语义被添加到 DESCRIPTION 中。
- `posix_typed_mem_open()` 函数被添加到 SEE ALSO 部分。

应用 IEEE Std 1003.1-2001/Cor 1-2002，项目 XSH/TC1/D6/36，将 SYNOPSIS 中的边距代码从 MF|SHM 更改为 MC3（MF|SHM|TYM 的表示法）。

### Issue 7

应用 Austin Group Interpretation 1003.1-2001 #078，阐明了页面对齐要求。

`munmap()` 函数从内存映射文件选项移至 Base。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*