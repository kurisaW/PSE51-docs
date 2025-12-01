# strxfrm, strxfrm_l — string transformation

## SYNOPSIS

```c
#include <string.h>

size_t strxfrm(char *restrict s1, const char *restrict s2, size_t n);

/* [CX] Extension */
size_t strxfrm_l(char *restrict s1, const char *restrict s2,
                 size_t n, locale_t locale);
```

## DESCRIPTION

For `strxfrm()`: [CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `strxfrm()` [CX] and `strxfrm_l()` functions shall transform the string pointed to by `s2` and place the resulting string into the array pointed to by `s1`. The transformation is such that if `strcmp()` is applied to two transformed strings, it shall return a value greater than, equal to, or less than 0, corresponding to the result of `strcoll()` [CX] or `strcoll_l()`, respectively, applied to the same two original strings [CX] with the same locale. No more than `n` bytes are placed into the resulting array pointed to by `s1`, including the terminating NUL character. If `n` is 0, `s1` is permitted to be a null pointer. If copying takes place between objects that overlap, the behavior is undefined.

[CX] The `strxfrm()` and `strxfrm_l()` functions shall not change the setting of `errno` if successful.

Since no return value is reserved to indicate an error, an application wishing to check for error situations should set `errno` to 0, then call `strxfrm()` [CX] or `strxfrm_l()`, then check `errno`.

[CX] The behavior is undefined if the `locale` argument to `strxfrm_l()` is the special locale object LC_GLOBAL_LOCALE or is not a valid locale object handle.

## RETURN VALUE

Upon successful completion, `strxfrm()` [CX] and `strxfrm_l()` shall return the length of the transformed string (not including the terminating NUL character). If the value returned is `n` or more, the contents of the array pointed to by `s1` are unspecified.

On error, `strxfrm()` [CX] and `strxfrm_l()` may set `errno` but no return value is reserved to indicate an error.

## ERRORS

These functions may fail if:

- **[EINVAL]** [CX] The string pointed to by the `s2` argument contains characters outside the domain of the collating sequence.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

The transformation function is such that two transformed strings can be ordered by `strcmp()` as appropriate to collating sequence information in the current locale (category `LC_COLLATE`).

The fact that when `n` is 0 `s1` is permitted to be a null pointer is useful to determine the size of the `s1` array prior to making the transformation.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `strcmp()`
- `strcoll()`
- `<string.h>`

## CHANGE HISTORY

First released in Issue 3. Included for alignment with the ISO C standard.

### Issue 5

The DESCRIPTION is updated to indicate that `errno` does not change if the function is successful.

### Issue 6

Extensions beyond the ISO C standard are marked.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

- In the RETURN VALUE and ERRORS sections, the [EINVAL] optional error condition is added if no conversion could be performed.

The `strxfrm()` prototype is updated for alignment with the ISO/IEC 9899:1999 standard.

### Issue 7

The `strxfrm_l()` function is added from The Open Group Technical Standard, 2006, Extended API Set Part 4.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0624 [283], XSH/TC1-2008/0625 [283], and XSH/TC1-2008/0626 [302] are applied.

---

*End of informative text.*

UNIX® is a registered Trademark of The Open Group.
POSIX™ is a Trademark of The IEEE.
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved.