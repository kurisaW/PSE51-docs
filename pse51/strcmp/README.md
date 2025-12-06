# strcmp — compare two strings

## SYNOPSIS

```c
#include <string.h>

int strcmp(const char *s1, const char *s2);
```

## DESCRIPTION

The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `strcmp()` function shall compare the string pointed to by `s1` to the string pointed to by `s2`.

The sign of a non-zero return value shall be determined by the sign of the difference between the values of the first pair of bytes (both interpreted as type `unsigned char`) that differ in the strings being compared.

The `strcmp()` function shall not change the setting of `errno` on valid input.

## RETURN VALUE

Upon completion, `strcmp()` shall return an integer greater than, equal to, or less than 0, if the string pointed to by `s1` is greater than, equal to, or less than the string pointed to by `s2`, respectively.

## ERRORS

No errors are defined.

---

## EXAMPLES

### Checking a Password Entry

The following example compares the information read from standard input to the value of the name of the user entry. If the `strcmp()` function returns 0 (indicating a match), a further check will be made to see if the user entered the proper old password. The `crypt()` function shall encrypt the old password entered by the user, using the value of the encrypted password in the `passwd` structure as the salt. If this value matches the value of the encrypted `passwd` in the structure, the entered password `oldpasswd` is the correct user's password. Finally, the program encrypts the new password so that it can store the information in the `passwd` structure.

```c
#include <string.h>
#include <unistd.h>
#include <stdio.h>
...
int valid_change;
struct passwd *p;
char user[100];
char oldpasswd[100];
char newpasswd[100];
char savepasswd[100];
...
if (strcmp(p->pw_name, user) == 0) {
    if (strcmp(p->pw_passwd, crypt(oldpasswd, p->pw_passwd)) == 0) {
        strcpy(savepasswd, crypt(newpasswd, user));
        p->pw_passwd = savepasswd;
        valid_change = 1;
    }
    else {
        fprintf(stderr, "Old password is not valid\n");
    }
}
...
```

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`strncmp()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strncmp.html)
- [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

Extensions beyond the ISO C standard are marked.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `strcmp()` does not change the setting of `errno` on valid input.

---
