# sigdelset - delete a signal from a signal set

## SYNOPSIS

```c
#include <signal.h>

int sigdelset(sigset_t *set, int signo);
```

## DESCRIPTION

The `sigdelset()` function deletes the individual signal specified by `signo` from the signal set pointed to by `set`.

Applications should call either `sigemptyset()` or `sigfillset()` at least once for each object of type `sigset_t` prior to any other use of that object. If such an object is not initialized in this way, but is nonetheless supplied as an argument to any of `pthread_sigmask()`, `sigaction()`, `sigaddset()`, `sigdelset()`, `sigismember()`, `sigpending()`, `sigprocmask()`, `sigsuspend()`, `sigtimedwait()`, `sigwait()`, or `sigwaitinfo()`, the results are undefined.

## RETURN VALUE

Upon successful completion, `sigdelset()` shall return 0; otherwise, it shall return -1 and set `errno` to indicate the error.

## ERRORS

The `sigdelset()` function may fail if:

**[EINVAL]**
The `signo` argument is not a valid signal number, or is an unsupported signal number.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

* [Signal Concepts](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_04)
* [pthread_sigmask()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_sigmask.html)
* [sigaction()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html)
* [sigaddset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaddset.html)
* [sigemptyset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigemptyset.html)
* [sigfillset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigfillset.html)
* [sigismember()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigismember.html)
* [sigpending()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigpending.html)
* [sigsuspend()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigsuspend.html)
* [<signal.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)

## CHANGE HISTORY

### First released in Issue 3. Included for alignment with the POSIX.1-1988 standard.

### Issue 5

The last paragraph of the DESCRIPTION was included as an APPLICATION USAGE note in previous issues.

### Issue 6

The SYNOPSIS is marked CX since the presence of this function in the `<signal.h>` header is an extension over the ISO C standard.

---
