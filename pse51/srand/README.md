# rand, srand — pseudo-random number generator

## SYNOPSIS

```c
#include <stdlib.h>

int rand(void);
void srand(unsigned seed);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `rand()` function shall compute a sequence of pseudo-random integers in the range [0,{RAND_MAX}] [XSI] with a period of at least 2^32.

The `rand()` function need not be thread-safe; however, `rand()` shall avoid data races with all functions other than non-thread-safe pseudo-random sequence generation functions.

The `srand()` function uses the argument as a seed for a new sequence of pseudo-random numbers to be returned by subsequent calls to `rand()`. If `srand()` is then called with the same seed value, the sequence of pseudo-random numbers shall be repeated. If `rand()` is called before any calls to `srand()` are made, the same sequence shall be generated as when `srand()` is first called with a seed value of 1.

The `srand()` function need not be thread-safe; however, `srand()` shall avoid data races with all functions other than non-thread-safe pseudo-random sequence generation functions.

The implementation shall behave as if no function defined in this volume of POSIX.1-2024 calls `rand()` or `srand()`.

## RETURN VALUE

The `rand()` function shall return the next pseudo-random number in the sequence.

The `srand()` function shall not return a value.

## ERRORS

No errors are defined.

## EXAMPLES

### Generating a Pseudo-Random Number Sequence

The following example demonstrates how to generate a sequence of pseudo-random numbers.

```c
#include <stdio.h>
#include <stdlib.h>
...
    long count, i;
    char *keystr;
    int elementlen, len;
    char c;
...
/* Initial random number generator. */
    srand(1);

    /* Create keys using only lowercase characters */
    len = 0;
    for (i=0; i<count; i++) {
        while (len < elementlen) {
            c = (char) (rand() % 128);
            if (islower(c))
                keystr[len++] = c;
        }

        keystr[len] = '\0';
        printf("%s Element%0*ld\n", keystr, elementlen, i);
        len = 0;
    }
```

### Generating the Same Sequence on Different Machines

The following code defines a pair of functions that could be incorporated into applications wishing to ensure that the same sequence of numbers is generated across different machines.

```c
static unsigned long next = 1;

int myrand(void)  /* RAND_MAX assumed to be 32767. */
{
    next = next * 1103515245 + 12345;
    return((unsigned)(next/65536) % 32768);
}

void mysrand(unsigned seed)
{
    next = seed;
}
```

## APPLICATION USAGE

These functions should be avoided whenever non-trivial requirements (including safety) have to be fulfilled, unless seeded using `getentropy()`.

The `drand48()` and `random()` functions provide much more elaborate pseudo-random number generators.

## RATIONALE

The ISO C standard `rand()` and `srand()` functions allow per-process pseudo-random streams shared by all threads. Those two functions need not change, but there has to be mutual-exclusion that prevents interference between two threads concurrently accessing the random number generator.

With regard to `rand()`, there are two different behaviors that may be wanted in a multi-threaded program:

1. A single per-process sequence of pseudo-random numbers that is shared by all threads that call `rand()`
2. A different sequence of pseudo-random numbers for each thread that calls `rand()`

This is provided by the modified thread-safe function based on whether the seed value is global to the entire process or local to each thread.

This does not address the known deficiencies of the `rand()` function implementations, which have been approached by maintaining more state. In effect, this specifies new thread-safe forms of a deficient function.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `drand48()`
- `getentropy()`
- `initstate()`
- `<stdlib.h>`

## CHANGE HISTORY

### First released in Issue 1
Derived from Issue 1 of the SVID.

### Issue 5
- The `rand_r()` function is included for alignment with the POSIX Threads Extension.
- A note indicating that the `rand()` function need not be reentrant is added to the DESCRIPTION.

### Issue 6
- Extensions beyond the ISO C standard are marked.
- The `rand_r()` function is marked as part of the Thread-Safe Functions option.

### Issue 7
- Austin Group Interpretation 1003.1-2001 #156 is applied.
- The `rand_r()` function is marked obsolescent.
- POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0301 [743] is applied.

### Issue 8
- Austin Group Defect 1134 is applied, adding `getentropy()`.
- Austin Group Defect 1302 is applied, aligning these functions with the ISO/IEC 9899:2018 standard.
- Austin Group Defect 1330 is applied, removing obsolescent interfaces.

---

*UNIX® is a registered Trademark of The Open Group.*
*POSIX™ is a Trademark of The IEEE.*
*Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved*