# pthread_attr_getstack, pthread_attr_setstack

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

pthread_attr_getstack, pthread_attr_setstack — 获取和设置栈属性

## SYNOPSIS

```c
[TSA TSS]
#include <pthread.h>

int pthread_attr_getstack(const pthread_attr_t *restrict attr,
                         void **restrict stackaddr,
                         size_t *restrict stacksize);

int pthread_attr_setstack(pthread_attr_t *attr,
                         void *stackaddr,
                         size_t stacksize);
```

## DESCRIPTION

`pthread_attr_getstack()` 和 `pthread_attr_setstack()` 函数分别用于获取和设置 `attr` 对象中的线程创建栈属性 `stackaddr` 和 `stacksize`。

栈属性指定要用于创建线程栈的存储区域。存储的基址（最低可寻址字节）应为 `stackaddr`，存储的大小应为 `stacksize` 字节。`stacksize` 应至少为 {PTHREAD_STACK_MIN}。如果 `stackaddr` 不满足实现定义的对齐要求，`pthread_attr_setstack()` 函数可能失败并返回 [EINVAL] 错误。由 `stackaddr` 和 `stacksize` 描述的栈内的所有页应都可被线程读写。

如果在设置 `stackaddr` 属性之前调用 `pthread_attr_getstack()` 函数，其行为是未指定的。

如果传递给 `pthread_attr_getstack()` 或 `pthread_attr_setstack()` 的 `attr` 参数值不指向已初始化的线程属性对象，则行为是未定义的。

## RETURN VALUE

成功完成后，这些函数应返回 0 值；否则，应返回错误号以指示错误。

如果成功，`pthread_attr_getstack()` 函数应将栈属性值存储在 `stackaddr` 和 `stacksize` 中。

## ERRORS

`pthread_attr_setstack()` 函数在以下情况应失败：

- **[EINVAL]**
  - `stacksize` 的值小于 {PTHREAD_STACK_MIN} 或超过实现定义的限制。

`pthread_attr_setstack()` 函数在以下情况可能失败：

- **[EINVAL]**
  - `stackaddr` 的值没有适当的对齐以用作栈，或 ((char *)stackaddr + stacksize) 缺乏适当的对齐。

- **[EACCES]**
  - 由 `stackaddr` 和 `stacksize` 描述的栈页不可被线程同时读写。

这些函数不应返回 [EINTR] 错误码。

---

## EXAMPLES

无。

## APPLICATION USAGE

这些函数适用于应用程序必须将线程的栈放置在内存的特定区域的环境。

虽然看起来应用程序可以通过在指定栈区域外提供受保护页来检测栈溢出，但这无法可移植地实现。实现可以自由地将线程的初始栈指针放置在指定区域内的任何位置，以适应机器的栈指针行为和分配要求。此外，在某些体系结构上，如 IA-64，"溢出"可能意味着在区域内分配的两个独立栈指针将在区域的中间某处重叠。

在成功调用 `pthread_attr_setstack()` 后，由 `stackaddr` 参数指定的存储区域由实现控制，如 [2.9.8 Use of Application-Managed Thread Stacks](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_08) 中所述。

`stackaddr` 属性的规范存在一些歧义，使得这些函数的可移植使用变得不可能。例如，标准允许实现对 `stackaddr` 强加任意的对齐要求。应用程序不能假设从 [`malloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/malloc.html) 获得的缓冲区是适当对齐的。注意，虽然传递给 `pthread_attr_setstack()` 的 `stacksize` 值必须满足对齐要求，但对于 [`pthread_attr_setstacksize()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_setstacksize.html) 则不同，如果需要实现必须增加指定的大小以实现适当的对齐。

## RATIONALE

如果实现检测到传递给 `pthread_attr_getstack()` 或 `pthread_attr_setstack()` 的 `attr` 参数值不指向已初始化的线程属性对象，建议函数应失败并报告 [EINVAL] 错误。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`pthread_attr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_destroy.html)
- [`pthread_attr_getdetachstate()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getdetachstate.html)
- [`pthread_attr_getstacksize()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getstacksize.html)
- [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html)
- XBD [`<limits.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/limits.h.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## CHANGE HISTORY

首次发布于 Issue 6。作为 XSI 选项的一部分开发，并通过 IEEE PASC Interpretation 1003.1 #101 纳入 BASE。

应用 IEEE Std 1003.1-2001/Cor 2-2004 中的项目 XSH/TC2/D6/83，更新 APPLICATION USAGE 部分以引用 [2.9.8 Use of Application-Managed Thread Stacks](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_08)。

应用 IEEE Std 1003.1-2001/Cor 2-2004 中的项目 XSH/TC/D6/84，更新 ERRORS 部分以包含当 `attr` 指向未初始化线程属性对象时的可选错误。

### Issue 7

应用 SD5-XSH-ERN-66，修正 [EINVAL] 错误条件中对 `attr` 的使用。

应用 Austin Group Interpretation 1003.1-2001 #057，阐明在设置 `stackaddr` 属性之前调用函数时的行为。

应用 SD5-XSH-ERN-157，更新 APPLICATION USAGE 部分。

在 DESCRIPTION 和 APPLICATION USAGE 部分更新了 `stackaddr` 属性的描述。

删除了针对未初始化线程属性对象的 [EINVAL] 错误；此条件导致未定义行为。

*信息性文本结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved