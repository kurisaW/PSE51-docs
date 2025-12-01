# unsetenv

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

unsetenv — remove an environment variable

## SYNOPSIS

```c
#include <stdlib.h>

int unsetenv(const char *name);
```

## DESCRIPTION

The `unsetenv()` function shall remove an environment variable from the environment of the calling process. The `name` argument points to a string, which is the name of the variable to be removed. The named argument shall not contain an '=' character. If the named variable does not exist in the current environment, the environment shall be unchanged and the function is considered to have completed successfully.

The `unsetenv()` function shall update the list of pointers to which `environ` points.

The `unsetenv()` function need not be thread-safe.

## RETURN VALUE

Upon successful completion, zero shall be returned. Otherwise, -1 shall be returned, `errno` set to indicate the error, and the environment shall be unchanged.

## ERRORS

The `unsetenv()` function shall fail if:

- **[EINVAL]**
  - The `name` argument points to an empty string, or points to a string containing an '=' character.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

Refer to the RATIONALE section in `setenv()`.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `getenv()`
- `setenv()`
- XBD `<stdlib.h>`, `<sys/types.h>`

## CHANGE HISTORY

First released in Issue 6. Derived from the IEEE P1003.1a draft standard.

### Issue 7

Austin Group Interpretation 1003.1-2001 #156 is applied.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0698 [167] and XSH/TC1-2008/0699 [185] are applied.

---

*UNIX® is a registered Trademark of The Open Group.*
*POSIX™ is a Trademark of The IEEE.*
*Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved*