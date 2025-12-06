# sigpending

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

sigpending — 检查待处理信号

## SYNOPSIS

```c
#include <signal.h>

int sigpending(sigset_t *set);
```

## DESCRIPTION

`sigpending()` 函数应将那些被阻塞无法传递给调用线程且在进程或调用线程上处于待处理状态的信号集合存储在 `set` 参数所引用的位置中。

## RETURN VALUE

成功完成后，`sigpending()` 应返回 0；否则，应返回 -1 并设置 `errno` 以指示错误。

## ERRORS

未定义错误。

*以下部分为参考信息。*

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

[exec](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html),
[pthread_sigmask()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_sigmask.html),
[sigaddset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaddset.html),
[sigdelset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigdelset.html),
[sigemptyset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigemptyset.html),
[sigfillset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigfillset.html),
[sigismember()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigismember.html)

XBD [`<signal.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)

## CHANGE HISTORY

### 首次发布于 Issue 3。

### Issue 5

更新了 DESCRIPTION 以与 POSIX 线程扩展保持一致。

### Issue 6

将 SYNOPSIS 标记为 CX，因为该函数在 [`<signal.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html) 头文件中的存在是对 ISO C 标准的扩展。

