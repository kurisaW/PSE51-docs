# raise

## SYNOPSIS

```c
#include <signal.h>

int raise(int sig);
```

## DESCRIPTION

本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 本标准遵循 ISO C 标准。

`raise()` 函数应向执行线程或进程发送信号 `sig`。如果调用了信号处理函数，`raise()` 函数应等到信号处理函数执行完成后才返回。

`raise()` 函数的效果应等同于调用：

```c
pthread_kill(pthread_self(), sig);
```

## RETURN VALUE

成功完成后，应返回 0。否则，应返回非零值并设置 `errno` 来指示错误。

## ERRORS

`raise()` 函数在以下情况下应失败：

- **[EINVAL]**
  `sig` 参数的值是无效的信号编号。

---

*以下章节为参考信息。*

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

术语"线程"是对 ISO C 标准的扩展。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`kill()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/kill.html)
- [`sigaction()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html)
- XBD [`<signal.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)
- XBD [`<sys/types.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_types.h.html)

## CHANGE HISTORY

首次发布于第 4 版。衍生自 ANSI C 标准。

### Issue 5

更新了 DESCRIPTION 以与 POSIX 线程扩展保持一致。

### Issue 6

超出 ISO C 标准的扩展已被标记。

与单一 UNIX 规范对齐产生的 POSIX 实现新要求如下：

- 在 RETURN VALUE 部分，添加了错误时设置 `errno` 的要求。
- 添加了 [EINVAL] 错误条件。

### Issue 7

与线程选项相关的功能已移至基础部分。

