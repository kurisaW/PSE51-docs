# confstr — get configurable variables

## SYNOPSIS

```c
#include <unistd.h>

size_t confstr(int name, char *buf, size_t len);
```

## DESCRIPTION

The `confstr()` function shall return configuration-defined string values. Its use and purpose are similar to `sysconf()`, but it is used where string values rather than numeric values are returned.

The `name` argument represents the system variable to be queried. The implementation shall support the following name values, defined in `<unistd.h>`. It may support others:

- `_CS_PATH`
- `_CS_POSIX_V8_ILP32_OFF32_CFLAGS`
- `_CS_POSIX_V8_ILP32_OFF32_LDFLAGS`
- `_CS_POSIX_V8_ILP32_OFF32_LIBS`
- `_CS_POSIX_V8_ILP32_OFFBIG_CFLAGS`
- `_CS_POSIX_V8_ILP32_OFFBIG_LDFLAGS`
- `_CS_POSIX_V8_ILP32_OFFBIG_LIBS`
- `_CS_POSIX_V8_LP64_OFF64_CFLAGS`
- `_CS_POSIX_V8_LP64_OFF64_LDFLAGS`
- `_CS_POSIX_V8_LP64_OFF64_LIBS`
- `_CS_POSIX_V8_LPBIG_OFFBIG_CFLAGS`
- `_CS_POSIX_V8_LPBIG_OFFBIG_LDFLAGS`
- `_CS_POSIX_V8_LPBIG_OFFBIG_LIBS`
- `_CS_POSIX_V8_THREADS_CFLAGS`
- `_CS_POSIX_V8_THREADS_LDFLAGS`
- `_CS_POSIX_V8_WIDTH_RESTRICTED_ENVS`
- `_CS_V8_ENV`

[OB] `_CS_POSIX_V7_ILP32_OFF32_CFLAGS`
[OB] `_CS_POSIX_V7_ILP32_OFF32_LDFLAGS`
[OB] `_CS_POSIX_V7_ILP32_OFF32_LIBS`
[OB] `_CS_POSIX_V7_ILP32_OFFBIG_CFLAGS`
[OB] `_CS_POSIX_V7_ILP32_OFFBIG_LDFLAGS`
[OB] `_CS_POSIX_V7_ILP32_OFFBIG_LIBS`
[OB] `_CS_POSIX_V7_LP64_OFF64_CFLAGS`
[OB] `_CS_POSIX_V7_LP64_OFF64_LDFLAGS`
[OB] `_CS_POSIX_V7_LP64_OFF64_LIBS`
[OB] `_CS_POSIX_V7_LPBIG_OFFBIG_CFLAGS`
[OB] `_CS_POSIX_V7_LPBIG_OFFBIG_LDFLAGS`
[OB] `_CS_POSIX_V7_LPBIG_OFFBIG_LIBS`
[OB] `_CS_POSIX_V7_THREADS_CFLAGS`
[OB] `_CS_POSIX_V7_THREADS_LDFLAGS`
[OB] `_CS_POSIX_V7_WIDTH_RESTRICTED_ENVS`
[OB] `_CS_V7_ENV`

If `len` is not 0, and if `name` has a configuration-defined value, `confstr()` shall copy that value into the `len`-byte buffer pointed to by `buf`. If the string to be returned is longer than `len` bytes, including the terminating null, then `confstr()` shall truncate the string to `len`-1 bytes and null-terminate the result. The application can detect that the string was truncated by comparing the value returned by `confstr()` with `len`.

If `len` is 0 and `buf` is a null pointer, then `confstr()` shall still return the integer value as defined below, but shall not return a string. If `len` is 0 but `buf` is not a null pointer, the result is unspecified.

After a call to:

```c
confstr(_CS_V8_ENV, buf, sizeof(buf))
```

the string stored in `buf` shall contain a space-separated list of the variable=value environment variable pairs an implementation requires as part of specifying a conforming environment, as described in the implementations' conformance documentation.

If the implementation supports the POSIX shell option, the string stored in `buf` after a call to:

```c
confstr(_CS_PATH, buf, sizeof(buf))
```

