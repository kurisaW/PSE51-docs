# setenv — add or change environment variable

## SYNOPSIS

```c
#include <stdlib.h>

int setenv(const char *envname, const char *envval, int overwrite);
```

## DESCRIPTION

The `setenv()` function shall update or add a variable in the environment of the calling process. The `envname` argument points to a string containing the name of an environment variable to be added or altered. The environment variable shall be set to the value to which `envval` points. The function shall fail if `envname` points to a string which contains an '=' character.

If the environment variable named by `envname` already exists and the value of `overwrite` is non-zero, the function shall return success and the environment shall be updated. If the environment variable named by `envname` already exists and the value of `overwrite` is zero, the function shall return success and the environment shall remain unchanged.

The `setenv()` function shall update the list of pointers to which `environ` points.

The strings described by `envname` and `envval` are copied by this function.

The `setenv()` function need not be thread-safe.

## RETURN VALUE

Upon successful completion, zero shall be returned. Otherwise, -1 shall be returned, `errno` set to indicate the error, and the environment shall be unchanged.

## ERRORS

The `setenv()` function shall fail if:

- **[EINVAL]**
  - The `envname` argument points to an empty string or points to a string containing an '=' character.

- **[ENOMEM]**
  - Insufficient memory was available to add a variable or its value to the environment.

## EXAMPLES

None.

## APPLICATION USAGE

See `exec()` for restrictions on changing the environment in multi-threaded applications.

## RATIONALE

Unanticipated results may occur if `setenv()` changes the external variable `environ`. In particular, if the optional `envp` argument to `main()` is present, it is not changed, and thus may point to an obsolete copy of the environment (as may any other copy of `environ`). However, other than the aforementioned restriction, the standard developers intended that the traditional method of walking through the environment by way of the `environ` pointer must be supported.

It was decided that `setenv()` should be required by this version because it addresses a piece of missing functionality, and does not impose a significant burden on the implementor.

There was considerable debate as to whether the System V `putenv()` function or the BSD `setenv()` function should be required as a mandatory function. The `setenv()` function was chosen because it permitted the implementation of the `unsetenv()` function to delete environmental variables, without specifying an additional interface. The `putenv()` function is available as part of the XSI option.

The standard developers considered requiring that `setenv()` indicate an error when a call to it would result in exceeding {ARG_MAX}. The requirement was rejected since the condition might be temporary, with the application eventually reducing the environment size. The ultimate success or failure depends on the size at the time of a call to `exec`, which returns an indication of this error condition.

See also the RATIONALE section in `getenv()`.

## FUTURE DIRECTIONS

None.

## SEE ALSO

[exec](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html),
[getenv()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getenv.html),
[putenv()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/putenv.html),
[unsetenv()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/unsetenv.html)

XBD [<stdlib.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html),
[<sys/types.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_types.h.html),
[<unistd.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/unistd.h.html)

## CHANGE HISTORY

First released in Issue 6. Derived from the IEEE P1003.1a draft standard.

IEEE Std 1003.1-2001/Cor 1-2002, item XSH/TC1/D6/55 is applied, adding references to `exec` in the APPLICATION USAGE and SEE ALSO sections.

### Issue 7

Austin Group Interpretation 1003.1-2001 #156 is applied.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0549 [167], XSH/TC1-2008/0550 [185], XSH/TC1-2008/0551 [167], and XSH/TC1-2008/0552 [38] are applied.

---

*UNIX® is a registered Trademark of The Open Group.*
*POSIX™ is a Trademark of The IEEE.*
*Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved*