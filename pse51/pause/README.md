# pause

## SYNOPSIS

```c
#include <unistd.h>

int pause(void);
```

## DESCRIPTION

The `pause()` function shall suspend the calling thread until delivery of a signal whose action is either to execute a signal-catching function or to terminate the process.

If the action is to terminate the process, `pause()` shall not return.

If the action is to execute a signal-catching function, `pause()` shall return after the signal-catching function returns.

## RETURN VALUE

Since `pause()` suspends thread execution indefinitely unless interrupted by a signal, there is no successful completion return value. A value of -1 shall be returned and `errno` set to indicate the error.

## ERRORS

The `pause()` function shall fail if:

- **[EINTR]**
  A signal is caught by the calling process and control is returned from the signal-catching function.

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

Many common uses of `pause()` have timing windows. The scenario involves checking a condition related to a signal and, if the signal has not occurred, calling `pause()`. When the signal occurs between the check and the call to `pause()`, the process often blocks indefinitely. The `sigprocmask()` and `sigsuspend()` functions can be used to avoid this type of problem.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`sigsuspend()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigsuspend.html)
- XBD [`<unistd.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/unistd.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 5

The DESCRIPTION is updated for alignment with the POSIX Threads Extension.

### Issue 6

The APPLICATION USAGE section is added.

