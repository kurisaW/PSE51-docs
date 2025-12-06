# exit

## SYNOPSIS

```c
#include <stdlib.h>

void exit(int status);
```

## DESCRIPTION

`exit()` 函数应当导致正常进程终止的发生。不会调用通过 `at_quick_exit()` 函数注册的任何函数。如果一个进程多次调用 `exit()` 函数，或者在调用 `exit()` 函数的同时还调用 `quick_exit()` 函数，则其行为是未定义的。

`status` 的值可以是 0、`EXIT_SUCCESS`、`EXIT_FAILURE` 或任何其他值，不过只有最低的 8 位（即 `status` & 0377）可以从 `wait()` 和 `waitpid()` 获取；完整的值可以从 `waitid()` 和传递给 SIGCHLD 信号处理程序的 `siginfo_t` 中获取。

`exit()` 函数应当首先调用所有通过 `atexit()` 注册的函数，按其注册的相反顺序调用，但如果一个函数在注册时已有先前注册的函数已经被调用过，则该函数在这些之前注册的函数之后被调用。每个函数被调用的次数与它注册的次数相同。如果在调用任何此类函数的过程中，进行了 `longjmp()` 函数调用，该调用会终止对已注册函数的调用，则其行为是未定义的。

如果通过调用 `atexit()` 注册的函数未能返回，则剩余的已注册函数不会被调用，`exit()` 处理的其余部分也不会完成。

`exit()` 函数然后应当刷新所有带有未写入缓冲数据的打开流。对于每个作为其底层文件描述符的活动句柄，且文件尚未到达 EOF 且能够定位的流，底层打开文件描述的文件偏移量应当设置为该流的文件位置。对于每个打开的流，`exit()` 函数应当对与该流关联的文件描述符执行等效于 `close()` 的操作。最后，进程应当终止，具有与_进程终止的后果_中描述的相同后果。

## RETURN VALUE

`exit()` 函数不返回。

## ERRORS

没有定义错误。

## EXAMPLES

参见 APPLICATION USAGE。

## APPLICATION USAGE

当 `exit()` 刷新带有未写入缓冲数据的流时，调用进程无法发现 `exit()` 是否成功将数据写入了底层文件描述符。因此，强烈建议应用程序在调用 `exit()` 或从 `main()` 的初始调用返回时，始终确保任何流中没有未写入的缓冲数据，并使用表示没有发生错误的 `status` 值。

例如，以下代码演示了一种确保 `stdout` 在使用状态 0 调用 `exit()` 之前已经成功刷新的方法。如果刷新失败，`stdout` 的底层文件描述符被关闭，这样 `exit()` 就不会尝试重复失败的写入操作。如果刷新成功，则执行使用 `ferror()` 的最终检查，以确保在之前的刷新操作中（当时未处理的）没有写入错误。

```c
int status = 0;
if (fflush(stdout) != 0) {
    perror("appname: standard output");
    close(fileno(stdout));
    status = 1;
}
else if (ferror(stdout)) {
    fputs("appname: write error on standard output\n", stderr);
    status = 1;
}
exit(status);
```

另见 `_Exit()`。

## RATIONALE

参见 `_Exit()`。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `_Exit()`
- `at_quick_exit()`
- `atexit()`
- `exec`
- `fflush()`
- `longjmp()`
- `quick_exit()`
- `tmpfile()`
- `wait()`
- `waitid()`
- `<stdlib.h>`

## CHANGE HISTORY

### Issue 7

应用 Austin Group Interpretation 1003.1-2001 #031，将 `_Exit()` 和 `_exit()` 函数与 `exit()` 函数分离。

应用 Austin Group Interpretation 1003.1-2001 #085。

应用 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0100 [594]。

### Issue 8

应用 Austin Group Defect 610，阐明 `exit()` 对打开流的影响。

应用 Austin Group Defect 1302，使此函数与 ISO/IEC 9899:2018 标准保持一致。

应用 Austin Group Defect 1490，更改 EXAMPLES 和 APPLICATION USAGE 部分。

应用 Austin Group Defect 1629，更改 APPLICATION USAGE 部分。

---
