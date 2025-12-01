# strtok, strtok_r — split string into tokens

## SYNOPSIS

```c
#include <string.h>

char *strtok(char *restrict s, const char *restrict sep);

[X] char *strtok_r(char *restrict s, const char *restrict sep,
                  char **restrict state);
```

## DESCRIPTION

For `strtok()`:
[X] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

A sequence of calls to `strtok()` breaks the string pointed to by `s` into a sequence of tokens, each of which is delimited by a byte from the string pointed to by `sep`. The first call in the sequence has `s` as its first argument, and is followed by calls with a null pointer as their first argument. The separator string pointed to by `sep` may be different from call to call.

The first call in the sequence searches the string pointed to by `s` for the first byte that is **not** contained in the current separator string pointed to by `sep`. If no such byte is found, then there are no tokens in the string pointed to by `s` and `strtok()` shall return a null pointer. If such a byte is found, it is the start of the first token.

The `strtok()` function then searches from there for a byte that **is** contained in the current separator string. If no such byte is found, the current token extends to the end of the string pointed to by `s`, and subsequent searches for a token shall return a null pointer. If such a byte is found, it is overwritten by a NUL character, which terminates the current token. The `strtok()` function saves a pointer to the following byte, from which the next search for a token shall start.

Each subsequent call, with a null pointer as the value of the first argument, starts searching from the saved pointer and behaves as described above.

The implementation shall behave as if no function defined in this volume of POSIX.1-2024 calls `strtok()`.

The `strtok()` function need not be thread-safe; however, `strtok()` shall avoid data races with all other functions.

[X] The `strtok_r()` function shall be equivalent to `strtok()`, except that `strtok_r()` shall be thread-safe and the argument `state` points to a user-provided pointer that allows `strtok_r()` to maintain state between calls which scan the same string. The application shall ensure that the pointer pointed to by `state` is unique for each string (`s`) being processed concurrently by `strtok_r()` calls. The application need not initialize the pointer pointed to by `state` to any particular value. The implementation shall not update the pointer pointed to by `state` to point (directly or indirectly) to resources, other than within the string `s`, that need to be freed or released by the caller.

[X] The `strtok()` and `strtok_r()` functions shall not change the setting of `errno` on valid input.

## RETURN VALUE

Upon successful completion, `strtok()` shall return a pointer to the first byte of a token. Otherwise, if there is no token, `strtok()` shall return a null pointer.

[X] The `strtok_r()` function shall return a pointer to the token found, or a null pointer when no token is found.

## ERRORS

No errors are defined.

## EXAMPLES

### Searching for Word Separators

The following example searches for tokens separated by space characters.

```c
#include <string.h>
...
char *token;
char line[] = "LINE TO BE SEPARATED";
char *search = " ";

/* Token will point to "LINE". */
token = strtok(line, search);

/* Token will point to "TO". */
token = strtok(NULL, search);
```

### Find First two Fields in a Buffer

The following example uses `strtok()` to find two character strings (a key and data associated with that key) separated by any combination of space, tab, or newline characters at the start of the array of characters pointed to by `buffer`.

```c
#include <string.h>
...
char *buffer;
...
struct element {
    char *key;
    char *data;
} e;
...
// Load the buffer...
...
// Get the key and its data...
e.key = strtok(buffer, " \t\n");
e.data = strtok(NULL, " \t\n");
// Process the rest of the contents of the buffer...
...
```

## APPLICATION USAGE

Note that if `sep` is the empty string, `strtok()` and `strtok_r()` return a pointer to the remainder of the string being tokenized.

The `strtok_r()` function is thread-safe and stores its state in a user-supplied buffer instead of possibly using a static data area that may be overwritten by an unrelated call from another thread.

## RATIONALE

The `strtok()` function searches for a separator string within a larger string. It returns a pointer to the last substring between separator strings. This function uses static storage to keep track of the current string position between calls. The new function, `strtok_r()`, takes an additional argument, `state`, to keep track of the current position in the string.

## FUTURE DIRECTIONS

None.

## SEE ALSO

XBD `<string.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 5

The `strtok_r()` function is included for alignment with the POSIX Threads Extension.

A note indicating that the `strtok()` function need not be reentrant is added to the DESCRIPTION.

### Issue 6

Extensions beyond the ISO C standard are marked.

The `strtok_r()` function is marked as part of the Thread-Safe Functions option.

In the DESCRIPTION, the note about reentrancy is expanded to cover thread-safety.

The APPLICATION USAGE section is updated to include a note on the thread-safe function and its avoidance of possibly using a static data area.

The `restrict` keyword is added to the `strtok()` and `strtok_r()` prototypes for alignment with the ISO/IEC 9899:1999 standard.

### Issue 7

Austin Group Interpretation 1003.1-2001 #156 is applied.

SD5-XSH-ERN-235 is applied, correcting an example.

The `strtok_r()` function is moved from the Thread-Safe Functions option to the Base.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0615 [177] is applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0350 [878] is applied.

### Issue 8

Austin Group Defect 448 is applied, adding a requirement that `strtok()` and `strtok_r()` do not change the setting of `errno` on valid input.

Austin Group Defect 1302 is applied, aligning the `strtok()` function with the ISO/IEC 9899:2018 standard.