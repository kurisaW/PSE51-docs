# alarm

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

* * *

## NAME

alarm — 安排闹钟信号

## SYNOPSIS

```c
#include <unistd.h>

unsigned alarm(unsigned seconds);
```

## DESCRIPTION

`alarm()` 函数应使系统在经过 `seconds` 参数指定的实时秒数后为进程生成 SIGALRM 信号。处理器调度延迟可能会阻止进程在信号生成时立即处理该信号。

如果 `seconds` 为 0，则取消任何挂起的闹钟请求。

闹钟请求不会堆叠；只能以这种方式调度一个 SIGALRM 信号生成。如果 SIGALRM 信号尚未生成，则调用将导致重新调度生成 SIGALRM 信号的时间。

## RETURN VALUE

如果存在先前的 `alarm()` 请求且仍有剩余时间，`alarm()` 应返回一个非零值，该值是先前景会生成 SIGALRM 信号的秒数。否则，`alarm()` 应返回 0。

## ERRORS

`alarm()` 函数总是成功的，没有保留返回值来表示错误。

* * *

*以下章节为参考信息。*

## EXAMPLES

无。

## APPLICATION USAGE

`fork()` 函数会清除子进程中挂起的闹钟。由 `exec` 系列函数之一创建的新进程映像会继承旧进程映像中闹钟信号的剩余时间。

应用程序开发人员应注意，参数 `seconds` 和 `alarm()` 的返回值类型为 **unsigned**。这意味着严格符合的 POSIX 系统接口应用程序不能传递大于 {UINT_MAX} 最小保证值的值，ISO C 标准将其设置为 65535，任何传递更大值的应用程序都会限制其可移植性。曾考虑过使用不同的类型，但历史实现（包括具有 16 位 **int** 类型的实现）一致使用 **unsigned** 或 **int**。

应用程序开发人员应注意同一进程同时使用 `alarm()` 和 `sleep()` 函数时可能产生的交互。

## RATIONALE

许多历史实现（包括 Version 7 和 System V）允许闹钟最多提前一秒触发。其他实现允许闹钟最多提前半秒或一个时钟节拍触发，或者根本不允许提前触发。后者被认为是最合适的，因为它提供了最可预测的行为，特别是因为信号由于调度原因总是可以被无限期延迟。因此，应用程序可以将 `seconds` 参数选择为它们希望在信号之前经过的最小时间量。

此处和其他地方（`sleep()`、`times()`）使用的术语"实时"意在表示日常英语用法中的"挂钟时间"，与"实时操作系统"无关。它与*虚拟时间*形成对比，如果仅使用*时间*一词，可能会被误解。

在某些实现中，包括 4.3 BSD，`seconds` 参数的非常大的值会被静默向下舍入为特定于实现的最大值。该最大值足够大（以几个月为单位），以至于效果不明显。

在多线程应用程序中，闹钟生成有两种可能的选择：为调用线程生成或为进程生成。第一个选项不会特别有用，因为闹钟状态是按进程维护的，并且由最后一次调用 `alarm()` 建立的闹钟是唯一活跃的。

此外，允许为线程生成异步信号会在整体信号模型中引入例外。这需要有令人信服的理由才能被证明是合理的。

## FUTURE DIRECTIONS

无。

## SEE ALSO

[alarm](#), [exec](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html), [fork()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fork.html), [pause()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pause.html), [sigaction()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html), [sleep()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sleep.html), [timer_create()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/timer_create.html)

XBD [<signal.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html), [<unistd.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/unistd.h.html)

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID Issue 1。

### Issue 6

对 POSIX 实现的以下新要求源自与单一 UNIX 规范的对齐：

*   更新了 DESCRIPTION 以指明与 `setitimer()`、`ualarm()` 和 `usleep()` 函数的交互是未指定的。

应用了 IEEE Std 1003.1-2001/Cor 2-2004 项目 XSH/TC2/D6/16，在 RATIONALE 中将"实现定义的最大值"替换为"特定于实现的最大值"。

### Issue 8

应用了 Austin Group 缺陷 1330，移除了过时的接口。

*参考信息结束。*

* * *

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved
[ [Main Index](https://pubs.opengroup.org/onlinepubs/9799919799/mindex.html) | [XBD](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/contents.html) | [XSH](https://pubs.opengroup.org/onlinepubs/9799919799/functions/contents.html) | [XCU](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/contents.html) | [XRAT](https://pubs.opengroup.org/onlinepubs/9799919799/xrat/contents.html) ]

* * *