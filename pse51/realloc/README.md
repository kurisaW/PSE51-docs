# realloc, reallocarray — memory reallocators

## SYNOPSIS

```c
#include <stdlib.h>

void *realloc(void *ptr, size_t size);

[CX] void *reallocarray(void *ptr, size_t nelem, size_t elsize);
```

## DESCRIPTION

For `realloc()`: [CX] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

The `realloc()` function shall deallocate the old object pointed to by `ptr` and return a pointer to a new object that has the size specified by `size`. The contents of the new object shall be the same as that of the old object prior to deallocation, up to the lesser of the new and old sizes. Any bytes in the new object beyond the size of the old object have indeterminate values.

[CX] The `reallocarray()` function shall be equivalent to the call `realloc(ptr, nelem * elsize)` except that overflow in the multiplication shall be an error.

If `ptr` is a null pointer, `realloc()` [CX] or `reallocarray()` shall be equivalent to `malloc()` for the specified size. Otherwise, if `ptr` does not match a pointer earlier returned by `aligned_alloc()`, `calloc()`, `malloc()`, [ADV] `posix_memalign()`, `realloc()`, [CX] `reallocarray()`, or a function in POSIX.1-2024 that allocates memory as if by `malloc()`, or if the space has been deallocated by a call to `free()`, [CX] `reallocarray()`, or `realloc()`, the behavior is undefined.

If `size` is non-zero and memory for the new object is not allocated, the old object shall not be deallocated.

The order and contiguity of storage allocated by successive calls to `realloc()` [CX] or `reallocarray()` is unspecified. The pointer returned if the allocation succeeds shall be suitably aligned so that it may be assigned to a pointer to any type of object with a fundamental alignment requirement and then used to access such an object in the space allocated (until the space is explicitly freed or reallocated). Each such allocation shall yield a pointer to an object disjoint from any other object. The pointer returned shall point to the start (lowest byte address) of the allocated space. If the space cannot be allocated, a null pointer shall be returned.

For purposes of determining the existence of a data race, `realloc()` [CX] and `reallocarray()` shall each behave as though it accessed only memory locations accessible through its argument and not other static duration storage. The function may, however, visibly modify the storage that it allocates. Calls to `aligned_alloc()`, `calloc()`, `free()`, `malloc()`, [ADV] `posix_memalign()`, [CX] `reallocarray()`, and `realloc()` that allocate or deallocate a particular region of memory shall occur in a single total order (see 4.15.1 Memory Ordering), and each such deallocation call shall synchronize with the next allocation (if any) in this order.

## RETURN VALUE

Upon successful completion, `realloc()` [CX] and `reallocarray()` shall return a pointer to the new object (which can have the same value as a pointer to the old object), or a null pointer if the new object has not been allocated.

[OB] If `size` is 0, [OB CX] or either `nelem` or `elsize` is 0, [OB] either:

If there is not enough available memory, `realloc()` [CX] and `reallocarray()` shall return a null pointer [CX] and set `errno` to [ENOMEM].

## ERRORS

The `realloc()` [CX] and `reallocarray()` functions shall fail if:

**[ENOMEM]**
[CX] Insufficient memory is available.

[CX] The `reallocarray()` function shall fail if:

**[ENOMEM]**
The calculation `nelem * elsize` would overflow.

The `realloc()` [CX] and `reallocarray()` functions may fail if:

**[EINVAL]**
[CX] The requested allocation size is 0 and the implementation does not support 0 sized allocations.

## EXAMPLES

None.

## APPLICATION USAGE

The ISO C standard makes it implementation-defined whether a call to `realloc(p, 0)` frees the space pointed to by p if it returns a null pointer because memory for the new object was not allocated. POSIX.1 instead requires that implementations set `errno` if a null pointer is returned and the space has not been freed, and POSIX applications should only free the space if `errno` was changed.

## RATIONALE

See the RATIONALE for `malloc()`.

## FUTURE DIRECTIONS

The ISO C standard states that invoking `realloc()` with a `size` argument equal to zero is an obsolescent feature. This feature may be removed in a future version of this standard.

## SEE ALSO

- `aligned_alloc()`
- `calloc()`
- `free()`
- `malloc()`
- `<stdlib.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

Extensions beyond the ISO C standard are marked.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

- In the RETURN VALUE section, if there is not enough available memory, the setting of `errno` to [ENOMEM] is added.
- The [ENOMEM] error condition is added.

### Issue 7

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0495 [400], XSH/TC1-2008/0496 [400], XSH/TC1-2008/0497 [400], and XSH/TC1-2008/0498 [400] are applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0309 [526] and XSH/TC2-2008/0310 [526,688] are applied.

### Issue 8

Austin Group Defect 374 is applied, adding the [EINVAL] error.

Austin Group Defect 1218 is applied, adding `reallocarray()`.

Austin Group Defect 1302 is applied, aligning the `realloc()` function with the ISO/IEC 9899:2018 standard.

Austin Group Defect 1387 is applied, changing the RATIONALE section.

---
