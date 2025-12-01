# timer_create - create a per-process timer

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

timer_create — create a per-process timer

## SYNOPSIS

```c
#include <signal.h>
#include <time.h>

int timer_create(clockid_t clockid,
                 struct sigevent *restrict evp,
                 timer_t *restrict timerid);
```

## DESCRIPTION

The `timer_create()` function shall create a per-process timer using the specified clock, `clock_id`, as the timing base. The `timer_create()` function shall return, in the location referenced by `timerid`, a timer ID of type `timer_t` used to identify the timer in timer requests. This timer ID shall be unique within the calling process until the timer is deleted. The particular clock, `clock_id`, is defined in `<time.h>`. The timer whose ID is returned shall be in a disarmed state upon return from `timer_create()`.

The `evp` argument, if non-NULL, points to a `sigevent` structure. This structure, allocated by the application, defines the asynchronous notification to occur as specified in Signal Generation and Delivery when the timer expires. If the `evp` argument is NULL, the effect is as if the `evp` argument pointed to a `sigevent` structure with the `sigev_notify` member having the value SIGEV_SIGNAL, the `sigev_signo` having a default signal number, and the `sigev_value` member having the value of the timer ID.

Each implementation shall define a set of clocks that can be used as timing bases for per-process timers. All implementations shall support CLOCK_REALTIME and CLOCK_MONOTONIC as values for `clock_id`.

Per-process timers shall not be inherited by a child process across a `fork()` and shall be disarmed and deleted by an `exec`.

If _POSIX_CPUTIME is defined, implementations shall support `clock_id` values representing the CPU-time clock of the calling process.

If _POSIX_THREAD_CPUTIME is defined, implementations shall support `clock_id` values representing the CPU-time clock of the calling thread.

It is implementation-defined whether a `timer_create()` function will succeed if the value defined by `clock_id` corresponds to the CPU-time clock of a process or thread different from the process or thread invoking the function.

If `evp->sigev_notify` is SIGEV_THREAD and `sev->sigev_notify_attributes` is not NULL, if the attribute pointed to by `sev->sigev_notify_attributes` has a thread stack address specified by a call to `pthread_attr_setstack()`, the results are unspecified if the signal is generated more than once.

## RETURN VALUE

If the call succeeds, `timer_create()` shall return zero and update the location referenced by `timerid` to a `timer_t`, which can be passed to the per-process timer calls. If an error occurs, the function shall return a value of -1 and set `errno` to indicate the error. The value of `timerid` is undefined if an error occurs.

## ERRORS

The `timer_create()` function shall fail if:

- **[EAGAIN]**
  - The system lacks sufficient signal queuing resources to honor the request.
  - The calling process has already created all of the timers it is allowed by this implementation.

- **[EINVAL]**
  - The specified clock ID is not defined.

- **[ENOTSUP]**
  - The implementation does not support the creation of a timer attached to the CPU-time clock that is specified by `clock_id` and associated with a process or thread different from the process or thread invoking `timer_create()`.

## EXAMPLES

None.

## APPLICATION USAGE

If a timer is created which has `evp->sigev_notify` set to SIGEV_THREAD and the attribute pointed to by `evp->sigev_notify_attributes` has a thread stack address specified by a call to `pthread_attr_setstack()`, the memory dedicated as a thread stack cannot be recovered. The reason for this is that the threads created in response to a timer expiration are created detached, or in an unspecified way if the thread attribute's `detachstate` is PTHREAD_CREATE_JOINABLE. In neither case is it valid to call `pthread_join()`, which makes it impossible to determine the lifetime of the created thread which thus means the stack memory cannot be reused.

## RATIONALE

### Periodic Timer Overrun and Resource Allocation

The specified timer facilities may deliver realtime signals (that is, queued signals) on implementations that support this option. Since realtime applications cannot afford to lose notifications of asynchronous events, like timer expirations or asynchronous I/O completions, it must be possible to ensure that sufficient resources exist to deliver the signal when the event occurs. In general, this is not a difficulty because there is a one-to-one correspondence between a request and a subsequent signal generation. If the request cannot allocate the signal delivery resources, it can fail the call with an [EAGAIN] error.

