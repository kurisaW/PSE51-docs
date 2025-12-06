# pthread_mutexattr_getprotocol

## Synopsis

```c
#include <pthread.h>

int pthread_mutexattr_getprotocol(const pthread_mutexattr_t *restrict attr,
                                  int *restrict protocol);

int pthread_mutexattr_setprotocol(pthread_mutexattr_t *attr,
                                  int protocol);
```

## Description

The `pthread_mutexattr_getprotocol()` and `pthread_mutexattr_setprotocol()` functions, respectively, shall get and set the protocol attribute of a mutex attributes object pointed to by `attr` which was previously created by the function `pthread_mutexattr_init()`.

The `protocol` attribute defines the protocol to be followed in utilizing mutexes. The value of `protocol` may be one of:

- **PTHREAD_PRIO_INHERIT**
- **PTHREAD_PRIO_NONE**
- **PTHREAD_PRIO_PROTECT**

which are defined in the `<pthread.h>` header. The default value of the attribute shall be PTHREAD_PRIO_NONE.

### Protocol Behavior

When a thread owns a mutex with the PTHREAD_PRIO_NONE protocol attribute, its priority and scheduling shall not be affected by its mutex ownership.

#### PTHREAD_PRIO_INHERIT

When a thread is blocking higher priority threads because of owning one or more mutexes with the PTHREAD_PRIO_INHERIT protocol attribute, it shall execute at the higher of its priority or the priority of the highest priority thread waiting on any of the mutexes owned by this thread and initialized with this protocol.

When a thread makes a call to `pthread_mutex_lock()`, the mutex was initialized with the protocol attribute having the value PTHREAD_PRIO_INHERIT, when the calling thread is blocked because the mutex is owned by another thread, that owner thread shall inherit the priority level of the calling thread as long as it continues to own the mutex. The implementation shall update its execution priority to the maximum of its assigned priority and all its inherited priorities. Furthermore, if this owner thread itself becomes blocked on another mutex with the protocol attribute having the value PTHREAD_PRIO_INHERIT, the same priority inheritance effect shall be propagated to this other owner thread, in a recursive manner.

#### PTHREAD_PRIO_PROTECT

When a thread owns one or more mutexes initialized with the PTHREAD_PRIO_PROTECT protocol, it shall execute at the higher of its priority or the highest of the priority ceilings of all the mutexes owned by this thread and initialized with this attribute, regardless of whether other threads are blocked on any of these mutexes or not.

### Multiple Mutexes

If a thread simultaneously owns several mutexes initialized with different protocols, it shall execute at the highest of the priorities that it would have obtained by each of these protocols.

## Return Value

Upon successful completion, the `pthread_mutexattr_getprotocol()` and `pthread_mutexattr_setprotocol()` functions shall return zero; otherwise, an error number shall be returned to indicate the error.

## Errors

The `pthread_mutexattr_setprotocol()` function shall fail if:

- **ENOTSUP** - The value specified by `protocol` is an unsupported value.

The `pthread_mutexattr_getprotocol()` and `pthread_mutexattr_setprotocol()` functions may fail if:

- **EINVAL** - The value specified by `attr` or `protocol` is invalid.

## Examples

No examples are provided in the specification.

## Application Usage

The `pthread_mutexattr_getprotocol()` and `pthread_mutexattr_setprotocol()` functions are part of the Threads option and either the Thread Priority Protection or Thread Priority Inheritance options.

These functions require support of either the Non-Robust Mutex Priority Protection option or the Non-Robust Mutex Priority Inheritance option or the Robust Mutex Priority Protection option or the Robust Mutex Priority Inheritance option.

## Rationale

The protocol attribute allows applications to control the priority behavior of mutexes to prevent priority inversion scenarios where lower-priority threads hold mutexes needed by higher-priority threads.

### Default Value

The default value of the protocol attribute is PTHREAD_PRIO_NONE, which means that mutex ownership does not affect thread priority or scheduling.

### Priority Inheritance

Priority inheritance (PTHREAD_PRIO_INHERIT) temporarily raises the priority of a thread that holds a mutex when higher-priority threads are waiting for that mutex. This helps prevent unbounded priority inversion.

### Priority Protection

Priority protection (PTHREAD_PRIO_PROTECT) sets a priority ceiling for each mutex. A thread that owns such a mutex executes at the higher of its own priority or the highest priority ceiling of all mutexes it owns that were initialized with this protocol.

## Future Directions

None.

## See Also

- `pthread_mutexattr_init()`
- `pthread_mutex_lock()`
- `<pthread.h>`

## Copyright

Portions of this text are reprinted and reproduced in electronic form from IEEE Std 1003.1-2017, The Open Group Base Specifications Issue 7, Copyright (C) 2017 by the Institute of Electrical and Electronics Engineers, Inc and The Open Group. In the event of any discrepancy between this version and the original IEEE and The Open Group Standard, the original IEEE and The Open Group Standard shall prevail. The original Standard can be obtained online at http://www.opengroup.org/unix/online.html.

Any typographical or formatting errors that appear in this page are most likely to have been introduced during the conversion of the source files to man page format. To report such errors, see https://www.kernel.org/doc/man-pages/reporting_bugs.html.