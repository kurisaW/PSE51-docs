# feraiseexcept — raise floating-point exception

## SYNOPSIS

```c
#include <fenv.h>

int feraiseexcept(int excepts);
```

## DESCRIPTION

The `feraiseexcept()` function shall attempt to raise the supported floating-point exceptions represented by the `excepts` argument. The order in which these floating-point exceptions are raised is unspecified, except that if the `excepts` argument represents IEC 60559 valid coincident floating-point exceptions for atomic operations (namely overflow and inexact, or underflow and inexact), then overflow or underflow shall be raised before inexact. Whether the `feraiseexcept()` function additionally raises the inexact floating-point exception whenever it raises the overflow or underflow floating-point exception is implementation-defined.

## RETURN VALUE

If the argument is zero or if all the specified exceptions were successfully raised, `feraiseexcept()` shall return zero. Otherwise, it shall return a non-zero value.

## ERRORS

No errors are defined.

## EXAMPLES

None.

## APPLICATION USAGE

The effect is intended to be similar to that of floating-point exceptions raised by arithmetic operations. Hence, enabled traps for floating-point exceptions raised by this function are taken.

## RATIONALE

Raising overflow or underflow is allowed to also raise inexact because on some architectures the only practical way to raise an exception is to execute an instruction that has the exception as a side-effect. The function is not restricted to accept only valid coincident expressions for atomic operations, so the function can be used to raise exceptions accrued over several operations.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `feclearexcept()`
- `fegetexceptflag()`
- `fetestexcept()`
- `<fenv.h>`

## CHANGE HISTORY

First released in Issue 6. Derived from the ISO/IEC 9899:1999 standard.

ISO/IEC 9899:1999 standard, Technical Corrigendum 1 is incorporated.

### Issue 7

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0111 [543] is applied.

---

*Source: The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*