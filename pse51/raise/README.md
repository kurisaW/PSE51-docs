# raise

## SYNOPSIS

```c
#include <signal.h>

int raise(int sig);
```

## DESCRIPTION

The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `raise()` function shall send the signal `sig` to the executing thread or process. If a signal handler is called, the `raise()` function shall not return until after the signal handler does.

The effect of the `raise()` function shall be equivalent to calling:

```c
pthread_kill(pthread_self(), sig);
```

## RETURN VALUE

Upon successful completion, 0 shall be returned. Otherwise, a non-zero value shall be returned and `errno` shall be set to indicate the error.

## ERRORS

The `raise()` function shall fail if:

- **[EINVAL]**
  The value of the `sig` argument is an invalid signal number.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

The term "thread" is an extension to the ISO C standard.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`kill()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/kill.html)
- [`sigaction()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html)
- XBD [`<signal.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)
- XBD [`<sys/types.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_types.h.html)

## CHANGE HISTORY

First released in Issue 4. Derived from the ANSI C standard.

### Issue 5

The DESCRIPTION is updated for alignment with the POSIX Threads Extension.

### Issue 6

Extensions beyond the ISO C standard are marked.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

- In the RETURN VALUE section, the requirement to set `errno` on error is added.
- The [EINVAL] error condition is added.

### Issue 7

Functionality relating to the Threads option is moved to the Base.

