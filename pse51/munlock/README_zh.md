# munlock

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

---

## 名称 (NAME)

mlock, munlock — 锁定或解锁进程地址空间的一个范围 (**REALTIME**)

## 概要 (SYNOPSIS)

```c
[MLR]
#include <sys/mman.h>

int mlock(const void *addr, size_t len);
int munlock(const void *addr, size_t len);
```

## 描述 (DESCRIPTION)

`mlock()` 函数应导致包含从地址 `addr` 开始、持续 `len` 字节的进程地址空间任意部分的所有整页，在解锁之前或进程退出或 `exec` 另一个进程映像之前常驻内存。实现可能要求 `addr` 是 {PAGESIZE} 的倍数。

`munlock()` 函数应解锁包含从地址 `addr` 开始、持续 `len` 字节的进程地址空间任意部分的所有整页，无论进程已对指定范围内的任何页面调用了多少次 `mlock()`。实现可能要求 `addr` 是 {PAGESIZE} 的倍数。

如果在一次 `munlock()` 调用中指定的范围内有任何页面也映射到了其他进程的地址空间中，那么由其他进程在这些页面上建立的任何锁定都不会受到此进程调用 `munlock()` 的影响。如果在一次 `munlock()` 调用中指定的范围内有任何页面也映射到了调用进程地址空间中指定范围之外的其他部分，那么通过其他映射在这些页面上建立的任何锁定也不会受到此调用的影响。

从 `mlock()` 成功返回时，指定范围内的页面应被锁定并常驻内存。从 `munlock()` 成功返回时，指定范围内的页面应相对于进程的地址空间被解锁。解锁页面的内存驻留性是未指定的。

使用 `mlock()` 锁定进程内存需要适当的权限。

## 返回值 (RETURN VALUE)

成功完成时，`mlock()` 和 `munlock()` 函数应返回值为零。否则，进程地址空间中的任何锁定都不会发生更改，函数应返回值为 -1 并设置 `errno` 来指示错误。

## 错误 (ERRORS)

`mlock()` 和 `munlock()` 函数在以下情况下应失败：

- **[ENOMEM]**
  由 `addr` 和 `len` 参数指定的地址范围的某些或全部不对应于进程地址空间中的有效映射页面。

`mlock()` 函数在以下情况下应失败：

- **[EAGAIN]**
  在进行调用时，操作所标识的某些或全部内存无法被锁定。

`mlock()` 和 `munlock()` 函数在以下情况下可能失败：

- **[EINVAL]**
  `addr` 参数不是 {PAGESIZE} 的倍数。

`mlock()` 函数在以下情况下可能失败：

- **[ENOMEM]**
  锁定由指定范围映射的页面将超过实现定义的进程可锁定内存量的限制。
- **[EPERM]**
  调用进程没有执行请求操作的适当权限。

---

*以下各节为参考性内容。*

## 示例 (EXAMPLES)

无。

## 应用程序使用 (APPLICATION USAGE)

无。

## 原理 (RATIONALE)

无。

## 未来方向 (FUTURE DIRECTIONS)

无。

## 参见 (SEE ALSO)

[`exec()`](exec.html), [`_exit()`](exit.html), [`fork()`](fork.html), [`mlockall()`](mlockall.html), [`munmap()`](munmap.html)

XBD [`<sys/mman.h>`](sys_mman.h.html)

## 变更历史 (CHANGE HISTORY)

首次发布于 Issue 5。为与 POSIX 实时扩展保持一致而包含在内。

### Issue 6

`mlock()` 和 `munlock()` 函数被标记为范围内存锁定选项的一部分。

如果实现不支持范围内存锁定选项，则不需要提供存根，因此 [ENOSYS] 错误条件已被移除。

*参考性文本结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group，保留所有权利