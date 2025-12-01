# pthread_atfork

## Name

pthread_atfork - register fork handlers

## Synopsis

```c
#include <pthread.h>

int pthread_atfork(void (*prepare)(void), void (*parent)(void), void (*child)(void));
```

## Description

The following sections are informative.

### Rationale

There are at least two serious problems with the semantics of `fork()` in a multi-threaded program. One problem has to do with state (for example, memory) covered by mutexes. Consider the case where one thread has a mutex locked and the state covered by that mutex is inconsistent while another thread calls `fork()`. In the child, the mutex is in the locked state (locked by a nonexistent thread and thus can never be unlocked). Having the child simply reinitialize the mutex is unsatisfactory since this approach does not resolve the question about how to correct or otherwise deal with the inconsistent state in the child.

It is suggested that programs that use `fork()` call an `exec` function very soon afterwards in the child process, thus resetting all states. In the meantime, only a short list of async-signal-safe library routines are promised to be available.

Unfortunately, this solution does not address the needs of multi-threaded libraries. Application programs may not be aware that a multi-threaded library is in use, and they feel free to call any number of library routines between the `fork()` and `exec` calls, just as they always have. Indeed, they may be extant single-threaded programs and cannot, therefore, be expected to obey new restrictions imposed by the threads library.

On the other hand, the multi-threaded library needs a way to protect its internal state during `fork()` in case it is re-entered later in the child process. The problem arises especially in multi-threaded I/O libraries, which are almost sure to be invoked between the `fork()` and `exec` calls to effect I/O redirection. The solution may require locking mutex variables during `fork()`, or it may entail simply resetting the state in the child after the `fork()` processing completes.

The `pthread_atfork()` function was intended to provide multi-threaded libraries with a means to protect themselves from innocent application programs that call `fork()`, and to provide multi-threaded application programs with a standard mechanism for protecting themselves from `fork()` calls in a library routine or the application itself.

The expected usage was that the prepare handler would acquire all mutex locks and the other two fork handlers would release them.

For example, an application could have supplied a prepare routine that acquires the necessary mutexes the library maintains and supplied child and parent routines that release those mutexes, thus ensuring that the child would have got a consistent snapshot of the state of the library (and that no mutexes would have been left stranded). This is good in theory, but in reality not practical. Each and every mutex and lock in the process must be located and locked. Every component of a program including third-party components must participate and they must agree who is responsible for which mutex or lock. This is especially problematic for mutexes and locks in dynamically allocated memory. All mutexes and locks internal to the implementation must be locked, too. This possibly delays the thread calling `fork()` for a long time or even indefinitely since uses of these synchronization objects may not be under control of the application. A final problem to mention here is the problem of locking streams. At least the streams under control of the system (like `stdin`, `stdout`, `stderr`) must be protected by locking the stream with `flockfile()`. But the application itself could have done that, possibly in the same thread calling `fork()`. In this case, the process will deadlock.

Alternatively, some libraries might have been able to supply just a `child` routine that reinitializes the mutexes in the library and all associated states to some known value (for example, what it was when the image was originally executed). This approach is not possible, though, because implementations are allowed to fail `*_init_()` and `*_destroy_()` calls for mutexes and locks if the mutex or lock is still locked. In this case, the `child` routine is not able to reinitialize the mutexes and locks.

When `fork()` is called, only the calling thread is duplicated in the child process. Synchronization variables remain in the same state in the child as they were in the parent at the time `fork()` was called. Thus, for example, mutex locks may be held by threads that no longer exist in the child process, and any associated states may be inconsistent. The intention was that the parent process could have avoided this by explicit code that acquires and releases locks critical to the child via `pthread_atfork()`. In addition, any critical threads would have needed to be recreated and reinitialized to the proper state in the child (also via `pthread_atfork()`).

A higher-level package may acquire locks on its own data structures before invoking lower-level packages. Under this scenario, the order specified for fork handler calls allows a simple rule of initialization for avoiding package deadlock: a package initializes all packages on which it depends before it calls the `pthread_atfork()` function for itself.

As explained, there is no suitable solution for functionality which requires non-atomic operations to be protected through mutexes and locks. This is why the POSIX.1 standard since the 1996 release requires that the child process after `fork()` in a multi-threaded process only calls async-signal-safe interfaces.

An additional problem arises when `pthread_atfork()` is called to register a function in a library that was loaded using `dlopen()`. If the library is unloaded using `dlclose()`, and the implementation of `dlclose()` does not unregister the function, then when `fork()` tries to call it the result will be undefined behavior. Some implementations of `dlclose()` do unregister `pthread_atfork()` handlers, but this cannot be relied upon by portable applications. The standard provides no portable method for unregistering a function installed as a handler via `pthread_atfork()`.

## See Also

- [`exec`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html)
- [`fork`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fork.html)
- [`flockfile`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/flockfile.html)
- [`dlopen`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/dlopen.html)
- [`dlclose`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/dlclose.html)