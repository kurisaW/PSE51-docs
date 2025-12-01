# rand, rand_r, srand - pseudo-random number generator

## SYNOPSIS

```c
#include <stdlib.h>

int rand(void);

[OB CX] int rand_r(unsigned *seed);

void srand(unsigned seed);
```

## DESCRIPTION

For `rand()` and `srand()`: [CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2017 defers to the ISO C standard.

The `rand()` function shall compute a sequence of pseudo-random integers in the range [0, {RAND_MAX}] [XSI] with a period of at least 2^32.

[CX] The `rand()` function need not be thread-safe.

[OB CX] The `rand_r()` function shall compute a sequence of pseudo-random integers in the range [0, {RAND_MAX}]. (The value of the {RAND_MAX} macro shall be at least 32767.)

If `rand_r()` is called with the same initial value for the object pointed to by `seed` and that object is not modified between successive returns and calls to `rand_r()`, the same sequence shall be generated.

The `srand()` function uses the argument as a seed for a new sequence of pseudo-random numbers to be returned by subsequent calls to `rand()`. If `srand()` is then called with the same seed value, the sequence of pseudo-random numbers shall be repeated. If `rand()` is called before any calls to `srand()` are made, the same sequence shall be generated as when `srand()` is first called with a seed value of 1.

The implementation shall behave as if no function defined in this volume of POSIX.1-2017 calls `rand()` or `srand()`.

## RETURN VALUE

The `rand()` function shall return the next pseudo-random number in the sequence.

[OB CX] The `rand_r()` function shall return a pseudo-random integer.

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

The `drand48()` and `random()` functions provide much more elaborate pseudo-random number generators.

The limitations on the amount of state that can be carried between one function call and another mean the `rand_r()` function can never be implemented in a way which satisfies all of the requirements on a pseudo-random number generator.

These functions should be avoided whenever non-trivial requirements (including safety) have to be fulfilled.

## RATIONALE

The ISO C standard `rand()` and `srand()` functions allow per-process pseudo-random streams shared by all threads. Those two functions need not change, but there has to be mutual-exclusion that prevents interference between two threads concurrently accessing the random number generator.

With regard to `rand()`, there are two different behaviors that may be wanted in a multi-threaded program:

1. A single per-process sequence of pseudo-random numbers that is shared by all threads that call `rand()`
2. A different sequence of pseudo-random numbers for each thread that calls `rand()`

This is provided by the modified thread-safe function based on whether the seed value is global to the entire process or local to each thread.

This does not address the known deficiencies of the `rand()` function implementations, which have been approached by maintaining more state. In effect, this specifies new thread-safe forms of a deficient function.

## FUTURE DIRECTIONS

The `rand_r()` function may be removed in a future version.

## SEE ALSO

`drand48()`, `initstate()`

XBD `<stdlib.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 5

The `rand_r()` function is included for alignment with the POSIX Threads Extension.

A note indicating that the `rand()` function need not be reentrant is added to the DESCRIPTION.

### Issue 6

Extensions beyond the ISO C standard are marked.

The `rand_r()` function is marked as part of the Thread-Safe Functions option.

### Issue 7

Austin Group Interpretation 1003.1-2001 #156 is applied.

The `rand_r()` function is marked obsolescent.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0301 [743] is applied.