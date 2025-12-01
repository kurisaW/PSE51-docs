# strlen, strnlen — get length of fixed size string

## SYNOPSIS

```c
#include <string.h>

size_t strlen(const char *s);

[CX] size_t strnlen(const char *s, size_t maxlen);
```

## DESCRIPTION

For `strlen()`: [CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `strlen()` function shall compute the number of bytes in the string to which `s` points, not including the terminating NUL character.

[CX] The `strnlen()` function shall compute the smaller of the number of bytes in the array to which `s` points, not including any terminating NUL character, or the value of the `maxlen` argument. The `strnlen()` function shall never examine more than `maxlen` bytes of the array pointed to by `s`.

[CX] The `strlen()` and `strnlen()` functions shall not change the setting of `errno` on valid input.

## RETURN VALUE

The `strlen()` function shall return the length of `s`; no return value shall be reserved to indicate an error.

[CX] The `strnlen()` function shall return the number of bytes preceding the first null byte in the array to which `s` points, if `s` contains a null byte within the first `maxlen` bytes; otherwise, it shall return `maxlen`.

## ERRORS

No errors are defined.

## EXAMPLES

### Getting String Lengths

The following example sets the maximum length of `key` and `data` by using `strlen()` to get the lengths of those strings.

```c
#include <string.h>
...
struct element {
    char *key;
    char *data;
};
...
char *key, *data;
int len;

*keylength = *datalength = 0;
...
if ((len = strlen(key)) > *keylength)
    *keylength = len;
if ((len = strlen(data)) > *datalength)
    *datalength = len;
...
```

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`strlcat()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strlcat.html)
- [`wcslen()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/wcslen.html)

XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 5

The RETURN VALUE section is updated to indicate that `strlen()` returns the length of `s`, and not `s` itself as was previously stated.

### Issue 7

The `strnlen()` function is added from The Open Group Technical Standard, 2006, Extended API Set Part 1.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0344 [560] is applied.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `strlen()` and `strnlen()` do not change the setting of `errno` on valid input.

Austin Group Defect 986 is applied, adding [`strlcat()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strlcat.html) to the SEE ALSO section.