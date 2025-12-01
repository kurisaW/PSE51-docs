# fegetenv, fesetenv — get and set current floating-point environment

## SYNOPSIS

```c
#include <fenv.h>

int fegetenv(fenv_t *envp);
int fesetenv(const fenv_t *envp);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `fegetenv()` function shall attempt to store the current floating-point environment in the object pointed to by `envp`.

The `fesetenv()` function shall attempt to establish the floating-point environment represented by the object pointed to by `envp`. The argument `envp` shall point to an object set by a call to `fegetenv()` or `feholdexcept()`, or equal a floating-point environment macro. The `fesetenv()` function does not raise floating-point exceptions, but only installs the state of the floating-point status flags represented through its argument.

## RETURN VALUE

If the representation was successfully stored, `fegetenv()` shall return zero. Otherwise, it shall return a non-zero value. If the environment was successfully established, `fesetenv()` shall return zero. Otherwise, it shall return a non-zero value.

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

- `feholdexcept()`
- `feupdateenv()`
- `<fenv.h>`

## CHANGE HISTORY

First released in Issue 6. Derived from the ISO/IEC 9899:1999 standard.

ISO/IEC 9899:1999 standard, Technical Corrigendum 1 is incorporated.

---

*Source: The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*