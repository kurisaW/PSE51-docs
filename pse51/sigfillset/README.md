# sigfillset - initialize and fill a signal set

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

sigfillset — initialize and fill a signal set

## SYNOPSIS

```c
#include <signal.h>

int sigfillset(sigset_t *set);
```

## DESCRIPTION

The `sigfillset()` function shall initialize the signal set pointed to by `set`, such that all signals defined in this volume of POSIX.1-2024 are included.

## RETURN VALUE

Upon successful completion, `sigfillset()` shall return 0; otherwise, it shall return -1 and set `errno` to indicate the error.

## ERRORS

No errors are defined.

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

Refer to `sigemptyset()`.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- 2.4 Signal Concepts
- `pthread_sigmask()`
- `sigaction()`
- `sigaddset()`
- `sigdelset()`
- `sigemptyset()`
- `sigismember()`
- `sigpending()`
- `sigsuspend()`
- XBD `<signal.h>`

## CHANGE HISTORY

First released in Issue 3. Included for alignment with the POSIX.1-1988 standard.

### Issue 6

The SYNOPSIS is marked CX since the presence of this function in the `<signal.h>` header is an extension over the ISO C standard.

