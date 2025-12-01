# nanosleep — high resolution sleep

## SYNOPSIS

```c
#include <time.h>

int nanosleep(const struct timespec *rqtp, struct timespec *rmtp);
```

## DESCRIPTION

The `nanosleep()` function shall cause the current thread to be suspended from execution until either the time interval specified by the `rqtp` argument has elapsed or a signal is delivered to the calling thread, and its action is to invoke a signal-catching function or to terminate the process. The suspension time may be longer than requested because the argument value is rounded up to an integer multiple of the sleep resolution or because of the scheduling of other activity by the system. But, except for the case of being interrupted by a signal, the suspension time shall not be less than the time specified by `rqtp`, as measured by the system clock CLOCK_REALTIME.

The use of the `nanosleep()` function has no effect on the action or blockage of any signal.

## RETURN VALUE

If the `nanosleep()` function returns because the requested time has elapsed, its return value shall be zero.

If the `nanosleep()` function returns because it has been interrupted by a signal, it shall return a value of -1 and set `errno` to indicate the interruption. If the `rmtp` argument is non-NULL, the `timespec` structure referenced by it is updated to contain the amount of time remaining in the interval (the requested time minus the time actually slept). The `rqtp` and `rmtp` arguments can point to the same object. If the `rmtp` argument is NULL, the remaining time is not returned.

If `nanosleep()` fails, it shall return a value of -1 and set `errno` to indicate the error.

## ERRORS

The `nanosleep()` function shall fail if:

- **[EINTR]**
  The `nanosleep()` function was interrupted by a signal.

- **[EINVAL]**
  The `rqtp` argument specified a nanosecond value less than zero or greater than or equal to 1000 million.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

It is common to suspend execution of a thread for an interval in order to poll the status of a non-interrupting function. A large number of actual needs can be met with a simple extension to `sleep()` that provides finer resolution.

In the POSIX.1-1990 standard and SVR4, it is possible to implement such a routine, but the frequency of wakeup is limited by the resolution of the `alarm()` and `sleep()` functions. In 4.3 BSD, it is possible to write such a routine using no static storage and reserving no system facilities. Although it is possible to write a function with similar functionality to `sleep()` using the remainder of the `timer_*()` functions, such a function requires the use of signals and the reservation of some signal number. This volume of POSIX.1-2024 requires that `nanosleep()` be non-intrusive of the signals function.

The `nanosleep()` function shall return a value of 0 on success and -1 on failure or if interrupted. This latter case is different from `sleep()`. This was done because the remaining time is returned via an argument structure pointer, `rmtp`, instead of as the return value.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `clock_nanosleep()`
- `sleep()`
- `<time.h>`

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Realtime Extension.

### Issue 6
- The `nanosleep()` function is marked as part of the Timers option.
- The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Timers option.
- IEEE Std 1003.1-2001/Cor 1-2002, item XSH/TC1/D6/37 is applied, updating the SEE ALSO section to include the `clock_nanosleep()` function.
- IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/63 is applied, correcting text in the RATIONALE section.

### Issue 7
- SD5-XBD-ERN-33 is applied.
- The `nanosleep()` function is moved from the Timers option to the Base.
- POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0231 [909] is applied.

---

*Source: The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*