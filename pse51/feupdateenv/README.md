# feupdateenv — update floating-point environment

## SYNOPSIS

```c
#include <fenv.h>
int feupdateenv(const fenv_t *envp);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `feupdateenv()` function shall attempt to save the currently raised floating-point exceptions in its automatic storage, attempt to install the floating-point environment represented by the object pointed to by `envp`, and then attempt to raise the saved floating-point exceptions. The argument `envp` shall point to an object set by a call to `feholdexcept()` or `fegetenv()`, or equal a floating-point environment macro.

## RETURN VALUE

The `feupdateenv()` function shall return a zero value if and only if all the required actions were successfully carried out.

## ERRORS

No errors are defined.

*The following sections are informative.*

## EXAMPLES

The following example shows sample code to hide spurious underflow floating-point exceptions:

```c
#include <fenv.h>
double f(double x)
{
    #pragma STDC FENV_ACCESS ON
    double result;
    fenv_t save_env;
    feholdexcept(&save_env);
    // compute result
    if (/* test spurious underflow */)
    feclearexcept(FE_UNDERFLOW);
    feupdateenv(&save_env);
    return result;
}
```

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`fegetenv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fegetenv.html)
- [`feholdexcept()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/feholdexcept.html)
- XBD [`<fenv.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fenv.h.html)

## CHANGE HISTORY

First released in Issue 6. Derived from the ISO/IEC 9899:1999 standard.

ISO/IEC 9899:1999 standard, Technical Corrigendum 1 is incorporated.

*End of informative text.*