# signal — signal management

## SYNOPSIS

```c
#include <signal.h>

void (*signal(int sig, void (*func)(int)))(int);
```

## DESCRIPTION

The `signal()` function chooses one of three ways in which receipt of the signal number `sig` is to be subsequently handled. If the value of `func` is SIG_DFL, default handling for that signal shall occur. If the value of `func` is SIG_IGN, the signal shall be ignored. Otherwise, the application shall ensure that `func` points to a function to be called when that signal occurs. An invocation of such a function because of a signal, or (recursively) of any further functions called by that invocation (other than functions in the standard library), is called a "signal handler".

When a signal occurs, and `func` points to a function, it is implementation-defined whether the equivalent of a:

```c
signal(sig, SIG_DFL);
```

is executed or the implementation prevents some implementation-defined set of signals (at least including `sig`) from occurring until the current signal handling has completed. (If the value of `sig` is SIGILL, the implementation may alternatively define that no action is taken.) Next the equivalent of:

```c
(*func)(sig);
```

is executed. If and when the function returns, if the value of `sig` was SIGFPE, SIGILL, or SIGSEGV or any other implementation-defined value corresponding to a computational exception, the behavior is undefined. Otherwise, the program shall resume execution at the point it was interrupted.

If the process is multi-threaded, or if the process is single-threaded and a signal handler is executed other than as the result of:

- The process calling `abort()`, `raise()`, `kill()`, `pthread_kill()`, or `sigqueue()` to generate a signal that is not blocked
- A pending signal being unblocked and being delivered before the call that unblocked it returns

the behavior is undefined if:

[Additional behavior conditions continue in the full specification...]

At program start-up, the equivalent of:

```c
signal(sig, SIG_IGN);
```

is executed for some signals, and the equivalent of:

```c
signal(sig, SIG_DFL);
```

is executed for all other signals (see `exec`).

The `signal()` function shall not change the setting of `errno` if successful.

The `signal()` function is required to be thread-safe.

## RETURN VALUE

If the request can be honored, `signal()` shall return the value of `func` for the most recent call to `signal()` for the specified signal `sig`. Otherwise, SIG_ERR shall be returned and a positive value shall be stored in `errno`.

## ERRORS

The `signal()` function shall fail if:

- **[EINVAL]** The `sig` argument is not a valid signal number or an attempt is made to catch a signal that cannot be caught or ignore a signal that cannot be ignored.

The `signal()` function may fail if:

- **[EINVAL]** An attempt was made to set the action to SIG_DFL for a signal that cannot be caught or ignored (or both).

## APPLICATION USAGE

The `sigaction()` function provides a more comprehensive and reliable mechanism for controlling signals; new applications should use `sigaction()` rather than `signal()`.

## RATIONALE

The ISO C standard says that the use of `signal()` in a multi-threaded program results in undefined behavior. However, POSIX.1 has required `signal()` to be thread-safe since before threads were added to the ISO C standard.

## SEE ALSO

- [2.4 Signal Concepts](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_04)
- [exec](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html)
- [pause()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pause.html)
- [raise()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/raise.html)
- [sigaction()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html)
- [sigsuspend()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigsuspend.html)
- [waitid()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/waitid.html)
- [<signal.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 5
Moved from X/OPEN UNIX extension to BASE.

### Issue 6
Extensions beyond the ISO C standard are marked.

### Issue 7
POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0580 [275], XSH/TC1-2008/0581 [66], and XSH/TC1-2008/0582 [105] are applied.
POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0331 [785] is applied.

### Issue 8
Austin Group Defect 728 is applied, reducing the set of circumstances in which undefined behavior results when a signal handler refers to an object with static or thread storage duration.
Austin Group Defect 1302 is applied, aligning this function with the ISO/IEC 9899:2018 standard.

---
