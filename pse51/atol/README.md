# atol, atoll — convert a string to a long integer

## SYNOPSIS

```c
#include <stdlib.h>

long atol(const char *nptr);
long long atoll(const char *nptr);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

Except as noted below, the call `atol(nptr)` shall be equivalent to:

```c
strtol(nptr, (char **)NULL, 10)
```

Except as noted below, the call to `atoll(nptr)` shall be equivalent to:

```c
strtoll(nptr, (char **)NULL, 10)
```

The handling of errors may differ. If the value cannot be represented, the behavior is undefined.

## RETURN VALUE

These functions shall return the converted value if the value can be represented.

## ERRORS

No errors are defined.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

If the number is not known to be in range, `strtol()` or `strtoll()` should be used because `atol()` and `atoll()` are not required to perform any error checking.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `strtol()`
- `<stdlib.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

The `atoll()` function is added for alignment with the ISO/IEC 9899:1999 standard.

### Issue 7

SD5-XSH-ERN-61 is applied, correcting the DESCRIPTION of `atoll()`.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0046 [892] is applied.

---

*End of informative text.*

*UNIX® is a registered Trademark of The Open Group.*
*POSIX™ is a Trademark of The IEEE.*
*Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved*