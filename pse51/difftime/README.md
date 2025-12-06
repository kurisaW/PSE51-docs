# difftime — compute the difference between two calendar time values

## SYNOPSIS

```c
#include <time.h>

double difftime(time_t time1, time_t time0);
```

## DESCRIPTION

The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `difftime()` function shall compute the difference between two calendar times (as returned by `time()`): `time1 - time0`.

## RETURN VALUE

The `difftime()` function shall return the difference expressed in seconds as a type `double`.

## ERRORS

No errors are defined.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `asctime()`
- `clock()`
- `ctime()`
- `futimens()`
- `gmtime()`
- `localtime()`
- `mktime()`
- `strftime()`
- `strptime()`
- `time()`

[XBD `<time.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/time.h.html)

## CHANGE HISTORY

First released in Issue 4. Derived from the ISO C standard.

---

