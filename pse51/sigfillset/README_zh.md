# sigfillset - 初始化并填充信号集

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

sigfillset — 初始化并填充信号集

## SYNOPSIS

```c
#include <signal.h>

int sigfillset(sigset_t *set);
```

## DESCRIPTION

`sigfillset()` 函数应初始化 `set` 所指向的信号集，使其包含 POSIX.1-2024 本卷中定义的所有信号。

## RETURN VALUE

成功完成时，`sigfillset()` 应返回 0；否则，应返回 -1 并设置 `errno` 来指示错误。

## ERRORS

未定义错误。

*以下章节为参考信息。*

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

请参考 `sigemptyset()`。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- 2.4 信号概念
- `pthread_sigmask()`
- `sigaction()`
- `sigaddset()`
- `sigdelset()`
- `sigemptyset()`
- `sigismember()`
- `sigpending()`
- `sigsuspend()`
- XBD `<signal.h>`

## CHANGE HISTORY

首次在 Issue 3 中发布。为与 POSIX.1-1988 标准对齐而包含。

### Issue 6

SYNOPSIS 被标记为 CX，因为此函数在 `<signal.h>` 头文件中的存在是对 ISO C 标准的扩展。

*参考信息结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved