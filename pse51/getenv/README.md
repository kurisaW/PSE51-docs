# getenv, secure_getenv — get value of an environment variable

## SYNOPSIS

```c
#include <stdlib.h>

char *getenv(const char *name);

[CX] char *secure_getenv(const char *name);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `getenv()` function shall search the environment of the calling process (see XBD 8. Environment Variables) for the environment variable *name* if it exists and return a pointer to the value of the environment variable. If the specified environment variable cannot be found, a null pointer shall be returned. The application shall ensure that it does not modify the string pointed to by the `getenv()` function, [CX] unless it is part of a modifiable object previously placed in the environment by assigning a new value to `environ` [XSI] or by using `putenv()`.

[CX] The pointer returned by `getenv()` shall point to a string within the environment data pointed to by `environ`.

> **Note:**
> This requirement is an extension to the ISO C standard, which allows `getenv()` to copy the data to an internal buffer.

The `secure_getenv()` function shall be equivalent to `getenv()`, except that it shall return a null pointer if the calling process does not meet all of the following security criteria:

1. The effective user ID and real user ID of the calling process were equal during program startup.
2. The effective group ID and real group ID of the calling process were equal during program startup.
3. Additional implementation-defined security criteria.

## RETURN VALUE

Upon successful completion, `getenv()` shall return a pointer to a string containing the value for the specified *name*. If the specified *name* cannot be found in the environment of the calling process, a null pointer shall be returned.

[CX] Upon successful completion, `secure_getenv()` shall return a pointer to a string containing the value for the specified *name*. If the specified *name* cannot be found in the environment of the calling process, or the calling process does not meet the security criteria listed in DESCRIPTION, a null pointer shall be returned.

## ERRORS

No errors are defined.

---

## EXAMPLES

### Getting the Value of an Environment Variable

The following example gets the value of the HOME environment variable.

```c
#include <stdlib.h>
...
const char *name = "HOME";
char *value;

value = getenv(name);
```

---

## APPLICATION USAGE

None.

---

## RATIONALE

The `clearenv()` function was considered but rejected. The `putenv()` function has now been included for alignment with the Single UNIX Specification.

Some earlier versions of this standard did not require `getenv()` to be thread-safe because it was allowed to return a value pointing to an internal buffer. However, this behavior allowed by the ISO C standard is no longer allowed by POSIX.1. POSIX.1 requires the environment data to be available through `environ[]`, so there is no reason why `getenv()` can't return a pointer to the actual data instead of a copy. Therefore `getenv()` is now required to be thread-safe (except when another thread modifies the environment).

Conforming applications are required not to directly modify the pointers to which `environ` points, but to use only the `setenv()`, `unsetenv()`, and `putenv()` functions, or assignment to `environ` itself, to manipulate the process environment. This constraint allows the implementation to properly manage the memory it allocates. This enables the implementation to free any space it has allocated to strings (and perhaps the pointers to them) stored in `environ` when `unsetenv()` is called. A C runtime start-up procedure (that which invokes `main()` and perhaps initializes `environ`) can also initialize a flag indicating that none of the environment has yet been copied to allocated storage, or that the separate table has not yet been initialized. If the application switches to a complete new environment by assigning a new value to `environ`, this can be detected by `getenv()`, `setenv()`, `unsetenv()`, or `putenv()` and the implementation can at that point reinitialize based on the new environment. (This may include copying the environment strings into a new array and assigning `environ` to point to it.)

In fact, for higher performance of `getenv()`, implementations that do not provide `putenv()` could also maintain a separate copy of the environment in a data structure that could be searched much more quickly (such as an indexed hash table, or a binary tree), and update both it and the linear list at `environ` when `setenv()` or `unsetenv()` is invoked. On implementations that do provide `putenv()`, such a copy might still be worthwhile but would need to allow for the fact that applications can directly modify the content of environment strings added with `putenv()`. For example, if an environment string found by searching the copy is one that was added using `putenv()`, the implementation would need to check that the string in `environ` still has the same name (and value, if the copy includes values), and whenever searching the copy produces no match the implementation would then need to search each environment string in `environ` that was added using `putenv()` in case any of them have changed their names and now match. Thus, each use of `putenv()` to add to the environment would reduce the speed advantage of having the copy.

Performance of `getenv()` can be important for applications which have large numbers of environment variables. Typically, applications like this use the environment as a resource database of user-configurable parameters. The fact that these variables are in the user's shell environment usually means that any other program that uses environment variables (such as `ls`, which attempts to use `COLUMNS`), or really almost any utility (`LANG`, `LC_ALL`, and so on) is similarly slowed down by the linear search through the variables.

An implementation that maintains separate data structures, or even one that manages the memory it consumes, is not currently required as it was thought it would reduce consensus among implementors who do not want to change their historical implementations.

---

## FUTURE DIRECTIONS

None.

---

## SEE ALSO

- [`exec()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html)
- [`putenv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/putenv.html)
- [`setenv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setenv.html)
- [`unsetenv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/unsetenv.html)

XBD 8. Environment Variables, `<stdlib.h>`

---

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 5

Normative text previously in the APPLICATION USAGE section is moved to the RETURN VALUE section.

A note indicating that this function need not be reentrant is added to the DESCRIPTION.

### Issue 6

The following changes were made to align with the IEEE P1003.1a draft standard:

- References added to the new `setenv()` and `unsetenv()` functions.

The normative text is updated to avoid use of the term "must" for application requirements.

### Issue 7

Austin Group Interpretation 1003.1-2001 #062 is applied, clarifying that a call to `putenv()` may also cause the string to be overwritten.

Austin Group Interpretation 1003.1-2001 #148 is applied, adding the FUTURE DIRECTIONS.

Austin Group Interpretation 1003.1-2001 #156 is applied.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0238 [75,428], XSH/TC1-2008/0239 [167], and XSH/TC1-2008/0240 [167] are applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0157 [656] is applied.

### Issue 8

Austin Group Defects 188 and 1394 are applied, changing `getenv()` to be thread-safe.

Austin Group Defect 922 is applied, adding the `secure_getenv()` function.

---
