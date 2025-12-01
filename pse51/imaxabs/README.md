# imaxabs — return absolute value

## SYNOPSIS

```c
#include <inttypes.h>

intmax_t imaxabs(intmax_t j);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `imaxabs()` function shall compute the absolute value of an integer `j`. If the result cannot be represented, the behavior is undefined.

## RETURN VALUE

The `imaxabs()` function shall return the absolute value.

## ERRORS

No errors are defined.

---

## EXAMPLES

None.

## APPLICATION USAGE

Since POSIX.1 requires a two's complement representation of `intmax_t`, the absolute value of the negative integer with the largest magnitude {INTMAX_MIN} is not representable, thus `imaxabs(INTMAX_MIN)` is undefined.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`imaxdiv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/imaxdiv.html)
- XBD [`<inttypes.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/inttypes.h.html)

## CHANGE HISTORY

First released in Issue 6. Derived from the ISO/IEC 9899:1999 standard.

### Issue 8

Austin Group Defect 1108 is applied, changing the APPLICATION USAGE section.