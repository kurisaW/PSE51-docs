# strncpy

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

---

## NAME

stpncpy, strncpy — copy fixed length string, returning a pointer to the array end

## SYNOPSIS

```c
#include <string.h>

[CX] char *stpncpy(char *restrict s1, const char *restrict s2, size_t n);
char *strncpy(char *restrict s1, const char *restrict s2, size_t n);
```

## DESCRIPTION

For strncpy(): [CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The [CX] stpncpy() and strncpy() functions shall copy not more than n bytes (bytes that follow a NUL character are not copied) from the array pointed to by s2 to the array pointed to by s1.

If the array pointed to by s2 is a string that is shorter than n bytes, NUL characters shall be appended to the copy in the array pointed to by s1, until n bytes in all are written.

If copying takes place between objects that overlap, the behavior is undefined.

[CX] The strncpy() and stpncpy() functions shall not change the setting of errno on valid input.

## RETURN VALUE

[CX] If a NUL character is written to the destination, the stpncpy() function shall return the address of the first such NUL character. Otherwise, it shall return &s1[n].

The strncpy() function shall return s1.

No return values are reserved to indicate an error.

## ERRORS

No errors are defined.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

Applications must provide the space in s1 for the n bytes to be transferred, as well as ensure that the s2 and s1 arrays do not overlap.

Character movement is performed differently in different implementations. Thus, overlapping moves may yield surprises.

If there is no NUL character byte in the first n bytes of the array pointed to by s2, the result is not null-terminated.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- strcpy()
- strlcat()
- wcsncpy()

XBD <string.h>

## CHANGE HISTORY

**First released in Issue 1.** Derived from Issue 1 of the SVID.

### Issue 6

The strncpy() prototype is updated for alignment with the ISO/IEC 9899:1999 standard.

### Issue 7

The stpncpy() function is added from The Open Group Technical Standard, 2006, Extended API Set Part 1.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that strncpy() and stpncpy() do not change the setting of errno on valid input.

Austin Group Defect 986 is applied, adding strlcat() to the SEE ALSO section.

