# strtoumax

## SYNOPSIS

```c
#include <inttypes.h>

intmax_t strtoimax(const char *restrict nptr,
                   char **restrict endptr,
                   int base);
uintmax_t strtoumax(const char *restrict nptr,
                    char **restrict endptr,
                    int base);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

These functions shall be equivalent to the `strtol()`, `strtoll()`, `strtoul()`, and `strtoull()` functions, except that the initial portion of the string shall be converted to **intmax_t** and **uintmax_t** representation, respectively.

## RETURN VALUE

These functions shall return the converted value, if any.

If no conversion could be performed, zero shall be returned [CX] and `errno` may be set to [EINVAL].

[CX] If the value of `base` is not supported, 0 shall be returned and `errno` shall be set to [EINVAL].

If the correct value is outside the range of representable values, {INTMAX_MAX}, {INTMAX_MIN}, or {UINTMAX_MAX} shall be returned (according to the return type and sign of the value, if any), and `errno` shall be set to [ERANGE].

## ERRORS

These functions shall fail if:

- **[EINVAL]** [CX] The value of `base` is not supported.
- **[ERANGE]** The value to be returned is not representable.

These functions may fail if:

- **[EINVAL]** No conversion could be performed.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

Since the value of `*endptr` is unspecified if the value of `base` is not supported, applications should either ensure that `base` has a supported value (0 or between 2 and 36) before the call, or check for an [EINVAL] error before examining `*endptr`.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`strtol()`](strtol.html)
- [`strtoul()`](strtoul.html)
- XBD [`<inttypes.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/inttypes.h.html)

## CHANGE HISTORY

First released in Issue 6. Derived from the ISO/IEC 9899:1999 standard.

### Issue 7

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0613 [453] and XSH/TC1-2008/0614 [453] are applied.

---
