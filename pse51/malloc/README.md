# malloc — a memory allocator

## SYNOPSIS

```c
#include <stdlib.h>

void *malloc(size_t size);
```

## DESCRIPTION

The `malloc()` function shall allocate unused space for an object whose size in bytes is specified by `size` and whose value is unspecified.

The order and contiguity of storage allocated by successive calls to `malloc()` is unspecified. The pointer returned if the allocation succeeds shall be suitably aligned so that it may be assigned to a pointer to any type of object with a fundamental alignment requirement and then used to access such an object in the space allocated (until the space is explicitly freed or reallocated). Each such allocation shall yield a pointer to an object disjoint from any other object. The pointer returned points to the start (lowest byte address) of the allocated space. If the space cannot be allocated, a null pointer shall be returned. If the size of the space requested is 0, the behavior is implementation-defined: either a null pointer shall be returned, or the behavior shall be as if the size were some non-zero value, except that the behavior is undefined if the returned pointer is used to access an object.

For purposes of determining the existence of a data race, `malloc()` shall behave as though it accessed only memory locations accessible through its argument and not other static duration storage. The function may, however, visibly modify the storage that it allocates. Calls to `aligned_alloc()`, `calloc()`, `free()`, `malloc()`, `posix_memalign()`, `reallocarray()`, and `realloc()` that allocate or deallocate a particular region of memory shall occur in a single total order (see [4.15.1 Memory Ordering](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap04.html#tag_04_15_01)), and each such deallocation call shall synchronize with the next allocation (if any) in this order.

## RETURN VALUE

Upon successful completion, `malloc()` shall return a pointer to the allocated space; if `size` is 0, the application shall ensure that the pointer is not used to access an object.

Otherwise, it shall return a null pointer and set `errno` to indicate the error.

## ERRORS

The `malloc()` function shall fail if:

- **[ENOMEM]** Insufficient storage space is available.

The `malloc()` function may fail if:

- **[EINVAL]** `size` is 0 and the implementation does not support 0 sized allocations.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

Some implementations set `errno` to [EAGAIN] to signal memory allocation failures that might succeed if retried and [ENOMEM] for failures that are unlikely to ever succeed, for example due to configured limits. [2.3 Error Numbers](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_03) permits this behavior; when multiple error conditions are simultaneously true there is no precedence between them.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`aligned_alloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/aligned_alloc.html)
- [`calloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/calloc.html)
- [`free()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/free.html)
- [`getrlimit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getrlimit.html)
- [`posix_memalign()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_memalign.html)
- [`realloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/realloc.html)

XBD [`<stdlib.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

Extensions beyond the ISO C standard are marked.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

- In the RETURN VALUE section, the requirement to set `errno` to indicate an error is added.
- The [ENOMEM] error condition is added.

### Issue 7

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0203 [526] is applied.

### Issue 8

Austin Group Defect 374 is applied, changing the RETURN VALUE and ERRORS sections in relation to 0 sized allocations.

Austin Group Defect 1302 is applied, aligning this function with the ISO/IEC 9899:2018 standard.

Austin Group Defects 1387 and 1489 are applied, changing the RATIONALE section.

---
