# qsort, qsort_r — sort a table of data

## SYNOPSIS

```c
#include <stdlib.h>

void qsort(void *base, size_t nel, size_t width,
           int (*compar)(const void *, const void *));

[CX] void qsort_r(void *base, size_t nel, size_t width,
                  int (*compar)(const void *, const void *, void *), void *arg);
```

## DESCRIPTION

For `qsort()`: [CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `qsort()` function shall sort an array of `nel` objects, the initial element of which is pointed to by `base`. The size of each object, in bytes, is specified by the `width` argument. If the `nel` argument has the value zero, the comparison function pointed to by `compar` shall not be called and no rearrangement shall take place.

The application shall ensure that the comparison function pointed to by `compar` does not alter the contents of the array. The implementation may reorder elements of the array between calls to the comparison function, but shall not alter the contents of any individual element.

When the same objects (consisting of width bytes, irrespective of their current positions in the array) are passed more than once to the comparison function, the results shall be consistent with one another. That is, they shall define a total ordering on the array.

The contents of the array shall be sorted in ascending order according to a comparison function. The `compar` argument is a pointer to the comparison function, which is called with two arguments that point to the elements being compared. The application shall ensure that the function returns an integer less than, equal to, or greater than 0, if the first argument is considered respectively less than, equal to, or greater than the second. If two members compare as equal, their order in the sorted array is unspecified.

[CX] The `qsort_r()` function shall be identical to `qsort()` except that the comparison function `compar` takes a third argument. The `arg` opaque pointer passed to `qsort_r()` shall in turn be passed as the third argument to the comparison function.

## RETURN VALUE

These functions shall not return a value.

## ERRORS

No errors are defined.

## APPLICATION USAGE

The comparison function need not compare every byte, so arbitrary data may be contained in the elements in addition to the values being compared.

If the `compar` callback function requires any additional state outside of the items being sorted, it can only access this state through global variables, making it potentially unsafe to use `qsort()` with the same `compar` function from separate threads at the same time. The `qsort_r()` function was added with the ability to pass through arbitrary arguments to the comparator, which avoids the need to access global variables and thus making it possible to safely share a stateful comparator across threads.

## RATIONALE

The requirement that each argument (hereafter referred to as `p`) to the comparison function is a pointer to elements of the array implies that for every call, for each argument separately, all of the following expressions are non-zero:

```c
((char *)p - (char *)base) % width == 0
(char *)p >= (char *)base
(char *)p < (char *)base + nel * width
```

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `alphasort()`
- `<stdlib.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

The normative text is updated to avoid use of the term "must" for application requirements.

IEEE Std 1003.1-2001/Cor 1-2002, item XSH/TC1/D6/49 is applied, adding the last sentence to the first non-shaded paragraph in the DESCRIPTION, and the following two paragraphs. The RATIONALE is also updated. These changes are for alignment with the ISO C standard.

### Issue 8

Austin Group Defect 900 is applied, adding the `qsort_r()` function.

---
