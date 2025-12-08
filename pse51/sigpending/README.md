# sigpending

## SYNOPSIS

```c
#include <signal.h>

int sigpending(sigset_t *set);
```

## DESCRIPTION

The `sigpending()` function shall store, in the location referenced by the `set` argument, the set of signals that are blocked from delivery to the calling thread and that are pending on the process or the calling thread.

## RETURN VALUE

Upon successful completion, `sigpending()` shall return 0; otherwise, -1 shall be returned and `errno` set to indicate the error.

## ERRORS

No errors are defined.

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

[exec](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html),
[pthread_sigmask()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_sigmask.html),
[sigaddset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaddset.html),
[sigdelset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigdelset.html),
[sigemptyset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigemptyset.html),
[sigfillset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigfillset.html),
[sigismember()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigismember.html)

XBD [`<signal.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)

## CHANGE HISTORY

### First released in Issue 3.

### Issue 5

The DESCRIPTION is updated for alignment with the POSIX Threads Extension.

### Issue 6

The SYNOPSIS is marked CX since the presence of this function in the [`<signal.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html) header is an extension over the ISO C standard.

