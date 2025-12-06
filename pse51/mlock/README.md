# mlock, munlock — lock or unlock a range of process address space (REALTIME)

## SYNOPSIS

```c
#include <sys/mman.h>

int mlock(const void *addr, size_t len);
int munlock(const void *addr, size_t len);
```

## DESCRIPTION

The `mlock()` function shall cause those whole pages containing any part of the address space of the process starting at address `addr` and continuing for `len` bytes to be memory-resident until unlocked or until the process exits or `exec`s another process image. The implementation may require that `addr` be a multiple of {PAGESIZE}.

The `munlock()` function shall unlock those whole pages containing any part of the address space of the process starting at address `addr` and continuing for `len` bytes, regardless of how many times `mlock()` has been called by the process for any of the pages in the specified range. The implementation may require that `addr` be a multiple of {PAGESIZE}.

If any of the pages in the range specified to a call to `munlock()` are also mapped into the address spaces of other processes, any locks established on those pages by another process are unaffected by the call of this process to `munlock()`. If any of the pages in the range specified by a call to `munlock()` are also mapped into other portions of the address space of the calling process outside the range specified, any locks established on those pages via the other mappings are also unaffected by this call.

Upon successful return from `mlock()`, pages in the specified range shall be locked and memory-resident. Upon successful return from `munlock()`, pages in the specified range shall be unlocked with respect to the address space of the process. Memory residency of unlocked pages is unspecified.

Appropriate privileges are required to lock process memory with `mlock()`.

## RETURN VALUE

Upon successful completion, the `mlock()` and `munlock()` functions shall return a value of zero. Otherwise, no change is made to any locks in the address space of the process, and the function shall return a value of -1 and set `errno` to indicate the error.

## ERRORS

The `mlock()` and `munlock()` functions shall fail if:

- **[ENOMEM]**
  Some or all of the address range specified by the `addr` and `len` arguments does not correspond to valid mapped pages in the address space of the process.

The `mlock()` function shall fail if:

- **[EAGAIN]**
  Some or all of the memory identified by the operation could not be locked when the call was made.

The `mlock()` and `munlock()` functions may fail if:

- **[EINVAL]**
  The `addr` argument is not a multiple of {PAGESIZE}.

The `mlock()` function may fail if:

- **[ENOMEM]**
  Locking the pages mapped by the specified range would exceed an implementation-defined limit on the amount of memory that the process may lock.

- **[EPERM]**
  The calling process does not have appropriate privileges to perform the requested operation.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`exec`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html)
- [`_exit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exit.html)
- [`fork()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fork.html)
- [`mlockall()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/mlockall.html)
- [`munmap()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/munmap.html)
- [`<sys/mman.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_mman.h.html)

## CHANGE HISTORY

First released in Issue 5. Included for alignment with the POSIX Realtime Extension.

### Issue 6

The `mlock()` and `munlock()` functions are marked as part of the Range Memory Locking option.

The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Range Memory Locking option.

---
