# kill

## SYNOPSIS

```c
#include <signal.h>

int kill(pid_t pid, int sig);
```

## DESCRIPTION

`kill()` 函数应向由 `pid` 指定的进程或进程组发送信号。要发送的信号由 `sig` 指定，可以是 `<signal.h>` 中列出的信号之一，也可以是 0。如果 `sig` 为 0（空信号），则执行错误检查但不实际发送信号。空信号可用于检查 `pid` 的有效性。

要使进程有权向由 `pid` 指定的进程发送信号，除非发送进程具有适当的权限，否则发送进程的实际或有效用户 ID 必须与接收进程的实际或已保存的设置用户 ID 匹配。

如果 `pid` 大于 0，`sig` 应发送给进程 ID 等于 `pid` 的进程。

如果 `pid` 为 0，`sig` 应发送给所有进程组 ID 等于发送者进程组 ID 且进程有权限发送信号的进程（不包括一组未指定的系统进程）。

如果 `pid` 为 -1，`sig` 应发送给进程有权限发送该信号的所有进程（不包括一组未指定的系统进程）。

如果 `pid` 为负数但不是 -1，`sig` 应发送给所有进程组 ID 等于 `pid` 绝对值且进程有权限发送信号的进程（不包括一组未指定的系统进程）。

如果 `pid` 的值导致为发送进程生成 `sig`，并且如果 `sig` 对于调用线程未被阻塞，并且如果没有其他线程解除对 `sig` 的阻塞或在 `sigwait()` 函数中等待 `sig`，那么在 `kill()` 返回之前，`sig` 或至少一个待处理的未阻塞信号应被传递给发送线程。

当向与发送进程属于同一会话的进程发送 SIGCONT 时，不应应用上述用户 ID 检查。

提供扩展安全控制的实现可能会对信号发送（包括空信号）施加进一步的实现定义的限制。特别是，系统可能拒绝 `pid` 指定的部分或全部进程的存在。

如果进程有权限向 `pid` 指定的任何进程发送 `sig`，则 `kill()` 函数成功。如果 `kill()` 失败，则不会发送任何信号。

## RETURN VALUE

成功完成后，应返回 0。否则，应返回 -1 并设置 `errno` 以指示错误。

## ERRORS

`kill()` 函数在以下情况下应失败：

- **[EINVAL]**
  `sig` 参数的值是无效或不支持的信号编号。

- **[EPERM]**
  进程没有权限向任何接收进程发送信号。

- **[ESRCH]**
  找不到与 `pid` 指定的进程或进程组相对应的进程。

---

## APPLICATION USAGE

无。

## RATIONALE

`kill()` 的权限检查语义在 System V 和大多数其他实现（如 Version 7 或 4.3 BSD）之间有所不同。本卷 POSIX.1-2024 选择的语义与 System V 一致。具体来说，设置用户 ID 进程无法保护自己免受信号（或至少免受 SIGKILL）的影响，除非它更改其实际用户 ID。这个选择允许启动应用程序的用户即使应用程序更改了其有效用户 ID 也能向其发送信号。其他语义则赋予想要保护自己免受运行它的用户影响的应用程序更多权力。

一些实现在 `pid` 的绝对值大于某个最大值或其他特殊值时，为 `kill()` 函数提供语义扩展。负数值是 `kill()` 的标志。由于大多数实现在此情况下返回 [ESRCH]，因此此行为未包含在本卷 POSIX.1-2024 中，尽管符合标准的实现可以提供此类扩展。

不能向其发送信号的未指定进程可能包括调度器或 `init`。

