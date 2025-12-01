# shm_unlink — remove a shared memory object (REALTIME)

## SYNOPSIS

```c
#include <sys/mman.h>

int shm_unlink(const char *name);
```

## DESCRIPTION

The `shm_unlink()` function shall remove the name of the shared memory object named by the string pointed to by `name`.

If one or more references to the shared memory object exist when the object is unlinked, the name shall be removed before `shm_unlink()` returns, but the removal of the memory object contents shall be postponed until all open and map references to the shared memory object have been removed.

Even if the object continues to exist after the last `shm_unlink()`, reuse of the name shall subsequently cause `shm_open()` to behave as if no shared memory object of this name exists (that is, `shm_open()` shall fail if O_CREAT is not set, or shall create a new shared memory object if O_CREAT is set).

## RETURN VALUE

Upon successful completion, a value of zero shall be returned. Otherwise, a value of -1 shall be returned and `errno` set to indicate the error. If -1 is returned, the named shared memory object shall not be changed by this function call.

## ERRORS

The `shm_unlink()` function shall fail if:

- **[EACCES]** - Permission is denied to unlink the named shared memory object.
- **[ENOENT]** - The named shared memory object does not exist.

The `shm_unlink()` function may fail if:

- **[ENAMETOOLONG]** - The length of the `name` argument exceeds {_POSIX_PATH_MAX} on systems that do not support the XSI option or exceeds {_XOPEN_PATH_MAX} on XSI systems, or has a pathname component that is longer than {_POSIX_NAME_MAX} on systems that do not support the XSI option or longer than {_XOPEN_NAME_MAX} on XSI systems. A call to `shm_unlink()` with a `name` argument that contains the same shared memory object name as was previously used in a successful `shm_open()` call shall not give an [ENAMETOOLONG] error.

## EXAMPLES

None.

## APPLICATION USAGE

Names of memory objects that were allocated with `open()` are deleted with `unlink()` in the usual fashion. Names of memory objects that were allocated with `shm_open()` are deleted with `shm_unlink()`. Note that the actual memory object is not destroyed until the last close and unmap on it have occurred if it was already in use.

## RATIONALE

None.

## FUTURE DIRECTIONS

A future version might require the `shm_open()` and `shm_unlink()` functions to have semantics similar to normal file system operations.

## SEE ALSO

- `close()`
- `mmap()`
- `munmap()`
- `shmat()`
- `shmctl()`
- `shmdt()`
- `shm_open()`
- `<sys/mman.h>`

## CHANGE HISTORY

### First released in Issue 5
Included for alignment with the POSIX Realtime Extension.

### Issue 6
The `shm_unlink()` function is marked as part of the Shared Memory Objects option.

In the DESCRIPTION, text is added to clarify that reusing the same name after a `shm_unlink()` will not attach to the old shared memory object.

The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Shared Memory Objects option.

### Issue 7
Austin Group Interpretation 1003.1-2001 #077 is applied, changing [ENAMETOOLONG] from a "shall fail" to a "may fail" error.

Austin Group Interpretation 1003.1-2001 #141 is applied, adding FUTURE DIRECTIONS.

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*