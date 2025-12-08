# pthread_kill

## SYNOPSIS

```c
#include <signal.h>

int pthread_kill(pthread_t thread, int sig);
```

## DESCRIPTION

The `pthread_kill()` function shall request that a signal be delivered to the specified thread. It shall not be an error if `thread` is a zombie thread.

## RETURN VALUE

Upon successful completion, the function shall return a value of zero. Otherwise, the function shall return an error number. If the `pthread_kill()` function fails, no signal shall be sent.

## ERRORS

The `pthread_kill()` function may fail if:

- **[EINVAL]**
  - The value of the `sig` argument is zero.

The `pthread_kill()` function shall fail if:

- **[EINVAL]**
  - The value of the `sig` argument is non-zero and is an invalid or unsupported signal number.

The `pthread_kill()` function shall not return an error code of **[EINTR]**.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

The `pthread_kill()` function provides a mechanism for asynchronously directing a signal at a thread in the calling process. This could be used, for example, by one thread to affect broadcast delivery of a signal to a set of threads.

Note that `pthread_kill()` only causes the signal to be handled in the context of the given thread; the signal action (termination or stopping) affects the process as a whole.

## RATIONALE

If an implementation detects use of a thread ID after the end of its lifetime, it is recommended that the function should fail and report an **[ESRCH]** error.

Historical implementations varied on the result of a `pthread_kill()` with a thread ID indicating a zombie thread. Some indicated success on such a call, while others gave an error of **[ESRCH]**. Since the definition of thread lifetime in this volume of POSIX.1-2024 covers zombie threads, the **[ESRCH]** error as described is inappropriate in this case and implementations that give this error do not conform. In particular, this means that an application cannot have one thread check for termination of another by calling `pthread_kill()` with a `sig` argument of zero, and implementations may indicate that it is not possible by returning **[EINVAL]** when `sig` is zero.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`kill()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/kill.html#tag_17_296)
- [`pthread_self()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_self.html#)
- [`raise()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/raise.html#)

XBD [`<signal.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)

## CHANGE HISTORY

**First released in Issue 5.** Included for alignment with the POSIX Threads Extension.

### Issue 6

The `pthread_kill()` function is marked as part of the Threads option.

The APPLICATION USAGE section is added.

### Issue 7

The `pthread_kill()` function is moved from the Threads option to the Base.

Austin Group Interpretation 1003.1-2001 #142 is applied, removing the **[ESRCH]** error condition.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0277 [765] is applied.

### Issue 8

Austin Group Defect 792 is applied, adding a requirement that passing the thread ID of a zombie thread to `pthread_kill()` is not treated as an error.

Austin Group Defect 1214 is applied, allowing `pthread_kill()` to fail with **[EINVAL]** when the `sig` argument is zero.

