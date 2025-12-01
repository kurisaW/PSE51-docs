# pthread_attr_getstack, pthread_attr_setstack — get and set stack attributes

## SYNOPSIS

```c
[TSA TSS]
#include <pthread.h>

int pthread_attr_getstack(const pthread_attr_t *restrict attr,
                         void **restrict stackaddr,
                         size_t *restrict stacksize);

int pthread_attr_setstack(pthread_attr_t *attr,
                         void *stackaddr,
                         size_t stacksize);
```

## DESCRIPTION

The `pthread_attr_getstack()` and `pthread_attr_setstack()` functions, respectively, shall get and set the thread creation stack attributes `stackaddr` and `stacksize` in the `attr` object.

The stack attributes specify the area of storage to be used for the created thread's stack. The base (lowest addressable byte) of the storage shall be `stackaddr`, and the size of the storage shall be `stacksize` bytes. The `stacksize` shall be at least {PTHREAD_STACK_MIN}. The `pthread_attr_setstack()` function may fail with [EINVAL] if `stackaddr` does not meet implementation-defined alignment requirements. All pages within the stack described by `stackaddr` and `stacksize` shall be both readable and writable by the thread.

If the `pthread_attr_getstack()` function is called before the `stackaddr` attribute has been set, the behavior is unspecified.

The behavior is undefined if the value specified by the `attr` argument to `pthread_attr_getstack()` or `pthread_attr_setstack()` does not refer to an initialized thread attributes object.

## RETURN VALUE

Upon successful completion, these functions shall return a value of 0; otherwise, an error number shall be returned to indicate the error.

The `pthread_attr_getstack()` function shall store the stack attribute values in `stackaddr` and `stacksize` if successful.

## ERRORS

The `pthread_attr_setstack()` function shall fail if:

- **[EINVAL]**
  The value of `stacksize` is less than {PTHREAD_STACK_MIN} or exceeds an implementation-defined limit.

The `pthread_attr_setstack()` function may fail if:

- **[EINVAL]**
  The value of `stackaddr` does not have proper alignment to be used as a stack, or ((char *)`stackaddr` + `stacksize`) lacks proper alignment.

- **[EACCES]**
  The stack page(s) described by `stackaddr` and `stacksize` are not both readable and writable by the thread.

These functions shall not return an error code of [EINTR].

## EXAMPLES

None.

## APPLICATION USAGE

These functions are appropriate for use by applications in an environment where the stack for a thread must be placed in some particular region of memory.

While it might seem that an application could detect stack overflow by providing a protected page outside the specified stack region, this cannot be done portably. Implementations are free to place the thread's initial stack pointer anywhere within the specified region to accommodate the machine's stack pointer behavior and allocation requirements. Furthermore, on some architectures, such as the IA-64, "overflow" might mean that two separate stack pointers allocated within the region will overlap somewhere in the middle of the region.

After a successful call to `pthread_attr_setstack()`, the storage area specified by the `stackaddr` parameter is under the control of the implementation, as described in [2.9.8 Use of Application-Managed Thread Stacks](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_08).

The specification of the `stackaddr` attribute presents several ambiguities that make portable use of these functions impossible. For example, the standard allows implementations to impose arbitrary alignment requirements on `stackaddr`. Applications cannot assume that a buffer obtained from `malloc()` is suitably aligned. Note that although the `stacksize` value passed to `pthread_attr_setstack()` must satisfy alignment requirements, the same is not true for `pthread_attr_setstacksize()` where the implementation must increase the specified size if necessary to achieve the proper alignment.

## RATIONALE

If an implementation detects that the value specified by the `attr` argument to `pthread_attr_getstack()` or `pthread_attr_setstack()` does not refer to an initialized thread attributes object, it is recommended that the function should fail and report an [EINVAL] error.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`pthread_attr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_destroy.html)
- [`pthread_attr_getdetachstate()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getdetachstate.html)
- [`pthread_attr_getstacksize()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getstacksize.html)
- [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html)
- XBD [`<limits.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/limits.h.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## CHANGE HISTORY

First released in Issue 6. Developed as part of the XSI option and brought into the BASE by IEEE PASC Interpretation 1003.1 #101.

### IEEE Std 1003.1-2001/Cor 2-2004
- Item XSH/TC2/D6/83 is applied, updating the APPLICATION USAGE section to refer to [2.9.8 Use of Application-Managed Thread Stacks](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_08).
- Item XSH/TC/D6/84 is applied, updating the ERRORS section to include optional errors for the case when `attr` refers to an uninitialized thread attribute object.

### Issue 7
- SD5-XSH-ERN-66 is applied, correcting the use of `attr` in the [EINVAL] error condition.
- Austin Group Interpretation 1003.1-2001 #057 is applied, clarifying the behavior if the function is called before the `stackaddr` attribute is set.
- SD5-XSH-ERN-157 is applied, updating the APPLICATION USAGE section.
- The description of the `stackaddr` attribute is updated in the DESCRIPTION and APPLICATION USAGE sections.
- The [EINVAL] error for an uninitialized thread attributes object is removed; this condition results in undefined behavior.

---

*Copyright © 2001-2024 The IEEE and The Open Group. All Rights Reserved.*