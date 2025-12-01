# pthread_setconcurrency, pthread_getconcurrency

## NAME

pthread_setconcurrency, pthread_getconcurrency - set and get the concurrency level

## SYNOPSIS

```c
#include <pthread.h>

int pthread_setconcurrency(int new_level);
int pthread_getconcurrency(void);
```

## DESCRIPTION

Unbound threads in a process may or may not be required to be simultaneously active. By default, the threads implementation ensures that a sufficient number of threads are active so that the process can continue to make progress. While this conserves system resources, it may not produce the most effective level of concurrency.

The `pthread_setconcurrency()` function allows an application to inform the threads implementation of its desired concurrency level, `new_level`. The actual level of concurrency provided by the implementation as a result of this function call is unspecified.

If `new_level` is zero, it causes the implementation to maintain the concurrency level at its discretion as if `pthread_setconcurrency()` had never been called.

The `pthread_getconcurrency()` function shall return the value set by a previous call to the `pthread_setconcurrency()` function. If the `pthread_setconcurrency()` function was not previously called, this function shall return zero to indicate that the implementation is maintaining the concurrency level.

A call to `pthread_setconcurrency()` shall inform the implementation of its desired concurrency level. The implementation shall use this as a hint, not a requirement.

If an implementation does not support multiplexing of user threads on top of several kernel-scheduled entities, the `pthread_setconcurrency()` and `pthread_getconcurrency()` functions are provided for source code compatibility but they shall have no effect when called. To maintain the function semantics, the `new_level` parameter is saved when `pthread_setconcurrency()` is called so that a subsequent call to `pthread_getconcurrency()` shall return the same value.

## RETURN VALUE

If successful, the `pthread_setconcurrency()` function shall return zero; otherwise, an error number shall be returned to indicate the error.

The `pthread_getconcurrency()` function shall always return the concurrency level set by a previous call to `pthread_setconcurrency()`. If the `pthread_setconcurrency()` function has never been called, `pthread_getconcurrency()` shall return zero.

## ERRORS

No errors are defined for these functions.

## EXAMPLES

None provided.

## APPLICATION USAGE

These functions allow applications to provide hints to the threads implementation about the desired level of concurrency. The implementation is not required to honor these hints, and the actual concurrency level may differ from the requested level.

## RATIONALE

The concurrency level functions provide a mechanism for applications to influence thread scheduling behavior when they have knowledge about the optimal number of simultaneously active threads. However, implementations are given flexibility in how they respond to these hints to accommodate different threading models and system architectures.

## FUTURE DIRECTIONS

None.

## SEE ALSO

None.

## COPYRIGHT

Portions of this text are reprinted and reproduced in electronic form from the IEEE Std 1003.1, 2013 Edition, Standard for Information Technology -- Portable Operating System Interface (POSIX), The Open Group Base Specifications Issue 7, Copyright (C) 2013 by the Institute of Electrical and Electronics Engineers, Inc and The Open Group. (This is POSIX.1-2008 with the 2013 Technical Corrigendum 1 applied.) In the event of any discrepancy between this version and the original IEEE and The Open Group Standard, the original IEEE and The Open Group Standard shall be the referee. The original Standard can be obtained online at http://www.opengroup.org/unix/online.html.

*The following sections are informative.*

*End of informative text.*