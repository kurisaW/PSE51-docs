# strncmp

## SYNOPSIS

```c
#include <string.h>

int strncmp(const char *s1, const char *s2, size_t n);
```

## DESCRIPTION

[Option Start] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard. [Option End]

The `strncmp()` function shall compare not more than `n` bytes (bytes that follow a NUL character are not compared) from the array pointed to by `s1` to the array pointed to by `s2`.

The sign of a non-zero return value is determined by the sign of the difference between the values of the first pair of bytes (both interpreted as type `unsigned char`) that differ in the strings being compared.

[Option Start] The `strncmp()` function shall not change the setting of `errno` on valid input. [Option End]

## RETURN VALUE

Upon successful completion, `strncmp()` shall return an integer greater than, equal to, or less than 0, if the possibly null-terminated array pointed to by `s1` is greater than, equal to, or less than the possibly null-terminated array pointed to by `s2` respectively.

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

- [`strcmp()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strcmp.html)
- XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

Extensions beyond the ISO C standard are marked.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `strncmp()` does not change the setting of `errno` on valid input.

