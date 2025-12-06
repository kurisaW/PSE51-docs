# fetestexcept — test floating-point exception flags

## SYNOPSIS

```c
#include <fenv.h>

int fetestexcept(int excepts);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `fetestexcept()` function shall determine which of a specified subset of the floating-point exception flags are currently set. The `excepts` argument specifies the floating-point status flags to be queried.

## RETURN VALUE

The `fetestexcept()` function shall return the value of the bitwise-inclusive OR of the floating-point exception macros corresponding to the currently set floating-point exceptions included in `excepts`.

## ERRORS

No errors are defined.

---

*The following sections are informative.*

## EXAMPLES

The following example calls function `f()` if an invalid exception is set, and then function `g()` if an overflow exception is set:

```c
#include <fenv.h>
/* ... */
{
    #pragma STDC FENV_ACCESS ON
    int set_excepts;
    feclearexcept(FE_INVALID | FE_OVERFLOW);
    // maybe raise exceptions
    set_excepts = fetestexcept(FE_INVALID | FE_OVERFLOW);
    if (set_excepts & FE_INVALID) f();
    if (set_excepts & FE_OVERFLOW) g();
    /* ... */
}
```

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`feclearexcept()`](feclearexcept.md)
- [`fegetexceptflag()`](fegetexceptflag.md)
- [`feraiseexcept()`](feraiseexcept.md)

XBD [`<fenv.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fenv.h.html)

## CHANGE HISTORY

First released in Issue 6. Derived from the ISO/IEC 9899:1999 standard.

