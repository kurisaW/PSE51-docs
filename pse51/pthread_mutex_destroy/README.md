# pthread_mutex_destroy

## Name

pthread_mutex_destroy, pthread_mutex_init - destroy and initialize a mutex

## Synopsis

```c
#include <pthread.h>

int pthread_mutex_destroy(pthread_mutex_t *mutex);
int pthread_mutex_init(pthread_mutex_t *restrict mutex,
                      const pthread_mutexattr_t *restrict attr);

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
```

## Description

The `pthread_mutex_destroy()` function shall destroy the mutex object referenced by `mutex`; the mutex object becomes, in effect, uninitialized. An implementation may cause `pthread_mutex_destroy()` to set the object referenced by `mutex` to an invalid value.

A destroyed mutex object can be reinitialized using `pthread_mutex_init()`; the results of otherwise referencing the object after it has been destroyed are undefined.

It shall be safe to destroy an initialized mutex that is unlocked. Attempting to destroy a locked mutex, or a mutex that another thread is attempting to lock, or a mutex that is being used in a `pthread_cond_clockwait()`, `pthread_cond_timedwait()`, or `pthread_cond_wait()` call by another thread, results in undefined behavior.

The `pthread_mutex_init()` function shall initialize the mutex referenced by `mutex` with attributes specified by `attr`. If `attr` is NULL, the default mutex attributes are used; the effect shall be the same as passing the address of a default mutex attributes object. Upon successful initialization, the state of the mutex becomes initialized and unlocked.

See [2.9.9 Synchronization Object Copies and Alternative Mappings](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_09) for further requirements.

Attempting to initialize an already initialized mutex results in undefined behavior.

In cases where default mutex attributes are appropriate, the macro `PTHREAD_MUTEX_INITIALIZER` can be used to initialize mutexes. The effect shall be equivalent to dynamic initialization by a call to `pthread_mutex_init()` with parameter `attr` specified as NULL, except that no error checks are performed.

The behavior is undefined if the value specified by the `mutex` argument to `pthread_mutex_destroy()` does not refer to an initialized mutex.

The behavior is undefined if the value specified by the `attr` argument to `pthread_mutex_init()` does not refer to an initialized mutex attributes object.

## Return Value

If successful, the `pthread_mutex_destroy()` and `pthread_mutex_init()` functions shall return zero; otherwise, an error number shall be returned to indicate the error.

## Errors

The `pthread_mutex_destroy()` function may fail if:

- **EBUSY** - The implementation has detected an attempt to destroy the object referenced by `mutex` while it is locked or referenced (for example, while being used in a `pthread_cond_clockwait()`, `pthread_cond_timedwait()`, or `pthread_cond_wait()` call) by another thread.
- **EINVAL** - The value specified by `mutex` does not refer to an initialized mutex object.

The `pthread_mutex_init()` function may fail if:

- **EBUSY** - The implementation has detected an attempt to reinitialize the object referenced by `mutex`, a previously initialized, but not yet destroyed, mutex.
- **EINVAL** - The value specified by `attr` does not refer to an initialized mutex attributes object, or the value specified by `mutex` does not refer to an initialized mutex.
- **ENOMEM** - Insufficient memory exists to initialize the mutex.

These functions shall not return an error code of [EINTR].

## Examples

### Static Initialization

```c
static pthread_mutex_t foo_mutex = PTHREAD_MUTEX_INITIALIZER;

void foo()
{
    pthread_mutex_lock(&foo_mutex);
    /* Do work. */
    pthread_mutex_unlock(&foo_mutex);
}
```

### Dynamic Initialization

```c
static pthread_once_t foo_once = PTHREAD_ONCE_INIT;
static pthread_mutex_t foo_mutex;

void foo_init()
{
    pthread_mutex_init(&foo_mutex, NULL);
}

void foo()
{
    pthread_once(&foo_once, foo_init);
    pthread_mutex_lock(&foo_mutex);
    /* Do work. */
    pthread_mutex_unlock(&foo_mutex);
}
```

## Application Usage

The behavior is undefined if the value specified by the `mutex` argument to `pthread_mutex_destroy()` does not refer to an initialized mutex.

The behavior is undefined if the value specified by the `attr` argument to `pthread_mutex_init()` does not refer to an initialized mutex attributes object.

## Rationale

### Alternate Implementations Possible

This volume of POSIX.1-2024 supports several alternative implementations of mutexes. An implementation may store the lock directly in the object of type `pthread_mutex_t`. Alternatively, an implementation may store the lock in the heap and merely store a pointer, handle, or unique ID in the mutex object. Either implementation has advantages or may be required on certain hardware configurations. So that portable code can be written that is invariant to this choice, this volume of POSIX.1-2024 does not define assignment or equality for this type, and it uses the term "initialize" to reinforce the (more restrictive) notion that the lock may actually reside in the mutex object itself.

Note that this precludes an over-specification of the type of the mutex or condition variable and motivates the opaqueness of the type.

An implementation is permitted, but not required, to have `pthread_mutex_destroy()` store an illegal value into the mutex. This may help detect erroneous programs that try to lock (or otherwise reference) a mutex that has already been destroyed.

### Tradeoff Between Error Checks and Performance Supported

Many error conditions that can occur are not required to be detected by the implementation in order to let implementations trade off performance versus degree of error checking according to the needs of their specific applications and execution environment. As a general rule, conditions caused by the system (such as insufficient memory) are required to be detected, but conditions caused by an erroneously coded application (such as failing to provide adequate synchronization to prevent a mutex from being deleted while in use) are specified to result in undefined behavior.

### Static Initializers for Mutexes and Condition Variables

Providing for static initialization of statically allocated synchronization objects allows modules with private static synchronization variables to avoid runtime initialization tests and overhead. Furthermore, it simplifies the coding of self-initializing modules. Such modules are common in C libraries, where for various reasons the design calls for self-initialization instead of requiring an explicit module initialization function to be called.

### Destroying Mutexes

A mutex can be destroyed immediately after it is unlocked. However, since attempting to destroy a locked mutex, or a mutex that another thread is attempting to lock, or a mutex that is being used in a `pthread_cond_clockwait()`, `pthread_cond_timedwait()`, or `pthread_cond_wait()` call by another thread, results in undefined behavior, care must be taken to ensure that no other thread may be referencing the mutex.

### Robust Mutexes

Implementations are required to provide robust mutexes for mutexes with the process-shared attribute set to `PTHREAD_PROCESS_SHARED`. Implementations are allowed, but not required, to provide robust mutexes when the process-shared attribute is set to `PTHREAD_PROCESS_PRIVATE`.

## See Also

- [`pthread_cond_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_clockwait.html)
- [`pthread_cond_timedwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_timedwait.html)
- [`pthread_cond_wait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_wait.html)
- [`pthread_mutex_lock()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_lock.html)
- [`pthread_mutex_unlock()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_unlock.html)
- [`pthread_once()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_once.html)
- [Base Definitions, `<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## Change History

Derived from the POSIX Threads Extension (1003.1c-1995).

*End of informative text.*