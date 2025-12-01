# pthread_cond_destroy, pthread_cond_init — destroy and initialize condition variables

## SYNOPSIS

```c
#include <pthread.h>

int pthread_cond_destroy(pthread_cond_t *cond);

int pthread_cond_init(pthread_cond_t *restrict cond,
                     const pthread_condattr_t *restrict attr);

pthread_cond_t cond = PTHREAD_COND_INITIALIZER;
```

## DESCRIPTION

The `pthread_cond_destroy()` function shall destroy the given condition variable specified by `cond`; the object becomes, in effect, uninitialized. An implementation may cause `pthread_cond_destroy()` to set the object referenced by `cond` to an invalid value. A destroyed condition variable object can be reinitialized using `pthread_cond_init()`; the results of otherwise referencing the object after it has been destroyed are undefined.

It shall be safe to destroy an initialized condition variable upon which no threads are currently blocked. Attempting to destroy a condition variable upon which other threads are currently blocked results in undefined behavior.

The `pthread_cond_init()` function shall initialize the condition variable referenced by `cond` with attributes referenced by `attr`. If `attr` is NULL, the default condition variable attributes shall be used; the effect is the same as passing the address of a default condition variable attributes object. Upon successful initialization, the state of the condition variable shall become initialized.

See [2.9.9 Synchronization Object Copies and Alternative Mappings](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_09) for further requirements.

Attempting to initialize an already initialized condition variable results in undefined behavior.

In cases where default condition variable attributes are appropriate, the macro `PTHREAD_COND_INITIALIZER` can be used to initialize condition variables. The effect shall be equivalent to dynamic initialization by a call to `pthread_cond_init()` with parameter `attr` specified as NULL, except that no error checks are performed.

The behavior is undefined if the value specified by the `cond` argument to `pthread_cond_destroy()` does not refer to an initialized condition variable.

The behavior is undefined if the value specified by the `attr` argument to `pthread_cond_init()` does not refer to an initialized condition variable attributes object.

## RETURN VALUE

If successful, the `pthread_cond_destroy()` and `pthread_cond_init()` functions shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_cond_init()` function shall fail if:

- **[EAGAIN]**
  The system lacked the necessary resources (other than memory) to initialize another condition variable.

- **[ENOMEM]**
  Insufficient memory exists to initialize the condition variable.

These functions shall not return an error code of **[EINTR]**.

## EXAMPLES

A condition variable can be destroyed immediately after all the threads that are blocked on it are awakened. For example, consider the following code:

```c
struct list {
    pthread_mutex_t lm;
    ...
};

struct elt {
    key k;
    int busy;
    pthread_cond_t notbusy;
    ...
};

/* Find a list element and reserve it. */
struct elt *
list_find(struct list *lp, key k)
{
    struct elt *ep;

    pthread_mutex_lock(&lp->lm);
    while ((ep = find_elt(l, k) != NULL) && ep->busy)
        pthread_cond_wait(&ep->notbusy, &lp->lm);
    if (ep != NULL)
        ep->busy = 1;
    pthread_mutex_unlock(&lp->lm);
    return(ep);
}

delete_elt(struct list *lp, struct elt *ep)
{
    pthread_mutex_lock(&lp->lm);
    assert(ep->busy);
    ... remove ep from list ...
    ep->busy = 0;  /* Paranoid. */
(A) pthread_cond_broadcast(&ep->notbusy);
    pthread_mutex_unlock(&lp->lm);
(B) pthread_cond_destroy(&ep->notbusy);
    free(ep);
}
```

In this example, the condition variable and its list element may be freed (line B) immediately after all threads waiting for it are awakened (line A), since the mutex and the code ensure that no other thread can touch the element to be deleted.

## APPLICATION USAGE

None.

## RATIONALE

If an implementation detects that the value specified by the `cond` argument to `pthread_cond_destroy()` does not refer to an initialized condition variable, it is recommended that the function should fail and report an **[EINVAL]** error.

If an implementation detects that the value specified by the `cond` argument to `pthread_cond_destroy()` or `pthread_cond_init()` refers to a condition variable that is in use (for example, in a `pthread_cond_wait()` call) by another thread, or detects that the value specified by the `cond` argument to `pthread_cond_init()` refers to an already initialized condition variable, it is recommended that the function should fail and report an **[EBUSY]** error.

If an implementation detects that the value specified by the `attr` argument to `pthread_cond_init()` does not refer to an initialized condition variable attributes object, it is recommended that the function should fail and report an **[EINVAL]** error.

See also `pthread_mutex_destroy()`.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`pthread_cond_broadcast()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_broadcast.html)
- [`pthread_cond_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cond_clockwait.html)
- [`pthread_mutex_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_destroy.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Threads Extension.

### Issue 6
- The `pthread_cond_destroy()` and `pthread_cond_init()` functions are marked as part of the Threads option.
- IEEE PASC Interpretation 1003.1c #34 is applied, updating the DESCRIPTION.
- The **restrict** keyword is added to the `pthread_cond_init()` prototype for alignment with the ISO/IEC 9899:1999 standard.

### Issue 7
- The `pthread_cond_destroy()` and `pthread_cond_init()` functions are moved from the Threads option to the Base.
- The **[EINVAL]** error for an uninitialized condition variable and an uninitialized condition variable attributes object is removed; this condition results in undefined behavior.
- The **[EBUSY]** error for a condition variable already in use or an already initialized condition variable is removed; this condition results in undefined behavior.
- POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0455 [70] is applied.
- POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0269 [972] and XSH/TC2-2008/0270 [910] are applied.