Periodic timers are a special case. A single request can generate an unspecified number of signals. This is not a problem if the requesting process can service the signals as fast as they are generated, thus making the signal delivery resources available for delivery of subsequent periodic timer expiration signals. But, in general, this cannot be assured—processing of periodic timer signals may "overrun"; that is, subsequent periodic timer expirations may occur before the currently pending signal has been delivered.

Also, for signals, according to the POSIX.1-1990 standard, if subsequent occurrences of a pending signal are generated, it is implementation-defined whether a signal is delivered for each occurrence. This is not adequate for some realtime applications. So a mechanism is required to allow applications to detect how many timer expirations were delayed without requiring an indefinite amount of system resources to store the delayed expirations.

The specified facilities provide for an overrun count. The overrun count is defined as the number of extra timer expirations that occurred between the time a timer expiration signal is generated and the time the signal is delivered. The signal-catching function, if it is concerned with overruns, can retrieve this count on entry. With this method, a periodic timer only needs one "signal queuing resource" that can be allocated at the time of the `timer_create()` function call.

A function is defined to retrieve the overrun count so that an application need not allocate static storage to contain the count, and an implementation need not update this storage asynchronously on timer expirations. But, for some high-frequency periodic applications, the overhead of an additional system call on each timer expiration may be prohibitive. The functions, as defined, permit an implementation to maintain the overrun count in user space, associated with the `timerid`. The `timer_getoverrun()` function can then be implemented as a macro that uses the `timerid` argument (which may just be a pointer to a user space structure containing the counter) to locate the overrun count with no system call overhead. Other implementations, less concerned with this class of applications, can avoid the asynchronous update of user space by maintaining the count in a system structure at the cost of the extra system call to obtain it.

### Timer Expiration Signal Parameters

The realtime signals functionality supports an application-specific datum that is delivered to the extended signal handler. This value is explicitly specified by the application, along with the signal number to be delivered, in a `sigevent` structure. The type of the application-defined value can be either an integer constant or a pointer. This explicit specification of the value, as opposed to always sending the timer ID, was selected based on existing practice.

It is common practice for realtime applications (on non-POSIX systems or realtime extended POSIX systems) to use the parameters of event handlers as the case label of a switch statement or as a pointer to an application-defined data structure. Since timer_ids are dynamically allocated by the `timer_create()` function, they can be used for neither of these functions without additional application overhead in the signal handler; for example, to search an array of saved timer IDs to associate the ID with a constant or application data structure.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `clock_getres()`
- `timer_delete()`
- `timer_getoverrun()`
- `<signal.h>`
- `<time.h>`

## CHANGE HISTORY

First released in Issue 5. Included for alignment with the POSIX Realtime Extension.

### Issue 6

The `timer_create()` function is marked as part of the Timers option.

The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Timers option.

CPU-time clocks are added for alignment with IEEE Std 1003.1d-1999.

The DESCRIPTION is updated for alignment with IEEE Std 1003.1j-2000 by adding the requirement for the CLOCK_MONOTONIC clock under the Monotonic Clock option.

The `restrict` keyword is added to the `timer_create()` prototype for alignment with the ISO/IEC 9899:1999 standard.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/138 is applied, updating the DESCRIPTION and APPLICATION USAGE sections to describe the case when a timer is created with the notification method set to SIGEV_THREAD.

### Issue 7

The `timer_create()` function is moved from the Timers option to the Base.

### Issue 8

Austin Group Defect 1116 is applied, removing a reference to the Realtime Signals Extension option that existed in earlier versions of this standard.

Austin Group Defect 1346 is applied, requiring support for Monotonic Clock.

---

*UNIX® is a registered Trademark of The Open Group.*
*POSIX™ is a Trademark of The IEEE.*
*Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved*