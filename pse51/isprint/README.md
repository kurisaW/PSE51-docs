# isprint, isprint_l — test for a printable character

## SYNOPSIS

```c
#include <ctype.h>

int isprint(int c);

/* [CX] Extension */
int isprint_l(int c, locale_t locale);
```

## DESCRIPTION

For `isprint()`:
[The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.]

The `isprint()` and `isprint_l()` functions shall test whether `c` is a character of class **print** in the current locale, or in the locale represented by `locale`, respectively; see XBD [7. Locale].

The `c` argument is an `int`, the value of which the application shall ensure is a character representable as an `unsigned char` or equal to the value of the macro EOF. If the argument has any other value, the behavior is undefined.

[The behavior is undefined if the `locale` argument to `isprint_l()` is the special locale object LC_GLOBAL_LOCALE or is not a valid locale object handle.]

## RETURN VALUE

The `isprint()` and `isprint_l()` functions shall return non-zero if `c` is a printable character; otherwise, they shall return 0.

## ERRORS

No errors are defined.

---

## APPLICATION USAGE

To ensure applications portability, especially across natural languages, only these functions and the functions in the reference pages listed in the SEE ALSO section should be used for character classification.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`isalnum()`](isalnum.html)
- [`isalpha()`](isalpha.html)
- [`isblank()`](isblank.html)
- [`iscntrl()`](iscntrl.html)
- [`isdigit()`](isdigit.html)
- [`isgraph()`](isgraph.html)
- [`islower()`](islower.html)
- [`ispunct()`](ispunct.html)
- [`isspace()`](isspace.html)
- [`isupper()`](isupper.html)
- [`isxdigit()`](isxdigit.html)
- [`setlocale()`](setlocale.html)
- [`uselocale()`](uselocale.html)

XBD [7. Locale], [`<ctype.h>`](ctype.h), [`<locale.h>`](locale.h)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

The normative text is updated to avoid use of the term "must" for application requirements.

### Issue 7

The `isprint_l()` function is added from The Open Group Technical Standard, 2006, Extended API Set Part 4.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0297 [302], XSH/TC1-2008/0298 [283], and XSH/TC1-2008/0299 [283] are applied.

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*