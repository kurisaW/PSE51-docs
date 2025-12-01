# pthread_create - thread creation

## SYNOPSIS

```c
#include <pthread.h>

int pthread_create(pthread_t *restrict thread,
                  const pthread_attr_t *restrict attr,
                  void *(*start_routine)(void*),
                  void *restrict arg);
```

## DESCRIPTION

The `pthread_create()` function shall create a new thread, with attributes specified by `attr`, within a process. If `attr` is NULL, the default attributes shall be used. If the attributes specified by `attr` are modified later, the thread's attributes shall not be affected. Upon successful completion, `pthread_create()` shall store the ID of the created thread in the location referenced by `thread`.

The thread is created executing `start_routine` with `arg` as its sole argument. If the `start_routine` returns, the effect shall be as if there was an implicit call to `pthread_exit()` using the return value of `start_routine` as the exit status. Note that the thread in which `main()` was originally invoked differs from this. When it returns from `main()`, the effect shall be as if there was an implicit call to `exit()` using the return value of `main()` as the exit status.

### Signal State Initialization

The signal state of the new thread shall be initialized as follows:

* The signal mask shall be inherited from the creating thread.
* The set of signals pending for the new thread shall be empty.

The thread-local current locale [XSI] and the alternate stack shall not be inherited.

The floating-point environment shall be inherited from the creating thread.

If `pthread_create()` fails, no new thread is created and the contents of the location referenced by `thread` are undefined.

[TCT] If `_POSIX_THREAD_CPUTIME` is defined, the new thread shall have a CPU-time clock accessible, and the initial value of this clock shall be set to zero.

The behavior is undefined if the value specified by the `attr` argument to `pthread_create()` does not refer to an initialized thread attributes object.

## RETURN VALUE

If successful, the `pthread_create()` function shall return zero; otherwise, an error number shall be returned to indicate the error.

## ERRORS

The `pthread_create()` function shall fail if:

**[EAGAIN]**
The system lacked the necessary resources to create another thread, or the system-imposed limit on the total number of threads in a process `{PTHREAD_THREADS_MAX}` would be exceeded.

**[EPERM]**
The caller does not have appropriate privileges to set the required scheduling parameters or scheduling policy.

The `pthread_create()` function shall not return an error code of `[EINTR]`.

## EXAMPLES

None.

## APPLICATION USAGE

There is no requirement on the implementation that the ID of the created thread be available before the newly created thread starts executing. The calling thread can obtain the ID of the created thread through the `thread` argument of the `pthread_create()` function, and the newly created thread can obtain its ID by a call to `pthread_self()`.

## RATIONALE

A suggested alternative to `pthread_create()` would be to define two separate operations: create and start. Some applications would find such behavior more natural. Ada, in particular, separates the "creation" of a task from its "activation".

Splitting the operation was rejected by the standard developers for many reasons:

* The number of calls required to start a thread would increase from one to two and thus place an additional burden on applications that do not require the additional synchronization. The second call, however, could be avoided by the additional complication of a start-up state attribute.
* An extra state would be introduced: "created but not started". This would require the standard to specify the behavior of the thread operations when the target has not yet started executing.
* For those applications that require such behavior, it is possible to simulate the two separate steps with the facilities that are currently provided. The `start_routine()` can synchronize by waiting on a condition variable that is signaled by the start operation.

An Ada implementor can choose to create the thread at either of two points in the Ada program: when the task object is created, or when the task is activated (generally at a "begin"). If the first approach is adopted, the `start_routine()` needs to wait on a condition variable to receive the order to begin "activation". The second approach requires no such condition variable or extra synchronization. In either approach, a separate Ada task control block would need to be created when the task object is created to hold rendezvous queues, and so on.

An extension of the preceding model would be to allow the state of the thread to be modified between the create and start. This would allow the thread attributes object to be eliminated. This has been rejected because:

* All state in the thread attributes object has to be able to be set for the thread. This would require the definition of functions to modify thread attributes. There would be no reduction in the number of function calls required to set up the thread. In fact, for an application that creates all threads using identical attributes, the number of function calls required to set up the threads would be dramatically increased. Use of a thread attributes object permits the application to make one set of attribute setting function calls. Otherwise, the set of attribute setting function calls needs to be made for each thread creation.
* Depending on the implementation architecture, functions to set thread state would require kernel calls, or for other implementation reasons would not be able to be implemented as macros, thereby increasing the cost of thread creation.
* The ability for applications to segregate threads by class would be lost.

Another suggested alternative uses a model similar to that for process creation, such as "thread fork". The fork semantics would provide more flexibility and the "create" function can be implemented simply by doing a thread fork followed immediately by a call to the desired "start routine" for the thread. This alternative has these problems:

* For many implementations, the entire stack of the calling thread would need to be duplicated, since in many architectures there is no way to determine the size of the calling frame.
* Efficiency is reduced since at least some part of the stack has to be copied, even though in most cases the thread never needs the copied context, since it merely calls the desired start routine.

If an implementation detects that the value specified by the `attr` argument to `pthread_create()` does not refer to an initialized thread attributes object, it is recommended that the function should fail and report an `[EINVAL]` error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

* `fork()`
* `pthread_exit()`
* `pthread_join()`

XBD 4.15.2 Memory Synchronization, `<pthread.h>`

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Threads Extension.

### Issue 6
The `pthread_create()` function is marked as part of the Threads option.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

* The `[EPERM]` mandatory error condition is added.

The thread CPU-time clock semantics are added for alignment with IEEE Std 1003.1d-1999.

The `restrict` keyword is added to the `pthread_create()` prototype for alignment with the ISO/IEC 9899:1999 standard.

The DESCRIPTION is updated to make it explicit that the floating-point environment is inherited from the creating thread.

IEEE Std 1003.1-2001/Cor 1-2002, item XSH/TC1/D6/44 is applied, adding text that the alternate stack is not inherited.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/93 is applied, updating the ERRORS section to remove the mandatory `[EINVAL]` error ("The value specified by `attr` is invalid"), and adding the optional `[EINVAL]` error ("The attributes specified by `attr` are invalid").

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/94 is applied, adding the APPLICATION USAGE section.

### Issue 7
The `pthread_create()` function is moved from the Threads option to the Base.

The `[EINVAL]` error for an uninitialized thread attributes object is removed; this condition results in undefined behavior.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0458 [302] is applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0274 [849] is applied.

---

**Source:** The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024
**URL:** https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html