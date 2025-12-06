# strcpy

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

---

## NAME

stpcpy, strcpy — copy a string

## SYNOPSIS

```c
#include <string.h>

[CX] char *stpcpy(char *restrict s1, const char *restrict s2);

char *strcpy(char *restrict s1, const char *restrict s2);
```

## DESCRIPTION

For `strcpy()`: [CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The [CX] `stpcpy()` and `strcpy()` functions shall copy the string pointed to by `s2` (including the terminating NUL character) into the array pointed to by `s1`.

If copying takes place between objects that overlap, the behavior is undefined.

[CX] The `strcpy()` and `stpcpy()` functions shall not change the setting of `errno` on valid input.

## RETURN VALUE

[CX] The `stpcpy()` function shall return a pointer to the terminating NUL character copied into the `s1` buffer.

The `strcpy()` function shall return `s1`.

No return values are reserved to indicate an error.

## ERRORS

No errors are defined.

---

*The following sections are informative.*

## EXAMPLES

### Construction of a Multi-Part Message in a Single Buffer

```c
#include <string.h>
#include <stdio.h>

int
main (void)
{
    char buffer[10];
    char *name = buffer;

    name = stpcpy (stpcpy (stpcpy (name, "ice"),"-"), "cream");
    puts (buffer);
    return 0;
}
```

### Initializing a String

The following example copies the string "----------" into the `permstring` variable.

```c
#include <string.h>
...
static char permstring[11];
...
strcpy(permstring, "----------");
...
```

### Storing a Key and Data

The following example allocates space for a key using `malloc()` then uses `strcpy()` to place the key there. Then it allocates space for data using `malloc()`, and uses `strcpy()` to place data there. (The user-defined function `dbfree()` frees memory previously allocated to an array of type `struct element *`.)

```c
#include <string.h>
#include <stdlib.h>
#include <stdio.h>
...
/* Structure used to read data and store it. */
struct element {
    char *key;
    char *data;
};

struct element *tbl, *curtbl;
char *key, *data;
int count;
...
void dbfree(struct element *, int);
...
if ((curtbl->key = malloc(strlen(key) + 1)) == NULL) {
    perror("malloc"); dbfree(tbl, count); return NULL;
}
strcpy(curtbl->key, key);

if ((curtbl->data = malloc(strlen(data) + 1)) == NULL) {
    perror("malloc"); free(curtbl->key); dbfree(tbl, count); return NULL;
}
strcpy(curtbl->data, data);
...
```

## APPLICATION USAGE

Character movement is performed differently in different implementations. Thus, overlapping moves may yield surprises.

This version is aligned with the ISO C standard; this does not affect compatibility with XPG3 applications. Reliable error detection by this function was never guaranteed.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `strncpy()`
- `wcscpy()`
- XBD `<string.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

The `strcpy()` prototype is updated for alignment with the ISO/IEC 9899:1999 standard.

### Issue 7

The `stpcpy()` function is added from The Open Group Technical Standard, 2006, Extended API Set Part 1.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `strcpy()` and `stpcpy()` do not change the setting of `errno` on valid input.

Austin Group Defect 1787 is applied, changing the NAME section.

---

