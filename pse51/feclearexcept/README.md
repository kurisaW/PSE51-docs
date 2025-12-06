# feclearexcept — clear floating-point exception

## SYNOPSIS

```c
#include <fenv.h>

int feclearexcept(int excepts);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `feclearexcept()` function shall attempt to clear the supported floating-point exceptions represented by `excepts`.

## RETURN VALUE

If the argument is zero or if all the specified exceptions were successfully cleared, `feclearexcept()` shall return zero. Otherwise, it shall return a non-zero value.

## ERRORS

No errors are defined.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`fegetexceptflag()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fegetexceptflag.html)
- [`feraiseexcept()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/feraiseexcept.html)
- [`fetestexcept()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fetestexcept.html)

XBD [`<fenv.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fenv.h.html)

## CHANGE HISTORY

First released in Issue 6. Derived from the ISO/IEC 9899:1999 standard.

ISO/IEC 9899:1999 standard, Technical Corrigendum 1 is incorporated.

