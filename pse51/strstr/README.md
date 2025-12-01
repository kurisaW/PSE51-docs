# strstr — find a substring

## SYNOPSIS

```c
#include <string.h>

char *strstr(const char *s1, const char *s2);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `strstr()` function shall locate the first occurrence in the string pointed to by `s1` of the sequence of bytes (excluding the terminating NUL character) in the string pointed to by `s2`.

[CX] The `strstr()` function shall not change the setting of `errno` on valid input.

## RETURN VALUE

Upon successful completion, `strstr()` shall return a pointer to the located string or a null pointer if the string is not found.

If `s2` points to a string with zero length, the function shall return `s1`.

## ERRORS

No errors are defined.

---

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`memmem()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/memmem.html)
- [`strchr()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strchr.html)
- XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

First released in Issue 3. Included for alignment with the ANSI C standard.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `strstr()` does not change the setting of `errno` on valid input.

Austin Group Defect 1061 is applied, adding [`memmem()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/memmem.html) to the SEE ALSO section.