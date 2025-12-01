# toupper, toupper_l — transliterate lowercase characters to uppercase

## SYNOPSIS

```c
#include <ctype.h>

int toupper(int c);

/* XSI option */
int toupper_l(int c, locale_t locale);
```

## DESCRIPTION

For `toupper()`:
The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `toupper()` and `toupper_l()` functions have as a domain a type `int`, the value of which is representable as an `unsigned char` or the value of EOF. If the argument has any other value, the behavior is undefined.

If the argument of `toupper()` or `toupper_l()` represents a lowercase letter, and there exists a corresponding uppercase letter as defined by character type information in the current locale or in the locale represented by `locale`, respectively (category `LC_CTYPE`), the result shall be the corresponding uppercase letter.

All other arguments in the domain are returned unchanged.

The behavior is undefined if the `locale` argument to `toupper_l()` is the special locale object LC_GLOBAL_LOCALE or is not a valid locale object handle.

## RETURN VALUE

Upon successful completion, `toupper()` and `toupper_l()` shall return the uppercase letter corresponding to the argument passed; otherwise, they shall return the argument unchanged.

## ERRORS

No errors are defined.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`setlocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setlocale.html)
- [`uselocale()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/uselocale.html)
- XBD 7. Locale
- [`<ctype.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/ctype.h.html)
- [`<locale.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/locale.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

Extensions beyond the ISO C standard are marked.

### Issue 7

SD5-XSH-ERN-181 is applied, clarifying the RETURN VALUE section.

The `toupper_l()` function is added from The Open Group Technical Standard, 2006, Extended API Set Part 4.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0673 [283] and XSH/TC1-2008/0674 [283] are applied.