can be used as a value of the `PATH` environment variable that accesses all of the standard utilities of POSIX.1-2024, that are provided in a manner accessible via the `exec` family of functions, if the return value is less than or equal to `sizeof(buf)`.

## RETURN VALUE

If `name` has a configuration-defined value, `confstr()` shall return the size of buffer that would be needed to hold the entire configuration-defined value including the terminating null. If this return value is greater than `len`, the string returned in `buf` is truncated.

If `name` is invalid, `confstr()` shall return 0 and set `errno` to indicate the error.

If `name` does not have a configuration-defined value, `confstr()` shall return 0 and leave `errno` unchanged.

## ERRORS

The `confstr()` function shall fail if:

- **[EINVAL]**
  - The value of the `name` argument is invalid.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

An application can distinguish between an invalid `name` parameter value and one that corresponds to a configurable variable that has no configuration-defined value by checking if `errno` is modified. This mirrors the behavior of `sysconf()`.

The original need for this function was to provide a way of finding the configuration-defined default value for the environment variable `PATH`. Since `PATH` can be modified by the user to include directories that could contain utilities replacing the standard utilities in the Shell and Utilities volume of POSIX.1-2024, applications need a way to determine the system-supplied `PATH` environment variable value that contains the correct search path for the standard utilities.

An application could use:

```c
confstr(name, (char *)NULL, (size_t)0)
```

to find out how big a buffer is needed for the string value; use `malloc()` to allocate a buffer to hold the string; and call `confstr()` again to get the string. Alternately, it could allocate a fixed, static buffer that is big enough to hold most answers (perhaps 512 or 1024 bytes), but then use `malloc()` to allocate a larger buffer if it finds that this is too small.

## RATIONALE

Application developers can normally determine any configuration variable by means of reading from the stream opened by a call to:

```c
popen("command -p getconf variable", "r");
```

The `confstr()` function with a `name` argument of `_CS_PATH` returns a string that can be used as a `PATH` environment variable setting that will reference the standard shell and utilities as described in the Shell and Utilities volume of POSIX.1-2024.

The `confstr()` function copies the returned string into a buffer supplied by the application instead of returning a pointer to a string. This allows a cleaner function in some implementations (such as those with lightweight threads) and resolves questions about when the application must copy the string returned.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`exec`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html)
- [`fpathconf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fpathconf.html)
- [`sysconf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sysconf.html)

XBD `<unistd.h>`

XCU `c17`

## CHANGE HISTORY

First released in Issue 4. Derived from the ISO POSIX-2 standard.

### Issue 5

A table indicating the permissible values of `name` is added to the DESCRIPTION. All those marked EX are new in this version.

### Issue 6

The Open Group Corrigendum U033/7 is applied. The return value for the case returning the size of the buffer now explicitly states that this includes the terminating null.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

- The DESCRIPTION is updated with new arguments which can be used to determine configuration strings for C compiler flags, linker/loader flags, and libraries for each different supported programming environment. This is a change to support data size neutrality.

The following changes were made to align with the IEEE P1003.1a draft standard:

- The DESCRIPTION is updated to include text describing how `_CS_PATH` can be used to obtain a `PATH` to access the standard utilities.

The macros associated with the `c89` programming models are marked LEGACY and new equivalent macros associated with `c99` are introduced.

### Issue 7

Austin Group Interpretation 1003.1-2001 #047 is applied, adding the `_CS_V7_ENV` variable.

Austin Group Interpretations 1003.1-2001 #166 is applied to permit an additional compiler flag to enable threads.

The V6 variables for the supported programming environments are marked obsolescent.

The variables for the supported programming environments are updated to be V7.

The LEGACY variables and obsolescent values are removed.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0070 [810] and XSH/TC2-2008/0071 [911] are applied.

### Issue 8

Austin Group Defect 1330 is applied, changing "_V7_" to "_V8_" and "_V6_" to "_V7_".

---

*End of informative text.*

---

*UNIX® is a registered Trademark of The Open Group.*
*POSIX™ is a Trademark of The IEEE.*
*Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved*