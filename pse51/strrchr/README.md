# strrchr — string scanning operation

## SYNOPSIS

```c
#include <string.h>

char *strrchr(const char *s, int c);
```

## DESCRIPTION

The `strrchr()` function shall locate the last occurrence of `c` (converted to a `char`) in the string pointed to by `s`. The terminating NUL character is considered to be part of the string.

The `strrchr()` function shall not change the setting of `errno` on valid input.

## RETURN VALUE

Upon successful completion, `strrchr()` shall return a pointer to the byte or a null pointer if `c` does not occur in the string.

## ERRORS

No errors are defined.

## EXAMPLES

### Finding the Base Name of a File

The following example uses `strrchr()` to get a pointer to the base name of a file. The `strrchr()` function searches backwards through the name of the file to find the last '/' character in `name`. This pointer (plus one) will point to the base name of the file.

```c
#include <string.h>
...
const char *name;
char *basename;
...
basename = strrchr(name, '/') + 1;
...
```

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`strchr()`](strchr.html)
- [`<string.h>`](basedefs/string.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `strrchr()` does not change the setting of `errno` on valid input.

---

*Source: The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*