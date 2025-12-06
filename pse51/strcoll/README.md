# strcoll

## SYNOPSIS

```c
#include <string.h>

int strcoll(const char *s1, const char *s2);

int strcoll_l(const char *s1, const char *s2, locale_t locale);
```

## DESCRIPTION

For `strcoll()`: The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `strcoll()` and `strcoll_l()` functions shall compare the string pointed to by `s1` to the string pointed to by `s2`, both interpreted as appropriate to the `LC_COLLATE` category of the current locale, or of the locale represented by `locale`, respectively.

The `strcoll()` and `strcoll_l()` functions shall not change the setting of `errno` if successful.

Since no return value is reserved to indicate an error, an application wishing to check for error situations should set `errno` to 0, then call `strcoll()`, or `strcoll_l()`, then check `errno`.

The behavior is undefined if the `locale` argument to `strcoll_l()` is the special locale object LC_GLOBAL_LOCALE or is not a valid locale object handle.

## RETURN VALUE

Upon successful completion, `strcoll()` shall return an integer greater than, equal to, or less than 0, according to whether the string pointed to by `s1` is greater than, equal to, or less than the string pointed to by `s2` when both are interpreted as appropriate to the current locale. On error, `strcoll()` may set `errno`, but no return value is reserved to indicate an error.

Upon successful completion, `strcoll_l()` shall return an integer greater than, equal to, or less than 0, according to whether the string pointed to by `s1` is greater than, equal to, or less than the string pointed to by `s2` when both are interpreted as appropriate to the locale represented by `locale`. On error, `strcoll_l()` may set `errno`, but no return value is reserved to indicate an error.

## ERRORS

These functions may fail if:

- **[EINVAL]**
  - The `s1` or `s2` arguments contain characters outside the domain of the collating sequence.

## EXAMPLES

### Comparing Nodes

The following example uses an application-defined function, `node_compare()`, to compare two nodes based on an alphabetical ordering of the `string` field.

```c
#include <string.h>
...
struct node { /* These are stored in the table. */
    char *string;
    int length;
};
...
int node_compare(const void *node1, const void *node2)
{
    return strcoll(((const struct node *)node1)->string,
        ((const struct node *)node2)->string);
}
...
```

## APPLICATION USAGE

The `strxfrm()` and `strcmp()` functions should be used for sorting large lists.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `alphasort()`
- `strcmp()`
- `strxfrm()`

XBD `<string.h>`

## CHANGE HISTORY

### First released in Issue 3.

### Issue 5

The DESCRIPTION is updated to indicate that `errno` does not change if the function is successful.

### Issue 6

Extensions beyond the ISO C standard are marked.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

- The [EINVAL] optional error condition is added.

An example is added.

### Issue 7

The `strcoll_l()` function is added from The Open Group Technical Standard, 2006, Extended API Set Part 4.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0593 [283] and XSH/TC1-2008/0594 [283] are applied.

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*