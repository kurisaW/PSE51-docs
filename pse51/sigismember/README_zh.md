# sigismember

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

sigismember — 测试信号是否在信号集中

## SYNOPSIS

```c
#include <signal.h>

int sigismember(const sigset_t *set, int signo);
```

## DESCRIPTION

`sigismember()` 函数应测试由 `signo` 指定的信号是否是由 `set` 指向的集合的成员。

应用程序应在任何其他用途之前，对每个 `sigset_t` 类型的对象至少调用一次 `sigemptyset()` 或 `sigfillset()`。如果这样的对象没有通过这种方式初始化，但仍被作为参数提供给 `pthread_sigmask()`、`sigaction()`、`sigaddset()`、`sigdelset()`、`sigismember()`、`sigpending()`、`sigprocmask()`、`sigsuspend()`、`sigtimedwait()`、`sigwait()` 或 `sigwaitinfo()` 中的任何函数，则结果未定义。

## RETURN VALUE

成功完成后，如果指定的信号���指定集合的成员，`sigismember()` 应返回 1，否则返回 0。否则，它应返回 -1 并设置 `errno` 来指示错误。

## ERRORS

`sigismember()` 函数可能会失败，如果：

- **[EINVAL]**
  `signo` 参数不是有效的信号编号，或者是不支持的信号编号。

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [2.4 信号概念](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_04)
- [pthread_sigmask()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_sigmask.html)
- [sigaction()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html)
- [sigaddset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaddset.html)
- [sigdelset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigdelset.html)
- [sigfillset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigfillset.html)
- [sigemptyset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigemptyset.html)
- [sigpending()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigpending.html)
- [sigsuspend()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigsuspend.html)
- [<signal.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)

## CHANGE HISTORY

### 首次发布于 Issue 3
包含此项以与 POSIX.1-1988 标准保持一致。

### Issue 5
DESCRIPTION 的最后一段在之前的版本中作为 APPLICATION USAGE 说明包含在内。

### Issue 6
SYNOPSIS 被标记为 CX，因为此函数在 `<signal.h>` 头文件中的存在是对 ISO C 标准的扩展。

---

*UNIX® 是 The Open Group 的注册商标。*
*POSIX™ 是 The IEEE 的商标。*
*Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved*