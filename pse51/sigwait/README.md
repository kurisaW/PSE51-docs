# sigwait — wait for queued signals

## SYNOPSIS

```c
#include <signal.h>

int sigwait(const sigset_t *restrict set, int *restrict sig);
```

## DESCRIPTION

The `sigwait()` function shall select a pending signal from `set`, atomically clear it from the system's set of pending signals, and return that signal number in the location referenced by `sig`. If prior to the call to `sigwait()` there are multiple pending instances of a single signal number, it is implementation-defined whether upon successful return there are any remaining pending signals for that signal number. If the implementation supports queued signals and there are multiple signals queued for the signal number selected, the first such queued signal shall cause a return from `sigwait()` and the remainder shall remain queued. If no signal in `set` is pending at the time of the call, the thread shall be suspended until one or more becomes pending. The signals defined by `set` shall have been blocked at the time of the call to `sigwait()`; otherwise, the behavior is undefined. The effect of `sigwait()` on the signal actions for the signals in `set` is unspecified.

If more than one thread is using `sigwait()` to wait for the same signal, no more than one of these threads shall return from `sigwait()` with the signal number. If more than a single thread is blocked in `sigwait()` for a signal when that signal is generated for the process, it is unspecified which of the waiting threads returns from `sigwait()`. If the signal is generated for a specific thread, as by `pthread_kill()`, only that thread shall return.

Should any of the multiple pending signals in the range SIGRTMIN to SIGRTMAX be selected, it shall be the lowest numbered one. The selection order between realtime and non-realtime signals, or between multiple pending non-realtime signals, is unspecified.

## RETURN VALUE

Upon successful completion, `sigwait()` shall store the signal number of the received signal at the location referenced by `sig` and return zero. Otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `sigwait()` function may fail if:

- **[EINVAL]** The `set` argument contains an invalid or unsupported signal number.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

To provide a convenient way for a thread to wait for a signal, this volume of POSIX.1-2024 provides the `sigwait()` function. For most cases where a thread has to wait for a signal, the `sigwait()` function should be quite convenient, efficient, and adequate.

However, requests were made for a lower-level primitive than `sigwait()` and for semaphores that could be used by threads. After some consideration, threads were allowed to use semaphores and `sem_post()` was defined to be async-signal-safe.

In summary, when it is necessary for code run in response to an asynchronous signal to notify a thread, `sigwait()` should be used to handle the signal. Alternatively, if the implementation provides semaphores, they also can be used, either following `sigwait()` or from within a signal handling routine previously registered with `sigaction()`.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [Signal Concepts](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_04)
- [Realtime Signals](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_08_01)
- [`pause()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pause.html)
- [`pthread_sigmask()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_sigmask.html)
- [`sigaction()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html)
- [`sigpending()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigpending.html)
- [`sigsuspend()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigsuspend.html)
- [`sigtimedwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigtimedwait.html)
- [`<signal.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)
- [`<time.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/time.h.html)

## CHANGE HISTORY

First released in Issue 5. Included for alignment with the POSIX Realtime Extension and the POSIX Threads Extension.

### Issue 6

The **restrict** keyword is added to the `sigwait()` prototype for alignment with the ISO/IEC 9899:1999 standard.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/131 is applied, updating the DESCRIPTION to state that if more than a single thread is blocked in `sigwait()`, it is unspecified which of the waiting threads returns, and that if a signal is generated for a specific thread only that thread shall return.

### Issue 7

Functionality relating to the Realtime Signals Extension option is moved to the Base.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0584 [76] is applied.

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*