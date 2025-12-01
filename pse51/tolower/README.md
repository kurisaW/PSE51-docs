# tolower, tolower_l — transliterate uppercase characters to lowercase

## SYNOPSIS

```c
#include <ctype.h>

int tolower(int c);

[CX] int tolower_l(int c, locale_t locale);
```

## DESCRIPTION

For `tolower()`: [CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `tolower()` [CX] and `tolower_l()` functions have as a domain a type `int`, the value of which is representable as an `unsigned char` or the value of EOF. If the argument has any other value, the behavior is undefined. If the argument of `tolower()` [CX] or `tolower_l()` represents an uppercase letter, and there exists a corresponding lowercase letter as defined by character type information in the current locale [CX] or in the locale represented by `locale`, respectively (category `LC_CTYPE`), the result shall be the corresponding lowercase letter. All other arguments in the domain are returned unchanged.

[CX] The behavior is undefined if the `locale` argument to `tolower_l()` is the special locale object LC_GLOBAL_LOCALE or is not a valid locale object handle.

## RETURN VALUE

Upon successful completion, the `tolower()` [CX] and `tolower_l()` functions shall return the lowercase letter corresponding to the argument passed; otherwise, they shall return the argument unchanged.

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

- `setlocale()`
- `uselocale()`
- XBD 7. Locale
- `<ctype.h>`
- `<locale.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

Extensions beyond the ISO C standard are marked.

### Issue 7

The `tolower_l()` function is added from The Open Group Technical Standard, 2006, Extended API Set Part 4.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0671 [283] and XSH/TC1-2008/0672 [283] are applied.

---

*The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group*