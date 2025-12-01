# calloc — a memory allocator

## SYNOPSIS

```c
#include <stdlib.h>

void *calloc(size_t nelem, size_t elsize);
```

## DESCRIPTION

[CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `calloc()` function shall allocate unused space for an array of `nelem` elements each of whose size in bytes is `elsize`. The space shall be initialized to all bits 0.

The order and contiguity of storage allocated by successive calls to `calloc()` is unspecified. The pointer returned if the allocation succeeds shall be suitably aligned so that it may be assigned to a pointer to any type of object with a fundamental alignment requirement and then used to access such an object or an array of such objects in the space allocated (until the space is explicitly freed or reallocated). Each such allocation shall yield a pointer to an object disjoint from any other object. The pointer returned shall point to the start (lowest byte address) of the allocated space. If the space cannot be allocated, a null pointer shall be returned. If the size of the space requested is 0, the behavior is implementation-defined: either a null pointer shall be returned, or the behavior shall be as if the size were some non-zero value, except that the behavior is undefined if the returned pointer is used to access an object.

For purposes of determining the existence of a data race, `calloc()` shall behave as though it accessed only memory locations accessible through its arguments and not other static duration storage. The function may, however, visibly modify the storage that it allocates. Calls to `aligned_alloc()`, `calloc()`, `free()`, `malloc()`, [ADV] `posix_memalign()`, [CX] `reallocarray()`, and `realloc()` that allocate or deallocate a particular region of memory shall occur in a single total order (see 4.15.1 Memory Ordering), and each such deallocation call shall synchronize with the next allocation (if any) in this order.

## RETURN VALUE

Upon successful completion, `calloc()` shall return a pointer to the allocated space; if either `nelem` or `elsize` is 0, the application shall ensure that the pointer is not used to access an object.

Otherwise, it shall return a null pointer [CX] and set `errno` to indicate the error.

## ERRORS

The `calloc()` function shall fail if:

- **[ENOMEM]** [CX] Insufficient memory is available, including the case when `nelem` * `elsize` would overflow.

The `calloc()` function may fail if:

- **[EINVAL]** [CX] `nelem` or `elsize` is 0 and the implementation does not support 0 sized allocations.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

There is now no requirement for the implementation to support the inclusion of `<malloc.h>`.

## RATIONALE

See the RATIONALE for `malloc()`.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`aligned_alloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/aligned_alloc.html)
- [`free()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/free.html)
- [`malloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/malloc.html)
- [`realloc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/realloc.html)

XBD [`<stdlib.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

Extensions beyond the ISO C standard are marked.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

- The setting of `errno` and the [ENOMEM] error condition are mandatory if an insufficient memory condition occurs.

### Issue 7

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0053 [526] is applied.

### Issue 8

Austin Group Defect 374 is applied, changing the RETURN VALUE and ERRORS sections in relation to 0 sized allocations.

Austin Group Defect 1218 is applied, changing the [ENOMEM] error.

Austin Group Defect 1302 is applied, aligning this function with the ISO/IEC 9899:2018 standard.

Austin Group Defect 1387 is applied, changing the RATIONALE section.

---

*End of informative text.*

---

**The Open Group Base Specifications Issue 8**
**IEEE Std 1003.1-2024**
Copyright © 2001-2024 The IEEE and The Open Group

UNIX® is a registered Trademark of The Open Group.
POSIX™ is a Trademark of The IEEE.
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved