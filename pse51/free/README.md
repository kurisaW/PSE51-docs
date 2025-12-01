# free — free allocated memory

## SYNOPSIS

```c
#include <stdlib.h>

void free(void *ptr);
```

## DESCRIPTION

The `free()` function shall cause the space pointed to by `ptr` to be deallocated; that is, made available for further allocation. If `ptr` is a null pointer, no action shall occur. Otherwise, if the argument does not match a pointer earlier returned by `aligned_alloc()`, `calloc()`, `malloc()`, `posix_memalign()`, `realloc()`, `reallocarray()`, or a function in POSIX.1-2024 that allocates memory as if by `malloc()`, or if the space has been deallocated by a call to `free()`, `reallocarray()`, or `realloc()`, the behavior is undefined.

Any use of a pointer that refers to freed space results in undefined behavior.

The `free()` function shall not modify `errno` if `ptr` is a null pointer or a pointer previously returned as if by `malloc()` and not yet deallocated.

For purposes of determining the existence of a data race, `free()` shall behave as though it accessed only memory locations accessible through its argument and not other static duration storage. The function may, however, visibly modify the storage that it deallocates. Calls to `aligned_alloc()`, `calloc()`, `free()`, `malloc()`, `posix_memalign()`, `reallocarray()` and `realloc()` that allocate or deallocate a particular region of memory shall occur in a single total order, and each such deallocation call shall synchronize with the next allocation (if any) in this order.

## RETURN VALUE

The `free()` function shall not return a value.

## ERRORS

No errors are defined.

## EXAMPLES

None.

## APPLICATION USAGE

There is now no requirement for the implementation to support the inclusion of `<malloc.h>`.

Because the `free()` function does not modify `errno` for valid pointers, it is safe to use it in cleanup code without corrupting earlier errors, such as in this example code:

```c
// buf was obtained by malloc(buflen)
ret = write(fd, buf, buflen);
if (ret < 0) {
    free(buf);
    return ret;
}
```

However, earlier versions of this standard did not require this, and the same example had to be written as:

```c
// buf was obtained by malloc(buflen)
ret = write(fd, buf, buflen);
if (ret < 0) {
    int save = errno;
    free(buf);
    errno = save;
    return ret;
}
```

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `aligned_alloc()`
- `calloc()`
- `malloc()`
- `posix_memalign()`
- `realloc()`
- `<stdlib.h>`

## CHANGE HISTORY

### First released in Issue 1
Derived from Issue 1 of the SVID.

### Issue 6
Reference to the `valloc()` function is removed.

### Issue 7
The DESCRIPTION is updated to clarify that if the pointer returned is not by a function that allocates memory as if by `malloc()`, then the behavior is undefined.

### Issue 8
Austin Group Defect 385 is applied, adding a requirement that `free()` does not modify `errno` when passed a pointer to an object than can be freed.

Austin Group Defect 1218 is applied, adding `reallocarray()`.

Austin Group Defect 1302 is applied, aligning this function with the ISO/IEC 9899:2018 standard.

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*