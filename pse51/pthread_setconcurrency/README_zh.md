# pthread_setconcurrency, pthread_getconcurrency

## SYNOPSIS

```c
#include <pthread.h>

int pthread_setconcurrency(int new_level);
int pthread_getconcurrency(void);
```

## DESCRIPTION

进程中的非绑定线程可能需要也可能不需要同时激活。默认情况下，线程实现确保有足够数量的线程处于活动状态，以便进程能够继续取得进展。虽然这样做节省了系统资源，但可能不会产生最有效的并发级别。

`pthread_setconcurrency()` 函数允许应用程序通知线程实现其期望的并发级别 `new_level`。由于此函数调用，实现提供的实际并发级别是未指定的。

如果 `new_level` 为零，将导致实现自行决定维护并发级别，就像从未调用过 `pthread_setconcurrency()` 一样。

`pthread_getconcurrency()` 函数应返回先前调用 `pthread_setconcurrency()` 函数所设置的值。如果先前未调用过 `pthread_setconcurrency()` 函数，则此函数应返回零，以表示实现正在维护并发级别。

调用 `pthread_setconcurrency()` 应通知实现其期望的并发级别。实现应将此作为提示（hint），而非要求。

如果实现不支持在多个内核调度实体之上对用户线程进行多路复用，则提供 `pthread_setconcurrency()` 和 `pthread_getconcurrency()` 函数是为了源代码兼容性，但在调用时它们不起作用。为了保持函数语义，在调用 `pthread_setconcurrency()` 时会保存 `new_level` 参数，以便后续调用 `pthread_getconcurrency()` 能够返回相同的值。

## RETURN VALUE

如果成功，`pthread_setconcurrency()` 函数应返回零；否则，应返回错误码以指示错误。

`pthread_getconcurrency()` 函数应始终返回先前调用 `pthread_setconcurrency()` 所设置的并发级别。如果从未调用过 `pthread_setconcurrency()` 函数，`pthread_getconcurrency()` 应返回零。

## ERRORS

未定义这些函数的错误。

## EXAMPLES

未提供示例。

## APPLICATION USAGE

这些函数允许应用程序向线程实现提供有关期望并发级别的提示。实现不要求必须遵循这些提示，实际并发级别可能与请求的级别不同。

## RATIONALE

并发级别函数为应用程序提供了一种机制，当它们具有关于同时活动线程最优数量的知识时，可以影响线程调度行为。然而，实现在如何响应这些提示方面被赋予了灵活性，以适应不同的线程模型和系统架构。

## FUTURE DIRECTIONS

无。

## SEE ALSO

无。

## COPYRIGHT

本文本部分内容转载并复制自 IEEE Std 1003.1, 2013 Edition, Standard for Information Technology -- Portable Operating System Interface (POSIX), The Open Group Base Specifications Issue 7, 版权所有 (C) 2013 by the Institute of Electrical and Electronics Engineers, Inc and The Open Group。（这是应用了 2013 年技术勘误 1 的 POSIX.1-2008。）如果此版本与原始 IEEE 和 The Open Group 标准之间存在任何差异，应以原始 IEEE 和 The Open Group 标准为准。原始标准可在线获取：http://www.opengroup.org/unix/online.html。

*以下部分为参考信息。*
