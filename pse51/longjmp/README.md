# longjmp — non-local goto

## SYNOPSIS

```c
#include <setjmp.h>

_Noreturn void longjmp(jmp_buf env, int val);
```

## DESCRIPTION

The `longjmp()` function shall restore the environment saved by the most recent invocation of `setjmp()` in the same process, with the corresponding `jmp_buf` argument. If the most recent invocation of `setjmp()` with the corresponding `jmp_buf` occurred in another thread, or if there is no such invocation, or if the function containing the invocation of `setjmp()` has terminated execution in the interim, or if the invocation of `setjmp()` was within the scope of an identifier with variably modified type and execution has left that scope in the interim, the behavior is undefined. It is unspecified whether `longjmp()` restores the signal mask, leaves the signal mask unchanged, or restores it to its value at the time `setjmp()` was called.

All accessible objects have values, and all other components of the abstract machine have state (for example, floating-point status flags and open files), as of the time `longjmp()` was called, except that the values of objects of automatic storage duration are unspecified if they meet all the following conditions:

- They are local to the function containing the corresponding `setjmp()` invocation.
- They do not have volatile-qualified type.
- They are changed between the `setjmp()` invocation and `longjmp()` call.

Although `longjmp()` is an async-signal-safe function, if it is invoked from a signal handler which interrupted a non-async-signal-safe function or equivalent (such as the processing equivalent to `exit()` performed after a return from the initial call to `main()`), the behavior of any subsequent call to a non-async-signal-safe function or equivalent is undefined.

The effect of a call to `longjmp()` where initialization of the `jmp_buf` structure was not performed in the calling thread is undefined.

## RETURN VALUE

After `longjmp()` is completed, thread execution shall continue as if the corresponding invocation of `setjmp()` had just returned the value specified by `val`. The `longjmp()` function shall not cause `setjmp()` to return 0; if `val` is 0, `setjmp()` shall return 1.

## ERRORS

No errors are defined.

## APPLICATION USAGE

Applications whose behavior depends on the value of the signal mask should not use `longjmp()` and `setjmp()`, since their effect on the signal mask is unspecified, but should instead use the `siglongjmp()` and `sigsetjmp()` functions (which can save and restore the signal mask under application control).

It is recommended that applications do not call `longjmp()` or `siglongjmp()` from signal handlers. To avoid undefined behavior when calling these functions from a signal handler, the application needs to ensure one of the following two things:

1. After the call to `longjmp()` or `siglongjmp()` the process only calls async-signal-safe functions and does not return from the initial call to `main()`.
2. Any signal whose handler calls `longjmp()` or `siglongjmp()` is blocked during *every* call to a non-async-signal-safe function, and no such calls are made after returning from the initial call to `main()`.

## SEE ALSO

- `setjmp()`
- `sigaction()`
- `siglongjmp()`
- `sigsetjmp()`
- `<setjmp.h>`

## CHANGE HISTORY

### First released in Issue 1
Derived from Issue 1 of the SVID.

### Issue 5
The DESCRIPTION is updated for alignment with the POSIX Threads Extension.

### Issue 6
Extensions beyond the ISO C standard are marked.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

- The DESCRIPTION now explicitly makes `longjmp()`'s effect on the signal mask unspecified.

The DESCRIPTION is updated for alignment with the ISO/IEC 9899:1999 standard.

### Issue 7
POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0365 [394] is applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0202 [516] is applied.

### Issue 8
Austin Group Defect 1302 is applied, aligning this function with the ISO/IEC 9899:2018 standard.

---
