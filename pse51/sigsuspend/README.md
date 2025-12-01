# sigsuspend — wait for a signal

## SYNOPSIS

```c
#include <signal.h>

int sigsuspend(const sigset_t *sigmask);
```

## DESCRIPTION

The `sigsuspend()` function shall atomically both replace the current signal mask of the calling thread with the set of signals pointed to by `sigmask` and suspend the thread until delivery of a signal whose action is either to execute a signal-catching function or to terminate the process. This shall not cause any other signals that may have been pending on the process to become pending on the thread.

If the action is to terminate the process then `sigsuspend()` shall never return. If the action is to execute a signal-catching function, then `sigsuspend()` shall return after the signal-catching function returns, with the signal mask restored to the set that existed prior to the `sigsuspend()` call.

It is not possible to block signals that cannot be ignored. This is enforced by the system without causing an error to be indicated.

## RETURN VALUE

Since `sigsuspend()` suspends thread execution indefinitely, there is no successful completion return value. If a return occurs, -1 shall be returned and `errno` set to indicate the error.

## ERRORS

The `sigsuspend()` function shall fail if:

- **[EINTR]** - A signal is caught by the calling process and control is returned from the signal-catching function.

## APPLICATION USAGE

Normally, at the beginning of a critical code section, a specified set of signals is blocked using the `sigprocmask()` function. When the thread has completed the critical section and needs to wait for the previously blocked signal(s), it pauses by calling `sigsuspend()` with the mask that was returned by the `sigprocmask()` call.

## RATIONALE

Code which wants to avoid the ambiguity of the signal mask for thread cancellation handlers can install an additional cancellation handler which resets the signal mask to the expected value.

```c
void cleanup(void *arg)
{
    sigset_t *ss = (sigset_t *) arg;
    pthread_sigmask(SIG_SETMASK, ss, NULL);
}

int call_sigsuspend(const sigset_t *mask)
{
    sigset_t oldmask;
    int result;
    pthread_sigmask(SIG_SETMASK, NULL, &oldmask);
    pthread_cleanup_push(cleanup, &oldmask);
    result = sigsuspend(mask);
    pthread_cleanup_pop(0);
    return result;
}
```

## SEE ALSO

- [2.4 Signal Concepts](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_04)
- [pause()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pause.html)
- [sigaction()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaction.html)
- [sigaddset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigaddset.html)
- [sigdelset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigdelset.html)
- [sigemptyset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigemptyset.html)
- [sigfillset()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigfillset.html)
- [<signal.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/signal.h.html)

## CHANGE HISTORY

### First released in Issue 3
Included for alignment with the POSIX.1-1988 standard.

### Issue 5
The DESCRIPTION is updated for alignment with the POSIX Threads Extension.

### Issue 6
- The text in the RETURN VALUE section has been changed from "suspends process execution" to "suspends thread execution". This reflects IEEE PASC Interpretation 1003.1c #40.
- Text in the APPLICATION USAGE section has been replaced.
- The SYNOPSIS is marked CX since the presence of this function in the <signal.h> header is an extension over the ISO C standard.

### Issue 7
SD5-XSH-ERN-122 is applied, adding the example code in the RATIONALE.

### Issue 8
- Austin Group Defect 1201 is applied, clarifying the atomicity requirements for `sigsuspend()`.
- Austin Group Defect 1223 is applied, changing the example code in the RATIONALE.

---

*Source: The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*