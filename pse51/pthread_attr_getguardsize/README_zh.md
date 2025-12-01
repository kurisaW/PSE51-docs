# pthread_attr_getguardsize, pthread_attr_setguardsize — 获取和设置线程保护大小���性

## SYNOPSIS

```c
#include <pthread.h>

int pthread_attr_getguardsize(const pthread_attr_t *restrict attr,
                             size_t *restrict guardsize);

int pthread_attr_setguardsize(pthread_attr_t *attr,
                             size_t guardsize);
```

## DESCRIPTION

`pthread_attr_getguardsize()` 函数应获取 `attr` 对象中的 `guardsize` 属性。该属性应通过 `guardsize` 参数返回。

`pthread_attr_setguardsize()` 函数应设置 `attr` 对象中的 `guardsize` 属性。此属性的新值应从 `guardsize` 参数获取。如果 `guardsize` 为零，则不应为使用 `attr` 创建的线程提供保护区。如果 `guardsize` 大于零，则应为每个使用 `attr` 创建的线程提供至少大小为 `guardsize` 字节的保护区。

`guardsize` 属性控制创建线程的栈的保护区域大小。`guardsize` 属性提供对栈指针溢出的保护。如果线程的栈是使用保护机制创建的，实现会在栈的溢出端分配额外的内存，作为栈指针栈溢出的缓冲区。如果应用程序溢出到这个缓冲区中，将会产生错误（可能会向线程传递 `SIGSEGV` 信号）。

一致实现可以将 `guardsize` 中的值向上舍入为可配置系统变量 `{PAGESIZE}` 的倍数（参见 `<sys/mman.h>`）。如果实现将 `guardsize` 的值向上舍入为 `{PAGESIZE}` 的倍数，那么指定 `attr` 的 `pthread_attr_getguardsize()` 调用应在前一个 `pthread_attr_setguardsize()` 函数调用指定的保护大小存储到 `guardsize` 参数中。

`guardsize` 属性的默认值由实现定义。

如果设置了 `stackaddr` 属性（即调用者正在分配和管理自己的线程栈），则应忽略 `guardsize` 属性，实现不应提供保护。在这种情况下，应用程序有责任管理栈溢出以及栈的分配和管理。

如果 `pthread_attr_getguardsize()` 或 `pthread_attr_setguardsize()` 的 `attr` 参数指定的值不引用已初始化的线程属性对象，则行为未定义。

## RETURN VALUE

如果成功，`pthread_attr_getguardsize()` 和 `pthread_attr_setguardsize()` 函数应返回零；否则，应返回错误编号以指示错误。

## ERRORS

这些函数可能在以下情况下失败：

- **EINVAL**
  - 参数 `guardsize` 无效。

这些函数不应返回 `[EINTR]` 错误代码。

## EXAMPLES

### 获取 guardsize 属性

此示例显示如何获取线程属性对象的 `guardsize` 属性。

```c
#include <pthread.h>

pthread_attr_t thread_attr;
size_t guardsize;
int rc;

/* 初始化 thread_attr 的代码 */
...

rc = pthread_attr_getguardsize(&thread_attr, &guardsize);
if (rc != 0) {
    /* 处理错误 */
    ...
}
else {
    if (guardsize > 0) {
        /* 提供了至少 guardsize 字节大小的保护区 */
        ...
    }
    else {
        /* 没有提供保护区 */
        ...
    }
}
```

## APPLICATION USAGE

无。

## RATIONALE

向应用程序提供 `guardsize` 属性有两个原因：

1. 溢出保护可能导致系统资源浪费。创建大量线程且知道其线程永远不会溢出栈的应用程序可以通过关闭保护区来节省系统资源。

2. 当线程在栈上分配大型数据结构时，可能需要大的保护区来检测栈溢出。

保护区的默认大小由实现定义，因为在支持非常大页面大小的系统上，如果默认要求至少一个保护页面，开销可能会很大。

如果实现检测到 `pthread_attr_getguardsize()` 或 `pthread_attr_setguardsize()` 的 `attr` 参数指定的值不引用已初始化的线程属性对象，建议函数应失败并报告 `[EINVAL]` 错误。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `<pthread.h>`
- `<sys/mman.h>`

## CHANGE HISTORY

### 首次发布于 Issue 5。

### Issue 6

- 在 ERRORS 部分中，删除了第三个 `[EINVAL]` 错误条件，因为它已被第二个错误条件涵盖。
- 为了与 ISO/IEC 9899:1999 标准对齐，向 `pthread_attr_getguardsize()` 原型添加了 `restrict` 关键字。
- 应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/74，更新 ERRORS 部分以删除 `[EINVAL]` 错误（"属性 `attr` 无效。"），并用可选的 `[EINVAL]` 错误替换它。
- 应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/76，向 EXAMPLES 部分添加了示例。

### Issue 7

- 应用了 SD5-XSH-ERN-111，删除了 DESCRIPTION 中对 `stack` 属性的引用。
- 应用了 SD5-XSH-ERN-175，更新 DESCRIPTION 以注意保护区的默认大小由实现定义。
- `pthread_attr_getguardsize()` 和 `pthread_attr_setguardsize()` 函数从 XSI 选项移动到 Base。
- 删除了未初始化线程属性对象的 `[EINVAL]` 错误；此条件导致未定义行为。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*版权所有 © 2001-2024 The IEEE and The Open Group*