# sigdelset - 从信号集中删除一个信号

## SYNOPSIS（概要）

```c
#include <signal.h>

int sigdelset(sigset_t *set, int signo);
```

## DESCRIPTION（描述）

`sigdelset()` 函数从 `set` 指向的信号集中删除由 `signo` 指定的单个信号。

应用程序应在对任何 `sigset_t` 类型的对象进行任何其他使用之前，至少调用一次 `sigemptyset()` 或 `sigfillset()` 来初始化该对象。如果这样的对象没有以此方式初始化，但仍作为参数提供给 `pthread_sigmask()`、`sigaction()`、`sigaddset()`、`sigdelset()`、`sigismember()`、`sigpending()`、`sigprocmask()`、`sigsuspend()`、`sigtimedwait()`、`sigwait()` 或 `sigwaitinfo()` 中的任何一个函数，则结果未定义。

## RETURN VALUE（返回值）

成功完成时，`sigdelset()` 应返回 0；否则，应返回 -1 并设置 `errno` 来指示错误。

## ERRORS（错误）

`sigdelset()` 函数可能在以下情况下失败：

**[EINVAL]**
`signo` 参数不是有效的信号编号，或者是不支持的信号编号。

## EXAMPLES（示例）

无。

## APPLICATION USAGE（应用程序使用）

无。

## RATIONALE（原理）

无。

## FUTURE DIRECTIONS（未来方向）

无。

## SEE ALSO（参见）

* [Signal Concepts](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_04)（信号概念）
* [pthread_sigmask()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_sigmask.html)
* [sigaction()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html)
* [sigaddset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaddset.html)
* [sigemptyset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigemptyset.html)
* [sigfillset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigfillset.html)
* [sigismember()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigismember.html)
* [sigpending()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigpending.html)
* [sigsuspend()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigsuspend.html)
* [<signal.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)

## CHANGE HISTORY（变更历史）

### 首次发布于 Issue 3。为与 POSIX.1-1988 标准对齐而包含。

### Issue 5

在之前的版本中，DESCRIPTION 的最后一段作为 APPLICATION USAGE 注释包含在内。

### Issue 6

SYNOPSIS 被标记为 CX，因为此函数在 `<signal.h>` 头文件中的存在是对 ISO C 标准的扩展。

---
