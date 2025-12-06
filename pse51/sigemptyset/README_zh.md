# sigemptyset

## SYNOPSIS

```c
[CX] #include <signal.h>

int sigemptyset(sigset_t *set);
```

## DESCRIPTION

`sigemptyset()` 函数初始化 `set` 所指向的信号集，使得所有在 POSIX.1-2024 中定义的信号都被排除。

## RETURN VALUE

成功完成后，`sigemptyset()` 应返回 0；否则，应返回 -1 并设置 `errno` 以指示错误。

## ERRORS

未定义任何错误。

---

*以下章节为参考信息。*

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

`sigemptyset()`（或 `sigfillset()`）函数的实现可以很直接地清空（或设置）信号集中的所有位。另外，初始化结构的部分内容（如版本字段）也是合理���，这样可以允许在信号集大小不同的版本之间保持二进制兼容性。由于这些原因，在信号集的任何其他使用之前，必须调用 `sigemptyset()` 或 `sigfillset()`，即使这种使用是只读的（例如，作为 `sigpending()` 的参数）。此函数不适用于动态分配。

`sigfillset()` 和 `sigemptyset()` 函数要求生成的信号集包含（或排除）POSIX.1-2024 本卷中定义的所有信号。虽然 POSIX.1-2024 本卷的范围不包括对作为扩展实现的信号提出此要求，但建议实现定义的信号也受这些函数的影响。然而，可能有正当理由使某个特定信号不受影响。例如，阻塞或忽略一个实现定义的信号可能会产生不良副作用，而该信号的默认操作是无害的。在这种情况下，最好将此类信号从 `sigfillset()` 返回的信号集中排除。

在早期的提案中，无效信号和不支持信号之间没有区别（实现不支持的可选信号名称不会被该实现定义）。因此，[EINVAL] 错误被指定为无效信号的必需错误。有了这种区别，就不需要要求这些函数的实现来确定可选信号是否确实受支持，因为这对性能可能有显著影响而价值很小。错误本可以要求对无效信号是必需的，对不支持信号是可选的，但这似乎过于复杂。因此，在两种情况下错误都是可选的。

## FUTURE DIRECTIONS

无。

## SEE ALSO

*2.4 信号概念*, `pthread_sigmask()`, `sigaction()`, `sigaddset()`, `sigdelset()`, `sigfillset()`, `sigismember()`, `sigpending()`, `sigsuspend()`

XBD `<signal.h>`

## CHANGE HISTORY

首次在 Issue 3 中发布。为了与 POSIX.1-1988 标准对齐而包含。

### Issue 6

SYNOPSIS 被标记为 CX，因为此函数在 `<signal.h>` 头文件中的存在是对 ISO C 标准的扩展。

---
