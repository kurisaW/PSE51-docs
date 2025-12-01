# fegetexceptflag, fesetexceptflag — get and set floating-point status flags

## SYNOPSIS

```c
#include <fenv.h>

int fegetexceptflag(fexcept_t *flagp, int excepts);
int fesetexceptflag(const fexcept_t *flagp, int excepts);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `fegetexceptflag()` function shall attempt to store an implementation-defined representation of the states of the floating-point status flags indicated by the argument `excepts` in the object pointed to by the argument `flagp`.

The `fesetexceptflag()` function shall attempt to set the floating-point status flags indicated by the argument `excepts` to the states stored in the object pointed to by `flagp`. The value pointed to by `flagp` shall have been set by a previous call to `fegetexceptflag()` whose second argument represented at least those floating-point exceptions represented by the argument `excepts`. This function does not raise floating-point exceptions, but only sets the state of the flags.

## RETURN VALUE

If the representation was successfully stored, `fegetexceptflag()` shall return zero. Otherwise, it shall return a non-zero value. If the `excepts` argument is zero or if all the specified exceptions were successfully set, `fesetexceptflag()` shall return zero. Otherwise, it shall return a non-zero value.

## ERRORS

No errors are defined.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`feclearexcept()`](feclearexcept.md)
- [`feraiseexcept()`](feraiseexcept.md)
- [`fetestexcept()`](fetestexcept.md)
- XBD [`<fenv.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fenv.h.html)

## CHANGE HISTORY

First released in Issue 6. Derived from the ISO/IEC 9899:1999 standard.

ISO/IEC 9899:1999 standard, Technical Corrigendum 1 is incorporated.