最初有强烈意向规定，如果 `pid` 指定向调用进程发送信号且该信号未被阻塞，那么该信号应在 `kill()` 返回之前被传递。这将允许进程调用 `kill()` 并保证调用永不返回。然而，仅提供 `signal()` 函数的历史实现只满足本卷 POSIX.1-2024 中的较弱保证，因为它们只在进程每次进入内核时传递一个信号。对此类实现的修改以支持 `sigaction()` 函数通常需要在信号捕获函数返回后进入内核以恢复信号掩码。此类修改具有满足较强要求的效果，至少在使用 `sigaction()` 时是这样，但在使用 `signal()` 时不一定。标准开发者考虑过制定较强要求，但在使用 `signal()` 时除外，但认为这会不必要地复杂。鼓励实现者在可能的情况下满足较强要求。在实践中，较弱要求是相同的，除了在非常短的时间窗口内到达两个信号的罕见情况。此推理同样适用于 `sigprocmask()` 的类似要求。

在 4.2 BSD 中，SIGCONT 信号可以发送给任何后代进程，而无需进行用户 ID 安全检查。这允许作业控制 shell 继续作业，即使作业中的进程已更改其用户 ID（如 `su` 命令）。为了与会话概念的添加保持一致，通过允许向同一会话中的任何进程发送 SIGCONT 信号而无需进行用户 ID 安全检查，提供了类似功能。这比 BSD 限制更少，因为祖先进程（在同一会话中）现在可以成为接收者。这比 BSD 限制更多，因为形成新会话的后代进程现在需要接受用户 ID 检查。对于其他作业控制信号，不需要类似的安全放宽，因为这些信号通常由终端驱动程序发送以识别输入的特殊字符；终端驱动程序绕过所有安全检查。

在安全实现中，进程可能被限制向具有不同安全标签的进程发送信号。为了防止进程的存在或不存在被用作隐蔽通道，此类进程应对发送者显示不存在；也就是说，如果 `pid` 仅指此类进程，应返回 [ESRCH]，而不是 [EPERM]。

历史实现在 `pid` 指示僵尸进程的 `kill()` 结果上存在差异。一些实现在此类调用上指示成功（取决于权限检查），而其他实现则给出 [ESRCH] 错误。由于本卷 POSIX.1-2024 中进程生命周期的定义涵盖僵尸进程，因此描述的 [ESRCH] 错误在此情况下是不合适的，给出此错误的实现不符合标准。这意味着应用程序不能让父进程通过使用 `kill()` 向其发送空信号来检查特定子进程的终止，而必须使用 `waitpid()` 或 `waitid()`。

有人认为 `kill()` 的名称具有误导性，因为该函数并非总是旨在导致进程终止。然而，该名称在所有历史实现中都是通用的，任何更改都将与对现有应用程序代码进行最小更改的目标冲突。

## FUTURE DIRECTIONS

无。

## SEE ALSO

[`getpid()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getpid.html), [`raise()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/raise.html), [`setsid()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setsid.html), [`sig2str()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sig2str.html), [`sigaction()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html), [`sigqueue()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigqueue.html), [`wait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/wait.html)

XBD [`<signal.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html), [`<sys/types.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_types.h.html)

## CHANGE HISTORY

首次在 Issue 1 中发布。派生自 SVID 的 Issue 1。

### Issue 5

DESCRIPTION 已更新以与 POSIX 线程扩展保持一致。

### Issue 6

在 SYNOPSIS 中，移除了可选包含 `<sys/types.h>` 头文件。

以下对 POSIX 实现的新要求源于与单一 UNIX 规范的对齐：

- 在 DESCRIPTION 中，第二段被重新措辞，以指示检查调用进程的已保存设置用户 ID，而不是其有效用户 ID。这是一个 FIPS 要求。
- 包含 `<sys/types.h>` 的要求已被移除。尽管 `<sys/types.h>` 对于先前 POSIX 规范的符合性实现是必需的，但对于 UNIX 应用程序来说不是必需的。
- 当 `pid` 为 -1 时的行为现在已指定。这在 POSIX.1-1988 标准中之前是明确未指定的。

规范性文本已更新，以避免对应用程序要求使用术语"必须"。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/51，更正了 RATIONALE 部分。

### Issue 7

应用了 POSIX.1-2008，技术勘误 2，XSH/TC2-2008/0194 [765]。

### Issue 8

应用了 Austin Group Defect 1138，将 `sig2str()` 添加到 SEE ALSO 部分。

---
