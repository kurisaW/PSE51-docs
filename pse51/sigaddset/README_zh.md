# sigaddset

## SYNOPSIS

```c
#include <signal.h>

int sigaddset(sigset_t *set, int signo);
```

## DESCRIPTION

`sigaddset()` 函数将由 `signo` 指定的单个信号添加到��� `set` 指向的信号集中。

应用程序在首次使用任何 `sigset_t` 类型的对象之前，必须至少调用一次 `sigemptyset()` 或 `sigfillset()` 函数来初始化该对象。如果这样的对象未通过此方式初始化，但仍作为参数提供给 `pthread_sigmask()`、`sigaction()`、`sigaddset()`、`sigdelset()`、`sigismember()`、`sigpending()`、`sigprocmask()`、`sigsuspend()`、`sigtimedwait()`、`sigwait()` 或 `sigwaitinfo()` 中的任何一个函数，则结果是未定义的。

## RETURN VALUE

成功完成后，`sigaddset()` 应返回 0；否则，应返回 -1 并设置 `errno` 以指示错误。

## ERRORS

`sigaddset()` 函数可能在以下情况下失败：

- **[EINVAL]**
  `signo` 参数的值是无效或不支持的信号编号。

*以下章节是参考性内容。*

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [2.4 Signal Concepts](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_04)
- [pthread_sigmask()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_sigmask.html)
- [sigaction()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html)
- [sigdelset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigdelset.html)
- [sigemptyset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigemptyset.html)
- [sigfillset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigfillset.html)
- [sigismember()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigismember.html)
- [sigpending()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigpending.html)
- [sigsuspend()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigsuspend.html)
- XBD [<signal.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)

## CHANGE HISTORY

### 首次发布于 Issue 3。
为与 POSIX.1-1988 标准对齐而包含。

### Issue 5
DESCRIPTION 的最后一段在之前的版本中作为 APPLICATION USAGE 注释包含。

### Issue 6
更新规范性文本以避免对应用程序要求使用术语 "must"。

SYNOPSIS 被标记为 CX，因为此函数在 <signal.h> 头文件中的存在是对 ISO C 标准的扩展。

---

*参考性文本结束。*

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group，保留所有权利