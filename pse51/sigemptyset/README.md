# sigemptyset

## SYNOPSIS

```c
[CX] #include <signal.h>

int sigemptyset(sigset_t *set);
```

## DESCRIPTION

The `sigemptyset()` function initializes the signal set pointed to by `set`, such that all signals defined in POSIX.1-2024 are excluded.

## RETURN VALUE

Upon successful completion, `sigemptyset()` shall return 0; otherwise, it shall return -1 and set `errno` to indicate the error.

## ERRORS

No errors are defined.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

The implementation of the `sigemptyset()` (or `sigfillset()`) function could quite trivially clear (or set) all the bits in the signal set. Alternatively, it would be reasonable to initialize part of the structure, such as a version field, to permit binary-compatibility between releases where the size of the set varies. For such reasons, either `sigemptyset()` or `sigfillset()` must be called prior to any other use of the signal set, even if such use is read-only (for example, as an argument to `sigpending()`). This function is not intended for dynamic allocation.

The `sigfillset()` and `sigemptyset()` functions require that the resulting signal set include (or exclude) all the signals defined in this volume of POSIX.1-2024. Although it is outside the scope of this volume of POSIX.1-2024 to place this requirement on signals that are implemented as extensions, it is recommended that implementation-defined signals also be affected by these functions. However, there may be a good reason for a particular signal not to be affected. For example, blocking or ignoring an implementation-defined signal may have undesirable side-effects, whereas the default action for that signal is harmless. In such a case, it would be preferable for such a signal to be excluded from the signal set returned by `sigfillset()`.

In early proposals there was no distinction between invalid and unsupported signals (the names of optional signals that were not supported by an implementation were not defined by that implementation). The [EINVAL] error was thus specified as a required error for invalid signals. With that distinction, it is not necessary to require implementations of these functions to determine whether an optional signal is actually supported, as that could have a significant performance impact for little value. The error could have been required for invalid signals and optional for unsupported signals, but this seemed unnecessarily complex. Thus, the error is optional in both cases.

## FUTURE DIRECTIONS

None.

## SEE ALSO

*2.4 Signal Concepts*, `pthread_sigmask()`, `sigaction()`, `sigaddset()`, `sigdelset()`, `sigfillset()`, `sigismember()`, `sigpending()`, `sigsuspend()`

XBD `<signal.h>`

## CHANGE HISTORY

First released in Issue 3. Included for alignment with the POSIX.1-1988 standard.

### Issue 6

The SYNOPSIS is marked CX since the presence of this function in the `<signal.h>` header is an extension over the ISO C standard.

---
