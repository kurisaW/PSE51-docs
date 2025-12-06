# lldiv

## SYNOPSIS

```c
#include <stdlib.h>

ldiv_t ldiv(long numer, long denom);
lldiv_t lldiv(long long numer, long long denom);
```

## DESCRIPTION

These functions shall compute the quotient and remainder of the division of the numerator `numer` by the denominator `denom`. If the division is inexact, the resulting quotient is the **long** integer (for the `ldiv()` function) or **long long** integer (for the `lldiv()` function) of lesser magnitude that is the nearest to the algebraic quotient. If the result cannot be represented, the behavior is undefined; otherwise, `quot` * `denom` + `rem` shall equal `numer`.

## RETURN VALUE

The `ldiv()` function shall return a structure of type **ldiv_t**, comprising both the quotient and the remainder. The structure shall include the following members, in any order:

```c
long   quot;    /* Quotient */
long   rem;     /* Remainder */
```

The `lldiv()` function shall return a structure of type **lldiv_t**, comprising both the quotient and the remainder. The structure shall include the following members, in any order:

```c
long long   quot;    /* Quotient */
long long   rem;     /* Remainder */
```

## ERRORS

No errors are defined.

* * *

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`div()`](div.md)

[XBD `<stdlib.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## CHANGE HISTORY

First released in Issue 4. Derived from the ISO C standard.

### Issue 6

The `lldiv()` function is added for alignment with the ISO/IEC 9899:1999 standard.

---

*UNIX® is a registered Trademark of The Open Group.
POSIX™ is a Trademark of The IEEE.
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved*