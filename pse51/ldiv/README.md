# ldiv, lldiv — compute quotient and remainder of a long division

## SYNOPSIS

```c
#include <stdlib.h>

ldiv_t ldiv(long numer, long denom);
lldiv_t lldiv(long long numer, long long denom);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

These functions shall compute the quotient and remainder of the division of the numerator `numer` by the denominator `denom`. If the division is inexact, the resulting quotient is the **long** integer (for the `ldiv()` function) or **long long** integer (for the `lldiv()` function) of lesser magnitude that is the nearest to the algebraic quotient. If the result cannot be represented, the behavior is undefined; otherwise, `quot * denom + rem` shall equal `numer`.

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

## SEE ALSO

- [`div()`](div.html)
- [`<stdlib.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## CHANGE HISTORY

First released in Issue 4. Derived from the ISO C standard.

### Issue 6

The `lldiv()` function is added for alignment with the ISO/IEC 9899:1999 standard.

---

*The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*