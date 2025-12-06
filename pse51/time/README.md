# time — get time

## SYNOPSIS

```c
#include <time.h>

time_t time(time_t *tloc);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `time()` function shall return the value of time [CX] in seconds since the Epoch.

The `tloc` argument points to an area where the return value is also stored. If `tloc` is a null pointer, no value is stored.

## RETURN VALUE

Upon successful completion, `time()` shall return the value of time. Otherwise, `(time_t)-1` shall be returned.

## ERRORS

The `time()` function may fail if:

- **[EOVERFLOW]** [CX] The number of seconds since the Epoch will not fit in an object of type `time_t`.

*The following sections are informative.*

## EXAMPLES

### Getting the Current Time

The following example uses the `time()` function to calculate the time elapsed, in seconds, since the Epoch, `localtime()` to convert that value to a broken-down time, and `asctime()` to convert the broken-down time values into a printable string.

```c
#include <stdio.h>
#include <time.h>

int main(void)
{
    time_t result;

    result = time(NULL);
    printf("%s%ju secs since the Epoch\n",
        asctime(localtime(&result)),
            (uintmax_t)result);
    return(0);
}
```

This example writes the current time to `stdout` in a form like this:

```
Wed Jun 26 10:32:15 1996
835810335 secs since the Epoch
```

### Timing an Event

The following example gets the current time, prints it out in the user's format, and prints the number of minutes to an event being timed.

```c
#include <time.h>
#include <stdio.h>
...
time_t now;
int minutes_to_event;
...
time(&now);
minutes_to_event = ...;
printf("The time is ");
puts(asctime(localtime(&now)));
printf("There are %d minutes to the event.\n",
    minutes_to_event);
...
```

## APPLICATION USAGE

None.

## RATIONALE

The `time()` function returns a value in seconds while `clock_gettime()` returns a `struct timespec` (seconds and nanoseconds) and is therefore capable of returning more precise times. The `times()` function is also capable of more precision than `time()` as it returns a value in clock ticks, although it returns the elapsed time since an arbitrary point such as system boot time, not since the epoch.

Earlier versions of this standard allowed the width of `time_t` to be less than 64 bits. A 32-bit signed integer (as used in many historical implementations) fails in the year 2038, and although a 32-bit unsigned integer does not fail until 2106 the preferred solution is to make `time_t` wider rather than to make it unsigned.

On some systems the `time()` function is implemented using a system call that does not return an error condition in addition to the return value. On these systems it is impossible to differentiate between valid and invalid return values and hence overflow conditions cannot be reliably detected.

The use of the `<time.h>` header instead of `<sys/types.h>` allows compatibility with the ISO C standard.

Many historical implementations (including Version 7) and the 1984 /usr/group standard use `long` instead of `time_t`. This volume of POSIX.1-2024 uses the latter type in order to agree with the ISO C standard.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `asctime()`
- `clock()`
- `clock_getres()`
- `ctime()`
- `difftime()`
- `futimens()`
- `gmtime()`
- `localtime()`
- `mktime()`
- `strftime()`
- `strptime()`
- `times()`

XBD `<time.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

Extensions beyond the ISO C standard are marked.

The EXAMPLES, RATIONALE, and FUTURE DIRECTIONS sections are added.

### Issue 7

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0663 [106], XSH/TC1-2008/0664 [350], XSH/TC1-2008/0665 [106], XSH/TC1-2008/0666 [350], and XSH/TC1-2008/0667 [350] are applied.

### Issue 8

Austin Group Defect 1330 is applied, removing obsolescent interfaces.

Austin Group Defect 1462 is applied, changing the RATIONALE and FUTURE DIRECTIONS sections.

