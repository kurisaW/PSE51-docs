# imaxdiv

## NAME

imaxdiv — return quotient and remainder

## SYNOPSIS

```c
#include <inttypes.h>

imaxdiv_t imaxdiv(intmax_t numer, intmax_t denom);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `imaxdiv()` function shall compute `numer / denom` and `numer % denom` in a single operation.

## RETURN VALUE

The `imaxdiv()` function shall return a structure of type `imaxdiv_t`, comprising both the quotient and the remainder. The structure shall contain (in either order) the members `quot` (the quotient) and `rem` (the remainder), each of which has type `intmax_t`.

If either part of the result cannot be represented, the behavior is undefined.

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

- `imaxabs()`
- XBD `<inttypes.h>`

## CHANGE HISTORY

First released in Issue 6. Derived from the ISO/IEC 9899:1999 standard.

---

*End of informative text.*