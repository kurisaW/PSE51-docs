# sigaddset

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

sigaddset — add a signal to a signal set

## SYNOPSIS

```c
#include <signal.h>

int sigaddset(sigset_t *set, int signo);
```

## DESCRIPTION

The `sigaddset()` function adds the individual signal specified by the `signo` to the signal set pointed to by `set`.

Applications shall call either `sigemptyset()` or `sigfillset()` at least once for each object of type `sigset_t` prior to any other use of that object. If such an object is not initialized in this way, but is nonetheless supplied as an argument to any of `pthread_sigmask()`, `sigaction()`, `sigaddset()`, `sigdelset()`, `sigismember()`, `sigpending()`, `sigprocmask()`, `sigsuspend()`, `sigtimedwait()`, `sigwait()`, or `sigwaitinfo()`, the results are undefined.

## RETURN VALUE

Upon successful completion, `sigaddset()` shall return 0; otherwise, it shall return -1 and set `errno` to indicate the error.

## ERRORS

The `sigaddset()` function may fail if:

- **[EINVAL]**
  The value of the `signo` argument is an invalid or unsupported signal number.

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

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

### First released in Issue 3.
Included for alignment with the POSIX.1-1988 standard.

### Issue 5
The last paragraph of the DESCRIPTION was included as an APPLICATION USAGE note in previous issues.

### Issue 6
The normative text is updated to avoid use of the term "must" for application requirements.

The SYNOPSIS is marked CX since the presence of this function in the <signal.h> header is an extension over the ISO C standard.

---

