# sigqueue

## 概要 (SYNOPSIS)

```c
#include <signal.h>

int sigqueue(pid_t pid, int signo, union sigval value);
```

## 描述 (DESCRIPTION)

`sigqueue()` 函数应将由 `signo` 指定的信号以及由 `value` 指定的值发送到由 `pid` 指定的进程。如果 `signo` 为零（空信号），则执行错误检查但实际上不发送信号。空信号可用于检查 `pid` 的有效性。

进程拥有向另一个进程队列发送信号的权限条件与 [`kill()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/kill.html) 函数相同。

`sigqueue()` 函数应立即返回。如果为 `signo` 设置了 SA_SIGINFO，并且有资源可用于队列信号，则该信号应被排队并发送到接收进程。如果没有为 `signo` 设置 SA_SIGINFO，则 `signo` 应至少发送一次到接收进程；是否将 `value` 发送到接收进程作为此调用的结果是不确定的。

如果 `pid` 的值导致为发送进程生成 `signo`，并且如果 `signo` 没有被调用线程阻塞，并且没有其他线程解除 `signo` 阻塞或在 [`sigwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigwait.html) 函数中等待 `signo`，则在 `sigqueue()` 函数返回之前，`signo` 或至少挂起的、未阻塞的信号应传递给调用线程。如果选择了 SIGRTMIN 到 SIGRTMAX 范围内的多个挂起信号进行传递，应为编号最低的那个。实时信号和非实时信号之间的选择顺序，或多个挂起的非实时信号之间的选择顺序是不确定的。

## 返回值 (RETURN VALUE)

成功完成后，指定的信号应已被排队，并且 `sigqueue()` 函数应返回零值。否则，函数应返回 -1 值并设置 `errno` 以指示错误。

## 错误 (ERRORS)

`sigqueue()` 函数应在以下情况下失败：

- **[EAGAIN]**
  - 没有资源可用于队列信号。进程已经排队了 {SIGQUEUE_MAX} 个在接收方处仍然挂起的信号，或者已超过系统范围的资源限制。

- **[EINVAL]**
  - `signo` 参数的值是无效或不支持的信号编号。

- **[EPERM]**
  - 进程没有适当的权限向接收进程发送信号。

- **[ESRCH]**
  - 进程 `pid` 不存在。

---

*以下部分为信息性内容。*

## 示例 (EXAMPLES)

无。

## 应用程序用法 (APPLICATION USAGE)

无。

## 基本原理 (RATIONALE)

`sigqueue()` 函数允许应用程序向自身或其他进程队列实时信号，指定应用程序定义的值。这在现有实时系统上的实时应用程序中是常见的做法。人们认为在已经为信号预留的 `sig`... 名称空间中指定另一个函数，比扩展 [`kill()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/kill.html) 的接口更可取。

当消息队列的 put/get 事件函数被移除时，这样的函数变得必要。应该注意的是，`sigqueue()` 函数在安全意识强的实现中意味着性能降低，因为发送方和接收方之间的访问权限必须在每次发送时检查，当 `pid` 被解析为目标进程时。在之前的接口中，此类访问检查仅在消息队列打开时是必要的。

标准开发者要求 `sigqueue()` 对于空信号具有与 [`kill()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/kill.html) 相同的语义，并且使用相同的权限检查。但由于实现 [`kill()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/kill.html) 的"广播"语义（例如，到进程组）的困难以及与资源分配的交互，没有采用这种语义。`sigqueue()` 函数将信号队列发送到由 `pid` 参数指定的单个进程。

如果系统没有足够的资源来队列信号，`sigqueue()` 函数可能失败。引入了进程可以发送的排队信号数量的明确限制。虽然限制是"每个发送方"，但 POSIX.1-2024 卷没有指定资源是发送方状态的一部分。这将要求发送方在退出后一直维护，直到它发送到其他进程的所有信号都被处理，或者所有尚未被处理的此类信号都从接收方的队列中移除。POSIX.1-2024 卷不排除此行为，但也允许实现从系统范围的池分配排队资源（具有每个发送方限制），并且在发送方退出后保持排队信号挂起。

## 未来方向 (FUTURE DIRECTIONS)

无。

## 参见 (SEE ALSO)

- [2.8.1 实时信号](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_08_01)
- XBD [<signal.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)

## 变更历史 (CHANGE HISTORY)

### 首次发布于 Issue 5
为与 POSIX 实时扩展和 POSIX 线程扩展对齐而包含。

### Issue 6
`sigqueue()` 函数被标记为实时信号扩展选项的一部分。
如果实现不支持实时信号扩展选项，则移除了 [ENOSYS] 错误条件，因为不需要提供存根。

### Issue 7
`sigqueue()` 函数从实时信号扩展选项移动到基础。
POSIX.1-2008，技术勘误 2，XSH/TC2-2008/0332 [844] 被应用。

*信息性文本结束。*

---
