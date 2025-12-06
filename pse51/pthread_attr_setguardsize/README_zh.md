# pthread_attr_getguardsize

## SYNOPSIS

```c
#include <pthread.h>

int pthread_attr_getguardsize(const pthread_attr_t *restrict attr,
                              size_t *restrict guardsize);
int pthread_attr_setguardsize(pthread_attr_t *attr,
                              size_t guardsize);
```

## DESCRIPTION

`pthread_attr_getguardsize()` 函数应获取 `attr` 对象中的 `guardsize` 属性。此属性应在 `guardsize` 参数中返回。

`pthread_attr_setguardsize()` 函数应设置 `attr` 对象中的 `guardsize` 属性。此属性的新值应从 `guardsize` 参数获取。如果 `guardsize` 为零，则使用 `attr` 创建的线程不应提供警戒区。如果 `guardsize` 大于零，则使用 `attr` 创建的每个线程应提供至少大小为 `guardsize` 字节的警戒区。

`guardsize` 属性控制所创建线程栈的警戒区大小。`guardsize` 属性提供对栈指针溢出的保护。如果线程的栈是在警戒保护下创建的，实现会在栈的溢出端分配额外内存作为缓冲区，以防止栈指针的栈溢出。如果应用程序溢出到此缓冲区中，则应产生错误（可能导致向线程传递 SIGSEGV 信号）。

一致性实现可能将 `guardsize` 中包含的值向上舍入到可配置系统变量 {PAGESIZE} 的倍数（参见 `<sys/mman.h>`）。如果实现将 `guardsize` 的值向上舍入到 {PAGESIZE} 的倍数，则指定 `attr` 的 `pthread_attr_getguardsize()` 调用应在 `guardsize` 参数中存储由前一个 `pthread_attr_setguardsize()` 函数调用指定的警戒区大小。

`guardsize` 属性的默认值由实现定义。

如果已设置 `stackaddr` 属性（即调用者正在分配和管理自己的线程栈），则应忽略 `guardsize` 属性，实现不应提供保护。在这种情况下，应用程序负责管理栈溢出以及栈分配和管理。

如果 `pthread_attr_getguardsize()` 或 `pthread_attr_setguardsize()` 的 `attr` 参数指定的值不引用已初始化的线程属性对象，则行为未定义。

## RETURN VALUE

如果成功，`pthread_attr_getguardsize()` 和 `pthread_attr_setguardsize()` 函数应返回零；否则，应返回错误号以指示错误。

## ERRORS

这些函数可能在以下情况下失败：

- **[EINVAL]** - 参数 `guardsize` 无效。

这些函数不应返回 [EINTR] 错误代码。

---

*以下部分为提供信息的内容。*

## EXAMPLES

### 获取警戒区大小属性

此示例显示如何获取线程属性对象的 `guardsize` 属性。

```c
#include <pthread.h>

pthread_attr_t thread_attr;
size_t  guardsize;
int     rc;

/* 初始化 thread_attr 的代码 */
...

rc = pthread_attr_getguardsize (&thread_attr, &guardsize);
if (rc != 0)  {
    /* 处理错误 */
    ...
}
else {
    if (guardsize > 0) {
    /* 提供了至少 guardsize 字节的警戒区 */
    ...
    }
    else {
    /* 未提供警戒区 */
    ...
    }
}
```

## APPLICATION USAGE

无。

## RATIONALE

向应用程序提供 `guardsize` 属性有两个原因：

1. 溢出保护可能导致系统资源浪费。创建大量线程且知道其线程从不溢出栈的应用程序可以通过关闭警戒区来节省系统资源。

2. 当线程在栈上分配大型数据结构时，可能需要大型警戒区来检测栈溢出。

警戒区的默认大小由实现定义，因为在支持非常大页面大小的系统上，如果默认要求至少一个警戒页，开销可能很大。

如果实现检测到 `pthread_attr_getguardsize()` 或 `pthread_attr_setguardsize()` 的 `attr` 参数指定的值不引用已初始化的线程属性对象，建议函数应失败并报告 [EINVAL] 错误。

## FUTURE DIRECTIONS

无。

## SEE ALSO

XBD `<pthread.h>`, `<sys/mman.h>`

## CHANGE HISTORY

### 首次发布于 Issue 5。

### Issue 6

在 ERRORS 部分中，删除了第三个 [EINVAL] 错误条件，因为它已被第二个错误条件覆盖。

`restrict` 关键字被添加到 `pthread_attr_getguardsize()` 原型中，以与 ISO/IEC 9899:1999 标准对齐。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/74，更新 ERRORS 部分以删除 [EINVAL] 错误（"属性 `attr` 无效。"），并用可选的 [EINVAL] 错误替换。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/76，向 EXAMPLES 部分添加了示例。

### Issue 7

应用了 SD5-XSH-ERN-111，删除了 DESCRIPTION 中对 `stack` 属性的引用。

应用了 SD5-XSH-ERN-175，更新 DESCRIPTION 以注意警戒区的默认大小由实现定义。

`pthread_attr_getguardsize()` 和 `pthread_attr_setguardsize()` 函数从 XSI 选项移动到 Base。

删除了未初始化线程属性对象的 [EINVAL] 错误；此条件导致未定义行为。

---

*信息性文本结束。*

**版权所有 © 2001-2024 The IEEE and The Open Group，保留所有权利**