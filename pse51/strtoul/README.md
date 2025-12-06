# strtoul

## SYNOPSIS

```c
#include <stdlib.h>

unsigned long strtoul(const char *restrict str,
                      char **restrict endptr, int base);
unsigned long long strtoull(const char *restrict str,
                            char **restrict endptr, int base);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

These functions shall convert the initial portion of the string pointed to by `str` to a type **unsigned long** and **unsigned long long** representation, respectively. First, they decompose the input string into three parts:

1.  An initial, possibly empty, sequence of white-space bytes

2.  A subject sequence interpreted as an integer represented in some radix determined by the value of `base`

3.  A final string of one or more unrecognized characters, including the terminating NUL character of the input string


Then they shall attempt to convert the subject sequence to an unsigned integer, and return the result.

If the value of `base` is 0, the expected form of the subject sequence is that of a decimal constant, octal constant, or hexadecimal constant, any of which may be preceded by a '+' or '-' sign. A decimal constant begins with a non-zero digit, and consists of a sequence of decimal digits. An octal constant consists of the prefix '0' optionally followed by a sequence of the digits '0' to '7' only. A hexadecimal constant consists of the prefix 0x or 0X followed by a sequence of the decimal digits and letters 'a' (or 'A') to 'f' (or 'F') with values 10 to 15 respectively.

If the value of `base` is between 2 and 36, the expected form of the subject sequence is a sequence of letters and digits representing an integer with the radix specified by `base`, optionally preceded by a '+' or '-' sign. The letters from 'a' (or 'A') to 'z' (or 'Z') inclusive are ascribed the values 10 to 35; only letters whose ascribed values are less than that of `base` are permitted. If the value of `base` is 16, the characters 0x or 0X may optionally precede the sequence of letters and digits, following the sign if present.

The subject sequence is defined as the longest initial subsequence of the input string, starting with the first non-white-space byte, that is of the expected form. The subject sequence shall contain no characters if the input string is empty or consists entirely of white-space bytes, or if the first non-white-space byte is other than a sign or a permissible letter or digit.

If the subject sequence has the expected form and the value of `base` is 0, the sequence of characters starting with the first digit shall be interpreted as an integer constant. If the subject sequence has the expected form and the value of `base` is between 2 and 36, it shall be used as the base for conversion, ascribing to each letter its value as given above. If the subject sequence begins with a <hyphen-minus>, the resulting value shall be the negative of the converted value; this action shall be performed in the return type. A pointer to the final string shall be stored in the object pointed to by `endptr`, provided that `endptr` is not a null pointer.

In other than the C [CX] or POSIX locale, additional locale-specific subject sequence forms may be accepted.

If the subject sequence is empty or does not have the expected form, no conversion shall be performed; the value of `str` shall be stored in the object pointed to by `endptr`, provided that `endptr` is not a null pointer.

These functions shall not change the setting of `errno` if successful.

Since 0, {ULONG_MAX}, and {ULLONG_MAX} are returned on error and are also valid returns on success, an application wishing to check for error situations should set `errno` to 0, then call `strtoul()` or `strtoull()`, then check `errno`.

## RETURN VALUE

Upon successful completion, these functions shall return the converted value, if any. If no conversion could be performed, 0 shall be returned [CX] and `errno` may be set to [EINVAL].

[CX] If the value of `base` is not supported, 0 shall be returned and `errno` shall be set to [EINVAL].

If the correct value is outside the range of representable values, {ULONG_MAX} or {ULLONG_MAX} shall be returned and `errno` set to [ERANGE].

## ERRORS

These functions shall fail if:

- **[EINVAL]**: [CX] The value of `base` is not supported.
- **[ERANGE]**: The value to be returned is not representable.

These functions may fail if:

- **[EINVAL]**: [CX] No conversion could be performed.

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

- `fscanf()`
- `isalpha()`
- `strtod()`
- `strtol()`

XBD `<stdlib.h>`

## CHANGE HISTORY

### First released in Issue 4
Derived from the ANSI C standard.

### Issue 5
The DESCRIPTION is updated to indicate that `errno` is not changed if the function is successful.

### Issue 6
Extensions beyond the ISO C standard are marked.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

- The [EINVAL] error condition is added for when the value of `base` is not supported.
- In the RETURN VALUE and ERRORS sections, the [EINVAL] optional error condition is added if no conversion could be performed.

The following changes are made for alignment with the ISO/IEC 9899:1999 standard:

- The `strtoul()` prototype is updated.
- The `strtoull()` function is added.

### Issue 7
POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0621 [105], XSH/TC1-2008/0622 [453], and XSH/TC1-2008/0623 [453] are applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0355 [584] and XSH/TC2-2008/0356 [796] are applied.

### Issue 8
Austin Group Defect 700 is applied, clarifying how a subject sequence beginning with <hyphen-minus> is converted.

Austin Group Defect 1163 is applied, clarifying the handling of white space in the input string.

---

