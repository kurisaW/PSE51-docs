# iscntrl, iscntrl_l - test for a control character

## SYNOPSIS

```c
#include <ctype.h>

int iscntrl(int c);

[CX] int iscntrl_l(int c, locale_t locale);
```

## DESCRIPTION

For `iscntrl()`: [CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `iscntrl()` [CX] and `iscntrl_l()` functions shall test whether `c` is a character of class **cntrl** in the current locale, [CX] or in the locale represented by `locale`, respectively; see XBD [7. Locale].

The `c` argument is a type `int`, the value of which the application shall ensure is a character representable as an `unsigned char` or equal to the value of the macro EOF. If the argument has any other value, the behavior is undefined.

[CX] The behavior is undefined if the `locale` argument to `iscntrl_l()` is the special locale object LC_GLOBAL_LOCALE or is not a valid locale object handle.

## RETURN VALUE

The `iscntrl()` [CX] and `iscntrl_l()` functions shall return non-zero if `c` is a control character; otherwise, they shall return 0.

## ERRORS

No errors are defined.

## EXAMPLES

None.

## APPLICATION USAGE

To ensure applications portability, especially across natural languages, only these functions and the functions in the reference pages listed in the SEE ALSO section should be used for character classification.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`isalnum()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalnum.html)
- [`isalpha()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isalpha.html)
- [`isblank()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isblank.html)
- [`isdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isdigit.html)
- [`isgraph()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isgraph.html)
- [`islower()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/islower.html)
- [`isprint()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isprint.html)
- [`ispunct()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ispunct.html)
- [`isspace()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isspace.html)
- [`isupper()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isupper.html)
- [`isxdigit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/isxdigit.html)
- [`setlocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlocale.html)
- [`uselocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/uselocale.html)

XBD [7. Locale], [`<ctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html), [`<locale.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/locale.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

The normative text is updated to avoid use of the term "must" for application requirements.

### Issue 7

The `iscntrl_l()` function is added from The Open Group Technical Standard, 2006, Extended API Set Part 4.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0283 [302], XSH/TC1-2008/0284 [283], and XSH/TC1-2008/0285 [283] are applied.

---
