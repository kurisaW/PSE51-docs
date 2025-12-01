# uname

## Name

uname - get system name

## Synopsis

```c
#include <sys/utsname.h>

int uname(struct utsname *name);
```

## Description

The `uname()` function shall store information identifying the current system in the structure pointed to by `name`.

The `uname()` function uses the **utsname** structure defined in `<sys/utsname.h>`.

The `uname()` function shall return a string naming the current system in the character array `sysname`. Similarly, `nodename` shall contain the name of this node within an implementation-defined communications network. The arrays `release` and `version` shall further identify the operating system. The array `machine` shall contain a name that identifies the hardware that the system is running on.

The format of each member is implementation-defined.

## Return Value

Upon successful completion, `uname()` shall return 0; otherwise, it shall return -1 and set `errno` to indicate the error.

## Errors

No errors are defined.

## Examples

### Example 1: Get System Information

```c
#include <stdio.h>
#include <sys/utsname.h>

int main(void)
{
    struct utsname name;

    if (uname(&name) == -1) {
        perror("uname");
        return 1;
    }

    printf("System name: %s\n", name.sysname);
    printf("Node name:   %s\n", name.nodename);
    printf("Release:     %s\n", name.release);
    printf("Version:     %s\n", name.version);
    printf("Machine:     %s\n", name.machine);

    return 0;
}
```

## Application Usage

The values of the structure members are not constrained to have any relation to the version of this volume of POSIX.1-2024 implemented in the operating system. An application should instead depend on `_POSIX_VERSION` and related constants defined in `<unistd.h>`.

This volume of POSIX.1-2024 does not define the sizes of the members of the structure and permits them to be of different sizes, although most implementations define them all to be the same size: eight bytes plus one byte for the string terminator. That size for `nodename` is not enough for use with many networks.

## Rationale

The `uname()` function originated in System III, System V, and related implementations, and it does not exist in Version 7 or 4.3 BSD. The values it returns are set at system compile time in those historical implementations.

4.3 BSD has `gethostname()` and `gethostid()`, which return a symbolic name and a numeric value, respectively. There are related `sethostname()` and `sethostid()` functions that are used to set the values the other two functions return. The former functions are included in this specification, the latter are not.

## Future Directions

None.

## See Also

- [`gethostname()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/gethostname.html)
- [`gethostid()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/gethostid.html)
- [`<sys/utsname.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_utsname.h.html)
- [`<unistd.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/unistd.h.html)

## Change History

* First released in Issue 1. Derived from System V documentation.

* Copyright © 2018-2024, The Open Group. All Rights Reserved.
 SPDX-License-Identifier: CC-BY-4.0