# pthread_attr_getschedparam, pthread_attr_setschedparam — get and set the schedparam attribute

## SYNOPSIS

```c
#include <pthread.h>

int pthread_attr_getschedparam(const pthread_attr_t *restrict attr,
                               struct sched_param *restrict param);

int pthread_attr_setschedparam(pthread_attr_t *restrict attr,
                               const struct sched_param *restrict param);
```

## DESCRIPTION

The `pthread_attr_getschedparam()` and `pthread_attr_setschedparam()` functions, respectively, shall get and set the scheduling parameter attributes in the `attr` argument. The contents of the `param` structure are defined in the `<sched.h>` header. For the SCHED_FIFO and SCHED_RR policies, the only required member of `param` is `sched_priority`.

[TSP] For the SCHED_SPORADIC policy, the required members of the `param` structure are `sched_priority`, `sched_ss_low_priority`, `sched_ss_repl_period`, `sched_ss_init_budget`, and `sched_ss_max_repl`. The specified `sched_ss_repl_period` needs to be greater than or equal to the specified `sched_ss_init_budget` for the function to succeed; if it is not, then the function shall fail. The value of `sched_ss_max_repl` shall be within the inclusive range [1,{SS_REPL_MAX}] for the function to succeed; if not, the function shall fail. It is unspecified whether the `sched_ss_repl_period` and `sched_ss_init_budget` values are stored as provided by this function or are rounded to align with the resolution of the clock being used.

The behavior is undefined if the value specified by the `attr` argument to `pthread_attr_getschedparam()` or `pthread_attr_setschedparam()` does not refer to an initialized thread attributes object.

## RETURN VALUE

If successful, the `pthread_attr_getschedparam()` and `pthread_attr_setschedparam()` functions shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_attr_setschedparam()` function shall fail if:

- **ENOTSUP** - An attempt was made to set the attribute to an unsupported value.

The `pthread_attr_setschedparam()` function may fail if:

- **EINVAL** - The value of `param` is not valid.

These functions shall not return an error code of [EINTR].

## EXAMPLES

None.

## APPLICATION USAGE

After these attributes have been set, a thread can be created with the specified attributes using `pthread_create()`. Using these routines does not affect the current running thread.

## RATIONALE

If an implementation detects that the value specified by the `attr` argument to `pthread_attr_getschedparam()` or `pthread_attr_setschedparam()` does not refer to an initialized thread attributes object, it is recommended that the function should fail and report an [EINVAL] error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `pthread_attr_destroy()`
- `pthread_attr_getscope()`
- `pthread_attr_getinheritsched()`
- `pthread_attr_getschedpolicy()`
- `pthread_create()`
- `<pthread.h>`
- `<sched.h>`

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Threads Extension.

### Issue 6
- The `pthread_attr_getschedparam()` and `pthread_attr_setschedparam()` functions are marked as part of the Threads option.
- The SCHED_SPORADIC scheduling policy is added for alignment with IEEE Std 1003.1d-1999.
- The `restrict` keyword is added to the `pthread_attr_getschedparam()` and `pthread_attr_setschedparam()` prototypes for alignment with the ISO/IEC 9899:1999 standard.
- IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/78 is applied, updating the ERRORS section to include optional errors for the case when `attr` refers to an uninitialized thread attribute object.

### Issue 7
- The `pthread_attr_getschedparam()` and `pthread_attr_setschedparam()` functions are moved from the Threads option to the Base.
- Austin Group Interpretation 1003.1-2001 #119 is applied, clarifying the accuracy requirements for the `sched_ss_repl_period` and `sched_ss_init_budget` values.
- The [EINVAL] error for an uninitialized thread attributes object is removed; this condition results in undefined behavior.
- POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0451 [314] is applied.

---
