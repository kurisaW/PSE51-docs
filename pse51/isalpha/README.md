# isalpha, isalpha_l — test for an alphabetic character

## SYNOPSIS

```c
#include <ctype.h>

int isalpha(int c);

[CX] int isalpha_l(int c, locale_t locale);
```

## DESCRIPTION

For `isalpha()`: [CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `isalpha()` [CX] and `isalpha_l()` functions shall test whether `c` is a character of class **alpha** in the current locale, [CX] or in the locale represented by `locale`, respectively; see XBD 7. Locale.

The `c` argument is an `int`, the value of which the application shall ensure is representable as an `unsigned char` or equal to the value of the macro EOF. If the argument has any other value, the behavior is undefined.

[CX] The behavior is undefined if the `locale` argument to `isalpha_l()` is the special locale object LC_GLOBAL_LOCALE or is not a valid locale object handle.

## RETURN VALUE

The `isalpha()` [CX] and `isalpha_l()` functions shall return non-zero if `c` is an alphabetic character; otherwise, they shall return 0.

## ERRORS

No errors are defined.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

To ensure applications portability, especially across natural languages, only these functions and the functions in the reference pages listed in the SEE ALSO section should be used for character classification.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `isalnum()`
- `isblank()`
- `iscntrl()`
- `isdigit()`
- `isgraph()`
- `islower()`
- `isprint()`
- `ispunct()`
- `isspace()`
- `isupper()`
- `isxdigit()`
- `setlocale()`
- `uselocale()`

XBD 7. Locale, `<ctype.h>`, `<locale.h>`, `<stdio.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

The normative text is updated to avoid use of the term "must" for application requirements.

### Issue 7

The `isalpha_l()` function is added from The Open Group Technical Standard, 2006, Extended API Set Part 4.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0277 [302], XSH/TC1-2008/0278 [283], and XSH/TC1-2008/0279 [283] are applied.

---

*End of informative text.*

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*