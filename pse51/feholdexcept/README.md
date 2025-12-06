# feholdexcept

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

---

## NAME

feholdexcept — save current floating-point environment

## SYNOPSIS

```c
#include <fenv.h>

int feholdexcept(fenv_t *envp);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `feholdexcept()` function shall save the current floating-point environment in the object pointed to by `envp`, clear the floating-point status flags, and then install a non-stop (continue on floating-point exceptions) mode, if available, for all floating-point exceptions.

## RETURN VALUE

The `feholdexcept()` function shall return zero if and only if non-stop floating-point exception handling was successfully installed.

## ERRORS

No errors are defined.

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

The `feholdexcept()` function should be effective on typical IEC 60559:1989 standard implementations which have the default non-stop mode and at least one other mode for trap handling or aborting. If the implementation provides only the non-stop mode, then installing the non-stop mode is trivial.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`fegetenv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fegetenv.html)
- [`feupdateenv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/feupdateenv.html)
- XBD [`<fenv.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fenv.h.html)

## CHANGE HISTORY

First released in Issue 6. Derived from the ISO/IEC 9899:1999 standard.

